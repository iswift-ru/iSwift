# Делаем блокнот с заметками на Flutter и Firebase (часть 2)
Реализация текстового редактора с обратимыми действия и Hero переходов
![Implements a plain-text editor with reversible actions & Hero transitions](https://iswift.ru/images/1_gVY7JU07MLstpEzGwgYDFA.jpeg)

Рад видеть вас во второй части статьи ***Делаем блокнот с заметками на Flutter и Firebase***. Если вы не читали предыдущую статью, то вы можете найти её [здесь](https://iswift.ru/articles/ru/build-a-note-taking-app-with-flutter-firebase-part-1-ru).

В [части 1](build-a-note-taking-app-with-flutter-firebase-part-1.md), мы сделали первый экран для приложения, **Flutter Keep**. В этой статье мы будем создавать редактор заметок с поддержкой обратимых действий и исследовать волшебную анимацию [Hero](https://flutter.dev/docs/development/ui/animations/hero-animations). 


## Редактор заметок
В [Google Keep](https://www.google.com/keep), много типов записей, включая обычные текстовые заметки, аудио заметки, чеклисты с опцией прикрепления картинки. Однако в нашем примере мы сфокусируемся на обычном редакторе заметок, это будет проще.

На картинке анонс того что мы будем делать:
![Note editor preview](https://iswift.ru/images/1_MGIa1fUmmPk2K87fMC9DmA.jpeg)
Note editor preview

По сути он состоит из двух текстовых полей в одном тайтл в текстовый контент. Плюс, в верху ```AppBar``` и  ```ModalBottomSheet``` обеспечивают действия обновления либо состояния либо цвета записи.
Итак, давайте начнём с ```StatefulWidget```:

<script src="https://gist.github.com/xinthink/55c8d5ee739d729e4556ba78e53af4fc.js"></script>
note_editor.dart

В состоянии редактора мы сохраняем копию исходной заметки (возможно, пустую), чтобы проверить, не загрязнен ли редактор.
И не забудьте сделать это доступным из ```HomeScreen```:

<script src="https://gist.github.com/xinthink/16e0fc34f115ac91b2fad5b685a5444e.js"></script>
home_screen.dart

Ok, продолжаем делать виджет:
<script src="https://gist.github.com/xinthink/ea6cab070973d44c2c1bafebaf21b231.js"></script>
editor_state.dart


Мы должны сохранить заметку перед тем как закроем редактор, для этого будем использовать ```WillPopScope``` виджет, который запустит ```onWillPop``` обратный вызов перед непосредственным закрытием текущего экрана.

<script src="https://gist.github.com/xinthink/4031a6c058ef49055aa0640852703664.js"></script>
NoteEditor#onWillPop

Представляем ```ChangeNotifierProvider``` предоставляет ```Note``` объект, отредактированный к виджетам потомка, чтобы сохранить их выровненными с последним состоянием. Для примера, редактор должен обновить цвет заднего фона, когда пользователь выбирает другой цвет в виджете  ```ColorPicker```.

Использовать ```ChangeNotifierProvider```,  Note класс должен быть extend ```ChangeNotifier```, и ```notifyListeners``` каждый раз когда его статус поменяется:

<script src="https://gist.github.com/xinthink/273c2e989febb8d7cb6cbc649b409a44.js"></script>
note.dart

Давайте посмотрим как это работает.
Действия архивации и выбора цвета темы орагнизованны в нижней части листа.
Одно может сбить с толку  это когда мы  ```showModalBottomSheet```, Мы должны предоставить объект заметки снова, или дочерние виджеты (нижнего листа) не сможет получить его через ```Consumer``` или ```Provider.of.```(Это другое дерево виджета из тела редактора)

<script src="https://gist.github.com/xinthink/452e4fdbab98f7e7a035dd4325bc7ac7.js"></script>
NoteEditor#showModalBottomSheet

Понять как ```ChangeNotifierProvider``` работает, мы выбирем цвет, как напримере.
Что делает ```LinearColorPicker```? Визуализация горизонтального списка доступных оттенков для заметки:

<script src="https://gist.github.com/xinthink/19d042dd248f868df4bc7d576684a6e6.js"></script>
color_picker.dart


Когда один из оттенков выбран, обновляется свойство  ```color``` в заметке вот и всё.
Ранее созданный виджет предка ( ```ColorPicker```), который наблюдает за заметкой, получает уведомление и затем обновляет экран, чтобы мы могли видеть весь редактор тонированным цветом, который мы выбираем.

[Editor state synchronization](https://iswift.ru/images/1_lxF2s-WTKFumm_LjPzrwdQ.gif)
Editor state synchronization

Другие действия, такие как удаление и архивация используют тот же мехаанизм.

## Обратимые операции
Теперь можно изменить заметку, обновив свойства, включая состояние. Но как насчёт UX? Что делать, если пользователи удалили заметку случайно?

Для опасных операций таких как удаление или архивация, ```SnackBar``` может использоваться для выполнения обратного действия, т.е. восстановление или разархивация в дополнительной подсказке.


[](https://iswift.ru/images/1_jSq4WiKVETUOcmoDWipdzg.gif)

Для более чистого выполнения обратимых операций мы будем применять [Command Pattern](https://en.wikipedia.org/wiki/Command_pattern).

Во первых мы определим коммандный интерфейс, который будет ответственный за выполнение действий к заметке.

<script src="https://gist.github.com/xinthink/273367c3be45f2e06f0617a2f5325e68.js"></script>
note_command.dart

Для этого приложения заметок действия, считающиеся обратимыми, направлены на изменение состояния заметки. Так что нужна только одна конкретная команда. Однако ничто не мешает распространить его на другие ситуации.

<script src="https://gist.github.com/xinthink/bfb348de0e95da409d4622d9f0cd5c18.js"></script>
note_state_command.dart

Дальше, мы создадим и используем команды, для примера:

<script src="https://gist.github.com/xinthink/5a292f5d1048bfd8577b2c41b861f51f.js"></script>
Using commands

Done, we’ve made the dangerous operations reversible! Теперь мы сделали опасные операции обратимыми.

## Hero переход
Сейчас мы имеем работающий редактор заметок, давайте приступим к следующему шагу, добавим красивый анимированный переход между ```HomeScreen``` и ```NoteEditor```. 

[](https://iswift.ru/images/1_DsydtXamxtPWtUva2NNvag.gif)

Мы видим, что элемент заметки увеличивается до размера всего экрана, откуда он расположен в сетке. В Flutter, это называется Hero Animation.

Прост в использовании. Во первых, мы обернём элемент заметки в сетку или лист виджетом ```Hero```:


<script src="https://gist.github.com/xinthink/4c823df06de3a325ccf2d0d09fa71b7c.js"></script>
Hero note item

Затем обернём виджет редактора:
<script src="https://gist.github.com/xinthink/50837a0b043d6f884979078f067d5dfa.js"></script>
Hero note editor

В вышеприведенных фрагментах два тега, переданные виджетам ```Hero```, должны быть идентичными.
И ```DefaultTextStyle```виджеты применены, чтобы избежать большого подчеркнутого текста во время перехода экрана на платформе iOS.

Теперь мы можем оставить в покое Flutter фреймворк.

Что примечательно, так это то, что стандартная анимация перехода экрана является специфичной для платформы. You could make a custom transition as you need, but it is beyond the scope of this article. Вы можете сделать пользовательский переход если нужно, но это за пределами данной статьи. Пожалуйста, перейдите сюда [cookbook](https://flutter.dev/docs/cookbook/animation/page-route-animation).

## Шаги
В примере приложения, мы не беспокоились о применении структуры такой как [BLOC](https://bloclibrary.dev/). Но есть еще способы сохранить код чистым и избежать шаблонов.

Например, можно переместить обратимую процедуру обработки операций в автономный режим [mixin](https://dart.dev/guides/language/language-tour#adding-features-to-a-class-mixins), чтобы сделать более чище разделение между UI и логикой, а также сделать его повторно используемым.

<script src="https://gist.github.com/xinthink/55d100dbcce13b11a025701218b3caf6.js"></script>
command_handler.dart

Каждый раз когда необходима обработка команд, просто смешивайте их в:

<script src="https://gist.github.com/xinthink/50d2e408173bd08c6fe3c0d5c1dc72ba.js"></script>
Mixin CommandHandler

Кроме того, начиная с Dart SDK ```2.7.0``` или позже, вы можете эффективно использовать [дополнительные методы](https://dart.dev/guides/language/extension-methods) делая магические вещи.

Мы можем дополнить модель ```Node``` функциональными возможностями FireStore

<script src="https://gist.github.com/xinthink/e8f2a9fc6671bc6e139fa34d76aa2db7.js"></script>
note_store.dart

Что делает сохранение заметки так же простым, как вызов метода:
<script src="https://gist.github.com/xinthink/f3d3176563b11cbfc5d8c9965acfe64b.js"></script>
Using note extensions

Мы даже можем добавить свойства и методы в перечисление, что невозможно в объявлении перечисляемых типов:

<script src="https://gist.github.com/xinthink/1f98ec3bca2fe7e8af8c161ffd9ad827.js"></script>
Extensions on an enum

Это спасает нас от повторения кода!

Завершая его, мы поставили рабочий редактор простых текстовых заметок в этой части. Мы даже добавили такие функции, как обратимые действия и Hero переходы. Найти полностью код примера вы можете в [GitHub repo](https://github.com/xinthink/flutter-keep).

В следующей части я хотел бы рассказать о том, как запрашивать различные подмножества заметок из FireStore, а также о выпуске составных индексов.

Спасибо, что читали! 🙌

