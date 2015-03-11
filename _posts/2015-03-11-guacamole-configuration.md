---
layout: post
title: Guacamole Configuration
---

There was a task: get remote control of a computer from anywhere, where u haven't any instruments, and where any ports are closed, except 80.

I found a <a href="http://guac-dev.org/">Guacamole project</a>. Support SSH, Telnet, RDP and VNC. Also support sound.

Contains 2 parts: daemon, web-client.

First, downloading daemon sources (guacamole-server, now it is 0.9.5 version) from <a href="http://guac-dev.org/release/release-notes-0-9-5">the site</a>. After you should install dependencies for supporting protocols, I need only vnc so:
{% highlight sh %}
aptitude install libvnc-server-dev
{% endhighlight %}

Configure and compile it:

{% highlight sh %}
./configure --with-init-dir=/etc/init.d/
...
make && make install
ldconfig
{% endhighlight %}

Client. I have Tomcat server, so I downloaded WAR and just deployed it. For the client there are two config files: guacamole.properties, user-mapping.xml.
They can locate or in /etc/guacamole/, or in /home/<user>/.guacamole/.

guacamole.properties:
{% highlight sh %}
guacd-hostname: localhost
guacd-port:          4822

auth-provider: net.sourceforge.guacamole.net.basic.BasicFileAuthenticationProvider
basic-user-mapping: /home/freekode/.guacamole/user-mapping.xml
{% endhighlight %}
guacd-* - need for connecting to your daemon (guacd), default it is localhost and 4822 port.
auth-provider: BasicFileAuthenticationProvider - simple authentication based on a file, Guacamole also support LDAP and MySQL authentication, but it no needed for me.
basic-user-mapping: <file path> - location of a file with username password and available connections.

user-mappings.xml
{% highlight xml %}
<user-mapping>
    <authorize
        username="admin"
        password="21232f297a57a5a743894a0e4a801fc3"
        encoding="md5">
        <connection name="home">
            <protocol>vnc</protocol>
            <param name="hostname">localhost</param>
            <param name="port">5900</param>
        </connection>
    </authorize>
</user-mapping>
{% endhighlight %}

It's support md5 hashing, but if you want you can disable it, just remove "encoding" attribute and place plain password to "password" attribute. You can create many connections for users.

I hope it helps someone, but actually it is very easy configuration. More you can find in <a href="http://guac-dev.org/doc/gug/">official documentation</a>.
