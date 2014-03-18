---
layout: post
title: "Javascript Canvas Game Add a Hero"
date: 2014-03-16 12:15:00
categories: JavascriptGame
---

Let's add a hero to our javascript arena.
You'll need a hero.png image

{% highlight javascript %}
var heroImage = new Image();
heroImage.onload = function () {
	ctx.drawImage(heroImage, 474, 287);
};
heroImage.src = "images/hero.png";
{% endhighlight %}

![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/hero.png?raw=true)

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/add-hero
