---
layout: post
title: "Javascript Canvas Game Add Monster"
date: 2014-03-16 12:30:00
categories: javascriptgame
---

Let's add a monster to our javascript arena.
You'll need a monster.png image.

{% highlight javascript %}
var monsterImage = new Image();
monsterImage.onload = function () {
	ctx.drawImage(monsterImage, 100, 100);
};
monsterImage.src = "images/monster.png";
{% endhighlight %}

![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/monster.png?raw=true)

Next, let's make them move.
We'll make it so we can control the hero's movement.
First, we need to update the images so they load every time the hero and monster move.
Change the image code to:

{% highlight javascript %}
var bgImage = new Image();
bgImage.src = "images/canvas_background.png";

var heroImage = new Image();
heroImage.src = "images/hero.png";

var monsterImage = new Image();
monsterImage.src = "images/monster.png";
{% endhighlight %}

Basically removing the onload functions.

We will then reload the images every few miliseconds with:

{% highlight javascript %}
var hero = {};
hero.height = 32;
hero.width = 32;
hero.x = (canvas.width / 2) - (hero.width / 2);
hero.y = (canvas.height / 2) - (hero.height / 2);

var monster = {};
monster.height = 32
monster.width = 30;
monster.x = 100;
monster.y = 100;

var render = function () {
	ctx.clearRect (0, 0, canvas.width, canvas.height);
	ctx.drawImage(bgImage, 0, 0);
	ctx.drawImage(monsterImage, monster.x, monster.y);
	ctx.drawImage(heroImage, hero.x, hero.y);
};

var main = function() {
	render();
};

setInterval(main, 100);
{% endhighlight %}

Now let's make our monster move.

{% highlight javascript %}
var moveMonster = function () {
	monster.x += monster.xDirection;
	monster.y += monster.yDirection;
	resetObjectLocation(monster);
};

var updateMonsterDirection = function () {
	monster.xDirection = ((Math.random() < 0.5)? -1 : 1) * Math.random() * monster.speed;
	monster.yDirection= ((Math.random() < 0.5)? -1 : 1) * Math.random() * monster.speed;
};

var resetObjectLocation = function(object) {
	if(object.y < 0) {
		object.y = 0;
	}
	if(object.x < 0) {
		object.x = 0;
	}
	if(object.y > (canvas.height - object.height)) {
		object.y = canvas.height - object.height;
	}
	if(object.x > (canvas.width - object.width)) {
		object.x = canvas.width - object.width;
	}
}
{% endhighlight %}

And add the following to the main method.

{% highlight javascript %}
moveMonster();
{% endhighlight %}

Add the following to the monster array.

{% highlight javascript %}
monster.speed = 15;
monster.xDirection = monster.speed;
monster.yDirection = monster.speed;
{% endhighlight %}

This goes at the bottom of your file to tell the monster to change direction every half second

{% highlight javascript %}
setInterval(updateMonsterDirection, 500;
{% endhighlight %}

But what if we wanted to control the hero using the arrow keys?
It would be the same logic just based on input from the keyboard.
Here's how we receive input from the keyboard.
{% highlight javascript %}
var keysDown = {};

addEventListener("keydown", function (e) {
	keysDown[e.keyCode] = true;
}, false);

addEventListener("keyup", function (e) {
	delete keysDown[e.keyCode];
}, false);

var moveHero = function () {
	if(38 in keysDown) { // Player holding up
		hero.y -= hero.speed;
	}
	if(40 in keysDown) { // Player holding down
		hero.y += hero.speed;
	}
	if(37 in keysDown) { // Player holding left
		hero.x -= hero.speed;
	}
	if(39 in keysDown) { // Player holding right
		hero.x += hero.speed;
	}
	resetObjectLocation(hero);
};
{% endhighlight %}

Don't forget to add speed to your hero array.

{% highlight javascript %}
hero.speed = 15;
{% endhighlight %}

And add moveHero method call to your main method.
{% highlight javascript %}
moveHero();
{% endhighlight %}

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/monster
