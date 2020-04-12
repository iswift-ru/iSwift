# Как передать значение переменной во  Flutter используя VoidCallback и Function(x)

> В этой статье мы будем разбираться как использовать стиль обратного вызова событий для коммуникаций между виджетами во Flutter.

Почему это важно? Это позволяет нам разделять наши виджеты на небольшие тестируемые модули, которые можно адаптировать к их контексту.
## *Создаём новый Flutter проект*

Как всегда мы начинаем с создание нового проекта:

```# New Flutter project
$ flutter create widget_communication

# Open this up inside of VS Code
$ cd widget_communication && code .```

Теперь мы можем открыть это в симуляторе iOS или Android из VS Code.

## Видео версия

Если Вам интересно, я записал видео, где вы можете посмотреть как это сделать:


[![Video Version](https://iswift.ru/images/2020-04-10_23-00-30.png)](https://youtu.be/fWlPwj1Pp7U)


## Count This!
Первый метод, мы будем использовать простую передачу данных вниз ребёнку как свойство. Давайте обновим ```main.dart``` и укажем ссылку на **CounterPage**, который мы скоро создадим.


<script src="https://gist.github.com/iswift-ru/c39a6d7a1f4b95cafe3c32820d2042c1.js"></script>

Виджет ```CounterPage``` это простой ```StatefulWidget```:
<script src="https://gist.github.com/iswift-ru/a76e67e10c1e7943e3e979e60ed3ad4c.js"></script>


Внутри этого виджета мы установим ```count``` эквивалентный 0 и передадим это в виджет с именем ```Count``` как свойство. Теперь давайть создадить виджет с именем ```Count```.

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

