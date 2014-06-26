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
