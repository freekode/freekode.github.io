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

`-D`, `--no-createdb` - The new user will not be allowed to create databases. This is the default.<br>
`-R`, `--no-createrole` - The new user will not be allowed to create new roles. This is the default.<br>
`-S`, `--no-superuser` - The new user will not be a superuser. This is the default.<br>
`-P`, `--pwprompt` - If given, createuser will issue a prompt for the password of the new user. This is not necessary if you do not plan on using password authentication.<br>



**UPD:** Remove user
{% highlight sh %}
$ sudo -u postgres dropuser -i myuser 
Role "myuser" will be permanently removed.
Are you sure? (y/n)
{% endhighlight %}



Thats it. 
