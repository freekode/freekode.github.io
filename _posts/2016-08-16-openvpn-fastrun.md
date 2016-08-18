---
layout: post
title: OpenVPN FastRun
---

I quite rare configuring [OpenVPN](https://openvpn.net/index.php/open-source/downloads.html) severs, and I am
forgetting main command which I need to generate keys and so on.

So here is my set of commands:

{% highlight sh %}

sudo su
cd /etc/openvpn
mkdir easy-rsa
cd easy-rsa
cp -r /usr/share/doc/openvpn/examples/easy-rsa/2.0/* ./

{% endhighlight %}

edit tail of the `vars` file

{% highlight sh %}

source ./vars
./clean-all
./build-ca
./build-key-server my-sever
./build-dh
openvpn --genkey -- secret keys/ta.key
./build-key me

{% endhighlight %}

that's all.

[Here](https://github.com/freekode/config/blob/master/openvpn/server.conf) my example of a server config. And
[here](https://gist.github.com/trovao/18e428b5a758df24455b) you can find good ovpn generator. But do not forget
change `cipher` type in the generator.
