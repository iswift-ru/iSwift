# Flutter – Lists with JSON

Hi Friends,

This post is a continuation of my previous post here. I would kindly request you to read it once before proceeding.

To learn basics of flutter get my course at a discounted price – here.

Other courses on flutter – here. (Affliate link – keeps my site alive by helping me pay for hosting)

Let’s begin.

A screencast of this post is available here.

https://youtu.be/POexDexSpKE

Let’s continue from where we left off.

In our last post we saw how to make a http request and receive a json response right ?

Now open up main.dart file and modify it as shown below.

<script src="https://gist.github.com/iswift-ru/384f4b90218bb3260ec077da6c3a04f9.js"></script>

Let’s break this down.

First, I have removed the button in the center of the screen. Instead of the button, I need to make the http request automatically when the app loads. So, I am overriding the initState() function and making the call.

Once I receive the data, I am simply creating a list with a set of tiles and from the response I got, I am just getting the name, phone number and avatar image alone.

Now run the app and you will see the below screen.
![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-14-e1526746144606.png)
