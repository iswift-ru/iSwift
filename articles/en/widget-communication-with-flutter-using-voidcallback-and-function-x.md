# Widget Communication with Flutter using VoidCallback and Function(x)
> In this article we’re going to investigate how we can use callback-style events to communicate between widgets with Flutter.

Why is this important? It allows us to separate our widgets into small, testable units that can be adaptable to their context.

## *Creating a New Flutter Project*

As always, we’ll start off by setting up a new project:

```# New Flutter project
$ flutter create widget_communication

# Open this up inside of VS Code
$ cd widget_communication && code .```

We can now open this up in the iOS or Android simulator from within VS Code.

## Video Version
If you’re interested, I’ve recorded a video where you can see me go over the steps in this article:



[![Video Version](https://iswift.ru/images/2020-04-10_23-00-30.png)](https://youtu.be/fWlPwj1Pp7U)


## Count This!
The first method we’re going to use is simply passing data down to the child as a property. Let’s update main.dart to contain a reference to our CounterPage that we’ll create in a second:

<script src="https://gist.github.com/iswift-ru/c39a6d7a1f4b95cafe3c32820d2042c1.js"></script>
