---
layout: post
title: "Chef - Installing Postgres - Load Balanced Servers"
date: 2014-06-26 09:31:00
categories: RailsDeploy
---

I want to talk about provisioning a postgres server for a load balanced production rails application. This is probably a topic that everyone will come across. You want a live rails application and it starts becoming a bigger deal, you need to spin up more servers and have them load balanced. Well, in that case, you need one server that has the database and all the other application servers to talk to it. No big deal, right. Well, I'm using chef to provision my servers and I got stuck on a very simple thing. Okay, I provisioned my servers, but how do I get the password for my provisioned server so my application can talk to my database. Seems simple enough, but where is that password.


The postgres password is stored in the node config file. To access this using chef knife:


{% highlight bash %}
knife edit node SERVER-NAME
{% endhighlight %}



So simple, but I just didn't know where it was. I thought I would make note, so no more time is wasted.


A couple other notes about connecting to a postgres server. After chef provisioning, when I ran cap deployment, I was running into this error:
{% highlight bash %}
	rake aborted!
DEBUG [] 	PG::ConnectionBad: could not connect to server: Connection refused
DEBUG [] 		Is the server running on host "IP_ADDRESS" and accepting
DEBUG [] 		TCP/IP connections on port 5432?
{% endhighlight %}


So first thoughts turned to, is my server configured to accept tcp/ip connections? What port is my postgres server running on?
I read [this article][tcpip_socket] on tcp/ip configurations with postgres, but when I added `tcpip_socket = true`, it ran errors.
I then found [this article][listen_addresses] on setting up your postgres server to listen outside of localhost. That seemed to do the trick.


[tcpip_socket]: http://www.postgresql.org/message-id/004e01c22c05$18be79b0$05faa8c0@edios
[listen_addresses]: http://stackoverflow.com/questions/17648677/is-the-server-running-on-host-localhost-1-and-accepting-tcp-ip-connections
