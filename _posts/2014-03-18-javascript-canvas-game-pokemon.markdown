---
layout: post
title: "Javascript Canvas Game Pokemon"
date: 2014-03-18 10:45:00
categories: JavascriptGame
---

Wouldn't it be more fun if we were dealing with Pokemon instead of an Ogre?

![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/1.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/2.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/3.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/4.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/5.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/6.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/7.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/8.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/9.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/10.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/11.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/12.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/13.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/14.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/15.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/16.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/17.png?raw=true)
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/pokemon/18.png?raw=true)

These are the image dimensions for the pokemon.

{% highlight javascript %}
var pokemon_dimensions = new Array(
	new Array(27, 32),
	new Array(27, 32),
	new Array(29, 32),
	new Array(27, 32),
	new Array(21, 32),
	new Array(32, 24),
	new Array(31, 32),
	new Array(32, 30),
	new Array(30, 32),
	new Array(27, 32),
	new Array(26, 32),
	new Array(28, 32),
	new Array(30, 32),
	new Array(32, 24),
	new Array(32, 30),
	new Array(24, 32),
	new Array(32, 26),
	new Array(18, 32)
);
{% endhighlight %}

Add this to your spawnMonster function.

{% highlight javascript %}
pokemon = Math.floor(Math.random() * 18) + 1;
monsterImage.src = "images/pokemon/"+pokemon+".png";
monster.width = pokemon_dimensions[pokemon - 1][0];
monster.height = pokemon_dimensions[pokemon - 1][1];
{% endhighlight %}

Let's rename some methods.
monstersSlain should be pokemonCaught.
slayMonster should be catchPokemon.

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/add-hero
