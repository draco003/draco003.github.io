---
layout: post
title: "CodeIgniter Pagination Integrated with Bootstrap"
date: 2013-10-07 22:35:24 +0200
comments: true
categories: CodeIgniter
description: CodeIgniter Pagination Integration with twitter Boostrap CSS Framework
keywords: codeigniter, pagination, css, twitter, bootstrap, codeigniter bootstrap, codeigniter bootstrap pagination, bootstrap pagination codeigniter library 
---
*CodeIgniter* pagination class is really cool, but I was having a hard time integrating it with the [twitter Bootstrap](http://getbootstrap.com/2.3.2/index.html "twitter Bootstrap Framework") CSS framework, it was something like an extra dozen config parameters while initializing the pagination library, so I had to modify the CodeIgniter pagination class to be compatible with Bootstrap.

I created a new file under `application/libraries/` folder and called it `MY_Pagination.php` 

The MY_Pagination.php file is included at the end of this post, feel free to use it in your projects. This version is compatible with Bootstrap version 2.3.2

I included most if not all of the Bootstrap pagination styling into this new library, so all I have to do later on is just initializing the library in the controller like so,
<!-- more -->
This example makes use of a demo model called model_m which counts total records using the recordsCount() method, and then gets the pagination records using the getRecords() method. If you have any questions about the usage please consult the documentation for [CodeIgniter Pagination Class](http://ellislab.com/codeigniter/user-guide/libraries/pagination.html "CodeIgniter Pagination Class"), or just leave a comment below.
{% codeblock demo.php %}
$page = ($this->uri->segment(3)) ? $this->uri->segment(3) : 0;
$this->load->model('model_m');
$this->load->library('pagination');
$config['base_url'] = base_url('test_page/p/');
$config['total_rows'] = $this->model_m->recordsCount();
$config['per_page'] = 20; 
$config['num_links'] = 3;
$config['uri_segment'] = 3;
$this->pagination->initialize($config); 

$this->data = array(
'pagination' => $this->pagination->create_links(),
'pagination_data'	=> $this->model_m->getRecords($page, $config['per_page']),
);
{% endcodeblock %}

And then just echo the pagination into the html of the view file like so,
``` php
<?php echo (isset($pagination)) ? $pagination : ''; ?>
```
Below is the MY_Pagination.php file to be placed under `application/libraries/` folder of the CodeIgniter installation.
{% include_code MY_Pagination.php %}
