---
layout: post
title: Installing MYSQL Gem On Snow Leopard
categories:
  - software development
tags:
  - ruby
  - mysql
---

This was the **one** post I made on my tumblr blog. Since it looked kind of
lonely up there I thought I would bring it over...

I had quite a bit of a headache trying to get the MySQL ruby gem working
on Snow Leopard.  Every time I would try to migrate my database or start
Mongrel I would get "NameError (uninitialized constant
MysqlCompat::MysqlRes)".

After doing some googling I found the ruby weblog guide for upgrading to Snow Leopard.  There they recommend installing the 64 bit version of MySQL and then to install the gem run the following command:

{% highlight bash %}
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/usr/local/mysql/bin/mysql_config
{% endhighlight %}
<br />

MySQL and the gem installed ok but I would still get the same error when
trying to migrate or start the development server.  A little more
googling led me to Robby Grossman’s blog with instructions for
installing MySQL using macports and a slightly different command for
installing the gem.  To install MySQL with macports:

{% highlight bash %}
$ sudo port install mysql5-devel
$ sudo /opt/local/lib/mysql5/mysql_install_db -user=mysql
{% endhighlight %}
<br />

Then I combined his instructions for installing the gem with the one on
the ruby weblog.

{% highlight bash %}
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/usr/local/mysql/bin/mysql_config
{% endhighlight %}
<br />

Hope this helps, Good luck!
