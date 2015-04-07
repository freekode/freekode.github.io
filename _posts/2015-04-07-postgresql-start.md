---
layout: post
title: PostgreSQL start
---

I connect to database quite rarely (over the console). So often I forget simple things. This is my start for PostgreSQL database, you just installed it (9.4 version).

I will use console commands.

Create user and database:
{% highlight sh %}
$ sudo -u postgres createuser -DRSP myuser 
Enter password for new role: 
Enter it again:
$ sudo -u postgres createdb -O myuser mydb
{% endhighlight %}

Thats it. 
