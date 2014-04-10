---
layout: post
title: "First rails deploy"
date: 2014-04-09 18:25:00
categories: RailsDeploy
---

I've been working on learning how to deploy a rails app without the help of [Heroku][heroku].
You might ask why I would want to do that.
That's a really good question that I ask myself everytime I can't figure things out for hours on end.
I won't answer that question now.
I'll leave that for the last post I make on RailsDeploy.

My teacher is the author of [Fearless Rails Deployment][fearless].
If you preorder his book, he'll let you view the draft so far.
I don't want to give away anything that is in his book, so I'm just gonna skim over what I've learned.

I deployed using capistrano.
This will create the capistrano files:
{% highlight bash %}
cap install
{% endhighlight %}

To use capistrano you'll need to be able to ssh authenticate with ssh key instead of password.
[Here][authenticate] is a great tutorial on how to do that.

First, I needed to install unicorn as my rails server.
This part was a lot easier than I thought it was going to be.
After adding the gem and installing it, I just made a unicorn config file and ran unicorn.
{% highlight bash %}
vim /etc/unicorn/mahcloud.rb
{% endhighlight %}

{% highlight ruby %}
pid '/home/root/mahcloud/tmp/unicorn.pid'
working_directory '/home/root/mahcloud/'
{% endhighlight %}

The config file has a little more to it than that.

[heroku]: https://www.heroku.com
[fearless]: https://railsdeploymentbook.com/
[authenticate]: http://gorails.com/deploy/ubuntu/12.04
