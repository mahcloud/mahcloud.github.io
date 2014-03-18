---
layout: post
title: "Javascript Canvas Game Slay Monster"
date: 2014-03-18 10:00:00
categories: JavascriptGame
---

Slaying monsters in our javascript arena.

{% highlight javascript %}
var slayMonster = function() {
	if(isTouching(hero, monster)) {
		monster.x = 100;
		monster.y = 100;
	}
};

var isTouching = function(a, b) {
	var ret = false;
	if((a.x <= (b.x + b.width)) && (b.x <= (a.x + b.width)) && (a.y <= (b.y + b.height)) && (b.y <= (a.y + b.height))) {
		ret = true;
	}
	return ret;
};
{% endhighlight %}

Slay monster function simply checks if the hero and monster are touching and then respawns the monster at location 100, 100.

The slayMonster function should be called in the main function.

{% highlight javascript %}
slayMonster();
{% endhighlight %}

It's more fun if we had some stats on how many monsters we have slain.
Add a variable to the hero array to keep track of how many monsters have been slain.

{% highlight javascript %}
hero.monstersSlain = 0;
{% endhighlight %}

Now in the slayMonsters if isTouching block add an incrementer.
{% highlight javascript %}
++hero.monstersSlain;
{% endhighlight %}

We need to show this on the screen.
Add an HTML element to store the stat in.
We'll also need jQuery, a helpful javascript library.
{% highlight html %}
<div id="monsters_slain_header">
	<span>Monsters Slain: </span><span id="monsters_slain">0</span>
</div>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
{% endhighlight %}

To push the monsters slain count to the html element, add this to your render function.

{% highlight javascript %}
$('#monsters_slain').html(hero.monstersSlain);
{% endhighlight %}

Let's add some styling to our stat bar.

{% highlight css %}
#monsters_slain_header { color: white; font-weight: bold; font-size: 18px; margin: 10px 15px; }
{% endhighlight %}

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/slay-monster
