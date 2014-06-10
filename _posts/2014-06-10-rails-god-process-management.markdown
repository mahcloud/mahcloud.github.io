---
layout: post
title: "Ruby God - Process Management"
date: 2014-06-10 09:25:00
categories: RailsDeploy
---

So I'm back after a few months.


The project I was working on that needed a custom install was not received well. Originally everyone liked it but when customers that didn't understand it started to complain, management just wanted it shutdown. Bad data is better than no data at all I guess.


Well, I'm back and onto a new project that needs rails deployment. This time it is a reporting tools. In order to not slow down the live orders/quotes table with reporting, I'm building a seperate database of order/quote data that should be populated identical to the live database. I'm using [rabbitMQ][rabbit] to transfer a hash of the data when an order or quote is changed. This way it doesn't slow down the transaction or database on the live site. The hash is put in a queue by the rabbit exchange. Our reporting application will then consume that data and save the calculations to a postgres database.


Now that we have all the background story covered, I've got the RoR application deploying live and to a beta staging environment. When I deploy to these boxes, I need a zero downtime switch to the new code. Since the reporting server is constantly consuming data from rabbitMQ, it would be very bad if it missed any of these queued items. I'll be using [god][god] to keep the rabbitMQ listener up and running even during a code deployment. So, the line I need to add to my capistrano deployment is:


{% highlight bash %}
sudo /var/god/.rvm/bin/god restart APP-NAME
{% endhighlight %}

I'm having a little problem here.

{% highlight bash %}
Command: /usr/bin/env sudo
DEBUG [] 	usage: sudo -h | -K | -k | -V
DEBUG [] 	usage: sudo -v [-g group] [-h host] [-p prompt] [-u user]
DEBUG [] 	usage: sudo -l [-g group] [-h host] [-p prompt] [-U user] [-u user]
DEBUG [] 	  
DEBUG [] 	          [command]
DEBUG [] 	usage: sudo [-r role] [-t type] [-C num] [-g group] [-h host] [-p
DEBUG [] 	  
DEBUG [] 	          prompt] [-u user] [VAR=value] [-i|-s] [<command>]
DEBUG [] 	usage: sudo -e [-r role] [-t type] [-C num] [-g group] [-h host] [-p
DEBUG [] 	    
DEBUG [] 	        prompt] [-u user] file ...
cap aborted!
SSHKit::Command::Failed: sudo exit status: 1
sudo stdout: Nothing written
sudo stderr: usage: sudo -h | -K | -k | -V
/gems/sshkit-1.4.0/lib/sshkit/command.rb:98:in `exit_status='
{% endhighlight %}

First thought, this exact script does work on the server by manually entering it.


Next, what is [SSHKit][sshkit]?
It is a gem capistrano uses to make ssh connections to all the different environments I will be deploying to.


Exit status 1 just means there was an error with no more explanation than that.
So, I'm a little stuck here. Bash doesn't give any specific error code. I would guess it has something to do using sudo without a password on an ssh connection using an ssh-key for authentication. I asked my boss if he had any suggestions. He said `does the deploy user have password-less sudo rights?`. It looks like there is something called pty and tty. So what are they and what's the difference?
I found [TTY and PTY][ttyvspty] and [Teletype][ttystack]. Still very confused why I would need a virtual terminal for deploying and if I capistrano needs a virtual terminal to run commands how it did anything with out it. Maybe it was already using one and I can just set it to a different one.


Okay, so I still feel lost enough not to know where to go. Since, pty doesn't sound harmful at all, I guess I will just turn it on and see what happens.


Turning on pty with `set :pty, true` or `set :tty, true` in config/deploy.rb file gave the same results.


Okay, I think was on the wrong track with the pty thing. I had to add my user as a [passwordless sudo][passwordless] user for deploying. That took care of the problem.

[rabbit]: http://www.rabbitmq.com/
[god]: http://godrb.com/
[sshkit]: https://github.com/capistrano/sshkit
[ttyvspty]: http://ubuntuforums.org/archive/index.php/t-833765.html
[ttystack]: http://stackoverflow.com/questions/4426280/what-do-pty-and-tty-mean
[passwordless]: http://askubuntu.com/questions/192050/how-to-run-sudo-command-with-no-password
