---
layout: post
title: NUT send emails
---

NUT can send to you emails about ups status.

First of all, you need to configure your smtp sender. I am using mstmp.
Ok, now you need to configure your upsmon.conf, you need change this lines:

{% highlight sh %}
...

NOTIFYCMD "/etc/nut/notifycmd"

NOTIFYFLAG ONLINE       SYSLOG
NOTIFYFLAG ONBATT       SYSLOG
NOTIFYFLAG LOWBATT      SYSLOG+EXEC
NOTIFYFLAG FSD          SYSLOG+EXEC
NOTIFYFLAG COMMOK       SYSLOG
NOTIFYFLAG COMMBAD      SYSLOG
NOTIFYFLAG SHUTDOWN     SYSLOG+EXEC
NOTIFYFLAG REPLBATT     SYSLOG+EXEC
NOTIFYFLAG NOCOMM       SYSLOG+EXEC
NOTIFYFLAG NOPARENT     SYSLOG

...
{% endhighlight %}

<strong>NOTIFYCMD</strong> it is the path to your script, which will be send an email.
<strong>NOTIFYFLAG SHUTDOWN SYSLOG+EXEC</strong> means that in this state (SHUTDOWN) nut write to syslog this event, and execute script, which we declared previously. You can add EXEC to another flags.

Next we need create a script, which send an email.


 
{% highlight sh %}
#!/bin/bash
EMAIL='<your email>'

echo -e "Subject: nut: $NOTIFYTYPE\r\n\r\nUPS: $UPSNAME\r\nAlert type: $NOTIFYTYPE\r\n\r\n`upsc $UPSNAME`" | msmtp -a default $EMAIL
{% endhighlight %} 

<strong>$NOTIFYTYPE, $UPSNAME</strong> it is variables from NUT.

And you can test it by this command:

{% highlight sh %}
upsdrvctl -t shutdown
{% endhighlight %}
