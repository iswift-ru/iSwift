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
The first method we’re going to use is simply passing data down to the child as a property. Let’s update ```main.dart``` to contain a reference to our CounterPage that we’ll create in a second:

<script src="https://gist.github.com/iswift-ru/c39a6d7a1f4b95cafe3c32820d2042c1.js"></script>

```CounterPage``` The widget is a simple ```StatefulWidget```:
<script src="https://gist.github.com/iswift-ru/a76e67e10c1e7943e3e979e60ed3ad4c.js"></script>

Inside of this widget we’re establishing a ```count``` equal to 0 and passing this into a widget named ```Count``` as a property. Let’s create the ```Count``` widget:

<script src="https://gist.github.com/iswift-ru/6b567bdab848e6a501c6e327fd343a90.js"></script>

This gives us the following expected count of ```0```:

![](https://iswift.ru/images/w-com-prop.png)

## VoidCallback
For example’s sake, let’s turn our count into a ```Button``` and say that any time we click the button we want to notify the parent ```CounterPage```.

As we don’t want to return a value here, we’ll need to register a ```VoidCallback```. We’ll also add braces to the items within our ```Count``` constructor to make them named parameters.

<script src="https://gist.github.com/iswift-ru/b9d1214980e2e236a69078d286452458.js"></script>

We’ll then need to update our ```CounterPage``` to listen to the ```onCountSelected``` callback:

<script src="https://gist.github.com/iswift-ru/445b96b24c550649fbc3232d4ac579dc.js"></script>

If we select the value of our counter now, we should see **Count was selected**. inside of the debug console!

## Function(x)
Whilst the use of VoidCallback is great for identifying callback events with no expected value, what do we do when we want to return a value back to the parent?

Enter, ```Function(x)```:

<script src="https://gist.github.com/iswift-ru/bd0610c1afd08290206f327197121cb2.js"></script>

Here we’ve added a couple of buttons and a new ```Function(int)``` named ```onCountChange``` that we’re calling with the value that we want to pass back to the parent.

Inside of the parent we’re able to listen to this and change the value of ```count``` accordingly:

<script src="https://gist.github.com/iswift-ru/e6db35f5dfbee429be185757487e4857.js"></script>

Here’s the result of our work:

![result](https://iswift.ru/images/w-com-2.png)
