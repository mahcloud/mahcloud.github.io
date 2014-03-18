---
layout: post
title: "Javascript Canvas Game Monster Gates"
date: 2014-03-18 10:30:00
categories: JavascriptGame
---

Monsters should spawn randomly at gate opening in arena.
First, add the gates array.

{% highlight javascript %}
var gates = new Array(
		new Array(canvas.width / 2, 0),
		new Array(canvas.width, canvas.height / 2),
		new Array(canvas.width / 2, canvas.height),
		new Array(0, canvas.height / 2)
	);
{% endhighlight %}

Then we want to spawn a monster randomly.

{% highlight javascript %}
var spawnMonster = function() {
	gate = Math.floor(Math.random() * 4) + 1;
	monster.x = gates[gate - 1][0];
	monster.y = gates[gate - 1][1];
};
{% endhighlight %}

And change this in the slayMonster function

{% highlight javascript %}
monster.x = 100;
monster.y = 100;
{% endhighlight %}

for

{% highlight javascript %}
spawnMonster();
{% endhighlight %}

Added bonus, we can have the monster start at a spawn point by adding.

{% highlight javascript %}
spawnMonster();
{% endhighlight %}

And change the monster array values.

{% highlight javascript %}
monster.x = -100;
monster.y = -100;
{% endhighlight %}

This means that the monster will be drawn off the screen before it is spawned.

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/monster-gates
