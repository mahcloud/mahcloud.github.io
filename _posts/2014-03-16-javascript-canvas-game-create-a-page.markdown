---
layout: post
title: "Javascript Canvas Game Create a Page"
date: 2014-03-16 12:00:00
categories: javascriptgame
---

First create an index page.
You can name this file anything you want as long as it is saved as an html extension.
This is where your game lives.
You can see it by opening your index page.

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>Simple Canvas Game</title>
		<style>
			html,body { margin: 0px; padding: 0px; }
			body { background-image: url(images/background.jpg); }
		</style>
	</head>
	<body>
		<script src="js/main.js"></script>
	</body>
</html>
{% endhighlight %}

It won't look like much of anything yet.
Let's fix that by adding a background image.
Your index page says the background image is located at images/background.jpg.
So let's create a folder in the same place as your index page and add a background.jpg image in it.
![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/background.jpg?raw=true)

The game will be built on a javascript engine using acanvas.
First add a js folder and put a main.js file in it.
Adding the following with create a canvas with dimensions 979pixels X 606pixels and add it to the index page.

{% highlight javascript %}
var canvas = document.createElement("canvas");
var ctx = canvas.getContext("2d");
canvas.width = 979;
canvas.height = 606;
document.body.appendChild(canvas);
{% endhighlight %}

The following will create a background image and draw it on the canvas.

{% highlight javascript %}
var bgImage = new Image();
bgImage.onload = function () {
	ctx.drawImage(bgImage, 0, 0);
};
bgImage.src = "images/canvas_background.png";
{% endhighlight %}

![Background Image](https://github.com/mahcloud/SimpleCanvasGame/blob/master/images/canvas_background.png?raw=true)

You'll notice that there is an onload function called.
It takes time for javascript to load up the background image.

You can [download][jekyll-gh-branch] this example.

[jekyll-gh-branch]: https://github.com/mahcloud/SimpleCanvasGame/tree/start
