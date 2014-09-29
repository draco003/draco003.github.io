---
layout: post
title: "CodeIgniter Sessions using PostgreSQL"
date: 2013-09-14 11:17:31 +0200
comments: true
categories: [CodeIgniter, PostgreSQL]
description: CodeIgniter 2.1.4 Save Session to PostgreSQL database, creating ci_sessions table in PostgreSQL
keywords: postgres, postgresql, codeigniter, ci_sessions, sql, sessions database, codeigniter sessions postgres, ci_sessions postgresql codeigniter, ci_sessions postgres 
---
CodeIgniter is one of my favorite PHP MVC frameworks, it gives you the option to scale fast. Having a wide range of database drivers to choose from including postgres is a great option for me. I prefer PostgreSQL over MySQL for large projects where you have to insert 300k+ records daily or more. Of course you have to do some tweaks to your basic configuration of the PostgreSQL server to keep up with big data, but we might discuss this in a future post.

CodeIgniter offers a Sessions library to manage sessions your way instead of sticking to PHP server sessions variable **$_SESSION**. You can read more about it in the CodeIgniter User Guide V2.1.4: [Session class](http://ellislab.com/codeigniter/user-guide/libraries/sessions.html "Session Class").

It also gives you an option to save Session data into database, and it provides the database structure for MySQL, unfortunately this will not work with Postgres, so I created a Postgres version
``` sql
CREATE TABLE  ci_sessions (
	session_id varchar(40) DEFAULT '0' NOT NULL,
	ip_address varchar(45) DEFAULT '0' NOT NULL,
	user_agent varchar(120) NOT NULL,
	last_activity bigint DEFAULT 0 NOT NULL,
	user_data text NOT NULL,
	PRIMARY KEY (session_id)
);

CREATE INDEX last_activity_idx ON ci_sessions(last_activity);
```
Saving the Session data into a database is more secure than cookies, and it gives you more options and functions, for example something like who is online and tracking active sessions.

Thanks, feel free to comment or suggest.

