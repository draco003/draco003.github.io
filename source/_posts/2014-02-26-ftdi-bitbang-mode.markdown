---
layout: post
title: "FTDI bitbang Mode"
date: 2013-03-28 17:43:03 +0200
comments: true
categories: FTDI, FreeBSD
description: HOWTO FTDI Bitbang mode on FreeBSD using libftdi API
keywords: freebsd,bitbang,ftdi,ftdi bitbang,libftdi,ftdi libftdi bitbang,serial port 
---
I'd like to give a simple introduction on using FTDI chips in bitbang mode based on this post on [Hack a Day](http://hackaday.com/2009/09/22/introduction-to-ftdi-bitbang-mode/).

For the hardware we will be using the ["Breakout Board for FT232RL USB to Serial"](http://www.sparkfun.com/products/718 "Product Page") from Sparkfun, of course you can use any flavor of FTDI ICs.
<!-- more -->

Also we will need to get the libftdi devel/libftdi if you don't have it already.
``` bash
# cd /usr/ports/devel/libftdi
# make install clean
```

The documentation is very helpful when working with `libftdi` API in C++: [libftdi docs](http://www.intra2net.com/en/developer/libftdi/documentation/group__libftdi.html "Library Documentation").

When you connect the `FT232R` breakout board via USB, you should be able to see your device as a virtual serial port in my case: `/dev/cuaU0` and it was identified as a ugen1.2 device.
``` bash
% dmesg | tail
ugen1.2: < FTDI > at usbus1
uftdi0: < FT232R USB UART > on usbus1
```

Next let's try the LED blinker example by [Phil](http://hackaday.com/author/philburgess/ "Phil's Profile") at Hack a Day.

I made some changes to the C++ code to be compatible with the latest `libftdi 0.20`

We will need an LED and a 330 Ohm resistor. Connect the Anode of LED (long lead +ve) to CTS pin on FTDI breakout, and the Cathode (short lead -ve) to the resistor, and the other lead of the resistor will be connected to GND.

Put the following code in a file named `hello_ftdi.c`
``` c hello_ftdi.c
/* hello_ftdi.c: flash LED connected between CTS and GND.
This example uses the libftdi API.
Minimal error checking; written for brevity, not durability. */

#include <stdio.h>
#include <ftdi.h>

#define LED 0x08 /* CTS */

int main()
{
unsigned char c = 0;
struct ftdi_context ftdic;

/* Initialize context for subsequent function calls */
ftdi_init(&ftdic);

/* Open FTDI device based on FT232R vendor & product IDs */
if(ftdi_usb_open(&ftdic, 0x0403, 0x6001) < 0) {
puts("Can't open device");
return 1;
}

/* Enable bitbang mode with a single output line */
ftdi_set_bitmode(&ftdic, LED, BITMODE_BITBANG);

/* Endless loop: invert LED state, write output, pause 1 second */
for(;;) {
c ^= LED;
ftdi_write_data(&ftdic, &c, 1);
sleep(1);
}
}
```

To compile the above code we use the following command:
``` bash
# gcc -I/usr/local/include/ -L/usr/local/lib/ -o blink hello_ftdi.c -lftdi
```
This will generate an executable named `hello`, run it
``` bash
% ./hello
```

You should see the LED blinking on and off with a period of 1 second.

We might need this pin mapping for the FTDI pins in future programming:
``` c
/*
* bitbang I/O pin mappings 
* 
* #define PIN_TXD 0x01
* #define PIN_RXD 0x02
* #define PIN_RTS 0x04
* #define PIN_CTS 0x08
* #define PIN_DTR 0x10
* #define PIN_DSR 0x20
* #define PIN_DCD 0x40
* #define PIN_RI 0x80
*/
```
For now you can have a look on Phil's post on Hack a Day he explains the code pretty well, the code above is modified and works well with libftdi 0.20, also I'll post here the PWM LED chaser code in that other article it's written using D2XX API instead. So I'll post below a modified version to work with libftdi API.

The hardware setup includes 4 LEDs and 4 330 Ohm resistors, I happened to use an LED bar that I had laying around.
``` c pwmchase.c
/* pwmchase.c: 8-bit PWM on 4 LEDs using FTDI cable or breakout.
This example uses the libftdi API.
Minimal error checking; written for brevity, not durability. */

#include <stdio.h>
#include <string.h>
#include <math.h>
#include <ftdi.h>

#define LED1 0x08 /* CTS */
#define LED2 0x01 /* TXD */
#define LED3 0x02 /* RXD */
#define LED4 0x14 /* RTS + DTR */

int main()
{
int i,n;
unsigned char data[255 * 256];
struct ftdi_context ftdic;

/* Generate data for a single PWM 'throb' cycle */
memset(data, 0, sizeof(data));
for(i=1; i<128; i++) {
/* Apply gamma correction to PWM brightness */
n = (int)(pow((double)i / 127.0, 2.5) * 255.0);
memset(&data[i * 255], LED1, n); /* Ramp up */
memset(&data[(256 - i) * 255], LED1, n); /* Ramp down */
}

/* Copy data from first LED to others, offset as appropriate */
n = sizeof(data) / 4;
for(i=0; i<sizeof(data); i++)
{
if(data[i] & LED1) {
data[(i + n ) % sizeof(data)] |= LED2;
data[(i + n * 2) % sizeof(data)] |= LED3;
data[(i + n * 3) % sizeof(data)] |= LED4;
}
}

/* Initialize context for subsequent function calls */
ftdi_init(&ftdic);

/* Open FTDI device based on FT232R vendor & product IDs */
if(ftdi_usb_open(&ftdic, 0x0403, 0x6001) < 0) {
puts("Can't open device");
return 1;
}

/* Initialize, open device, set bitbang mode w/5 outputs */

ftdi_set_bitmode(&ftdic, LED1 | LED2 | LED3 | LED4, BITMODE_BITBANG); 
ftdi_set_baudrate(&ftdic, 9600); /* Actually 9600 * 16 */

/* Endless loop: dump precomputed PWM data to the device */
for(;;) ftdi_write_data(&ftdic, data, sizeof(data));
}
```

To compile run:
``` bash
# gcc -I/usr/local/include/ -L/usr/local/lib/ -o pwm pwmchase.c -lftdi -lm
```

then to run:
``` bash
% ./pwm
```

You should now see the 4 LEDs chasing each other with the PWM (Pulse Width Modulation) effect.

I'm currently working on connecting a 20x4 LCD display, and will have it show some data. Will keep you posted.

I hope this would be of any help to you, Have fun!
