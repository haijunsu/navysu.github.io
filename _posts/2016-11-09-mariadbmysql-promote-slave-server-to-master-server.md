---
id: 132
title: MariaDB/MySql promote slave server to master server
date: 2016-11-09T16:05:58+00:00
author: Navy Su
layout: post
guid: http://navysu.x10host.com/?p=132
permalink: /2016/11/09/mariadbmysql-promote-slave-server-to-master-server/
categories:
  - database
  - MariaDB
  - MySql
tags:
  - mariadb
  - mysql
  - primary
  - slave
---
Scenario: Master server had a problem and was gone. Promote slave 1 as a master server.

Before: (Images are from dev.mysql.com)

<img class="alignnone wp-image-134 size-full" src="https://i0.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-before.png?fit=504%2C353" alt="redundancy-before" srcset="https://i0.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-before.png?w=504 504w, https://i0.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-before.png?resize=300%2C210 300w" sizes="(max-width: 504px) 85vw, 504px" data-recalc-dims="1" />

After:

<img class="alignnone wp-image-133 size-full" src="https://i2.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-after.png?fit=538%2C432" alt="redundancy-after" srcset="https://i2.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-after.png?w=538 538w, https://i2.wp.com/navysu.x10host.com/wp-content/uploads/2016/11/redundancy-after.png?resize=300%2C241 300w" sizes="(max-width: 538px) 85vw, 538px" data-recalc-dims="1" />

One all slave servers,  see message &#8220;Slave has read all relay log&#8221;.

<pre class="prettyprint">$ mysql -u root -p
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 732
Server version: 10.1.19-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]&gt;STOP SLAVE IO_THREAD;
Query OK, 0 rows affected (0.01 sec)

MariaDB [(none)]&gt; SHOW PROCESSLIST;
+----+-------------+-----------+------+---------+------+-----------------------------------------------------------------------------+------------------+----------+
| Id | User        | Host      | db   | Command | Time | State                                                                       | Info             | Progress |
+----+-------------+-----------+------+---------+------+-----------------------------------------------------------------------------+------------------+----------+
|  4 | system user |           | NULL | Connect |  308 | <em><strong>Slave has read all relay log;</strong></em> waiting for the slave I/O thread to update it | NULL             |    0.000 |
|  8 | root        | localhost | NULL | Query   |    0 | init                                                                        | SHOW PROCESSLIST |    0.000 |
+----+-------------+-----------+------+---------+------+-----------------------------------------------------------------------------+------------------+----------+
2 rows in set (0.00 sec)</pre>

On all slave servers

<pre class="prettyprint">MariaDB [(none)]&gt; STOP SLAVE;</pre>

On Slave 1

<pre class="prettyprint">MariaDB [(none)]&gt; RESET MASTER;</pre>

On other slave servers

<pre class="prettyprint">MariaDB [(none)]&gt; change master to 
    -&gt; master_host='10.0.0.31',     # IP of Slave 1
    -&gt; master_user='replica',     # replication ID
    -&gt; master_password='password';     # replication ID's password

MariaDB [(none)]&gt; start slave;

MariaDB [(none)]&gt; show slave status\G</pre>

Reference: <a href="https://dev.mysql.com/doc/refman/5.7/en/replication-solutions-switch.html" target="_blank">https://dev.mysql.com/doc/refman/5.7/en/replication-solutions-switch.html</a>