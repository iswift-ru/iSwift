# Как передать значение переменной во  Flutter используя VoidCallback и Function(x)

> В этой статье мы будем разбираться как использовать стиль обратного вызова событий для коммуникаций между виджетами во Flutter.

Почему это важно? Это позволяет нам разделять наши виджеты на небольшие тестируемые модули, которые можно адаптировать к их контексту.

**Создаём новый Flutter проект**

Как всегда мы начинаем с создания нового проекта:

**New Flutter project**
$ flutter create widget_communication

**Open this up inside of VS Code**
$ cd widget_communication && code

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

Мы ожидаемо получим цифру равную  ```0```:

![](https://iswift.ru/images/w-com-prop.png)

## VoidCallback
For example’s sake, let’s turn our count into a ```Button``` and say that any time we click the button we want to notify the parent ```CounterPage```.
Для примера, давайте поместим count в кнопку ```Button``` и скажем ей что каждый раз когда нажимается кнопка мы хотели бы уведомить родителя ```CounterPage```


Т.к. мы не хотим возвращать значение здесь мы дожны харегистрировать```VoidCallback```. Мы так же добавим фигурные скобки к элементам нашего конструктора ```Count``` сделав их именнованными параметрами.

<script src="https://gist.github.com/iswift-ru/b9d1214980e2e236a69078d286452458.js"></script>

Теперь нам нудно обновить наш ```CounterPage``` для прослушивания обратного вызова ```onCountSelected```:

<script src="https://gist.github.com/iswift-ru/445b96b24c550649fbc3232d4ac579dc.js"></script>


Если мы нажмем на значение нашего счётчика, то мы должны увидеть **Count was selected** в нашей дебаг консоли!

## Function(x)
Хотя использование VoidCallback отлично подходит для идентификации событий обратного вызова без ожидаемого значения, что мы делаем, когда хотим вернуть значение обратно родителю?


Вводим, ```Function(x)```:

<script src="https://gist.github.com/iswift-ru/bd0610c1afd08290206f327197121cb2.js"></script>

Здесь мы добавим пару кнопок и новую функцию ```Function(int)``` и назовём её ```onCountChange``` которую мы вызываем со значением, которое мы хотим передать обратно родителю.

Внутри родителя мы можем прослушивать её и менять значение ```count``` на соответствующее:

<script src="https://gist.github.com/iswift-ru/e6db35f5dfbee429be185757487e4857.js"></script>

Here’s the result of our work:
А вот и результат нашей работы

![result](https://iswift.ru/images/w-com-2.png)

