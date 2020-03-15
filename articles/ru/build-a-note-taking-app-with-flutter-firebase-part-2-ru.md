# Делаем блокнот с заметками на Flutter и Firebase (часть 2)
Реализация текстового редактора с обратимыми действия и Hero переходов
![Implements a plain-text editor with reversible actions & Hero transitions](https://iswift.ru/images/1_gVY7JU07MLstpEzGwgYDFA.jpeg)

Рад видеть вас во второй части статьи ***Делаем блокнот с заметками на Flutter и Firebase***. Если вы не читали предыдущую статью, то вы можете найти её [здесь](https://iswift.ru/articles/ru/build-a-note-taking-app-with-flutter-firebase-part-1-ru).

В [части 1](build-a-note-taking-app-with-flutter-firebase-part-1.md), мы сделали первый экран для приложения, **Flutter Keep**. В этой статье мы будем создавать редактор заметок с поддержкой обратимых действий и исследовать волшебную анимацию [Hero](https://flutter.dev/docs/development/ui/animations/hero-animations). 


## Редактор заметок
В [Google Keep](https://www.google.com/keep), много типов записей, включая обычные текстовые заметки, аудио заметки, чеклисты с опцией прикрепления картинки. Однако в нашем примере мы сфокусируемся на обычном редакторе заметок, это будет проще.

На картинке анонс того что мы будем делать:
[Note editor preview](https://iswift.ru/images/1_MGIa1fUmmPk2K87fMC9DmA.jpeg)
Note editor preview

In essence, it composites of two text fields, one for the title, another for the plain-text content. Plus, a top ```AppBar``` and a ```ModalBottomSheet``` provides actions to update either the state or the color of the note.
So let’s start with a ```StatefulWidget```:

<script src="https://gist.github.com/xinthink/55c8d5ee739d729e4556ba78e53af4fc.js"></script>
note_editor.dart

In the editor state, we keep a copy of the original note (could be an empty one) to check whether the editor is dirty.
And don’t forget to make it accessible from the ```HomeScreen```:

<script src="https://gist.github.com/xinthink/16e0fc34f115ac91b2fad5b685a5444e.js"></script>
home_screen.dart

Ok, go on building the widget:
<script src="https://gist.github.com/xinthink/ea6cab070973d44c2c1bafebaf21b231.js"></script>
editor_state.dart

We’re going to save the note to FireStore before the editor is closed, by using a ```WillPopScope``` widget, which fires the ```onWillPop``` callback right before the current screen is being dismissed.

<script src="https://gist.github.com/xinthink/4031a6c058ef49055aa0640852703664.js"></script>
NoteEditor#onWillPop

Introducing a ```ChangeNotifierProvider``` is to provide the ```Note``` object being edited to descendant widgets to keep them aligned with the latest state. For example, the editor should update the background color when the user picks a different one in the ```ColorPicker``` widget.
To use ```ChangeNotifierProvider```, the Note class has to extend the ```ChangeNotifier```, and to ```notifyListeners``` whenever its state has changed:

<script src="https://gist.github.com/xinthink/273c2e989febb8d7cb6cbc649b409a44.js"></script>
note.dart

We’ll see how it works.
Actions like archiving and picking a theme color are organized in a bottom sheet.
One thing that may be confusing is when we ```showModalBottomSheet```, we have to provide the note object again, or the descendant widgets (of the bottom sheet) won’t be able to retrieve it via ```Consumer``` or ```Provider.of.```(It’s a different widget tree from the editor body)

<script src="https://gist.github.com/xinthink/452e4fdbab98f7e7a035dd4325bc7ac7.js"></script>
NoteEditor#showModalBottomSheet

To understand how a ```ChangeNotifierProvider``` works, we take the color picker as an example.
What does the ```LinearColorPicker``` do? It renders a horizontal list of available tints for a note:

<script src="https://gist.github.com/xinthink/19d042dd248f868df4bc7d576684a6e6.js"></script>
color_picker.dart

When one of the tints is selected, the picker updates the ```color``` property of the note, and that is it.
An ancestor widget (of the ```ColorPicker```) we’ve built previously, which watching the note, gets notified and then refreshes the screen so that we can see the whole editor is tinted with the color we pick.
[Editor state synchronization](https://iswift.ru/images/1_lxF2s-WTKFumm_LjPzrwdQ.gif)
Editor state synchronization

The other actions, such as deletion and archiving, share the same mechanism.

## Reversible operations
We can now edit a note by updating the properties including the state. But how about the UX? What if users delete a note by accident?
For dangerous operations like deleting or archiving, a ```SnackBar``` could be used to provide a reverse action, e.g., restoring or unarchiving, in addition to a prompt message.

[](https://iswift.ru/images/1_jSq4WiKVETUOcmoDWipdzg.gif)

To implement reversible operations more cleanly, we’re going to apply the [Command Pattern](https://en.wikipedia.org/wiki/Command_pattern).
First, we define the command interface, which is responsible for applying an action to a note.

<script src="https://gist.github.com/xinthink/273367c3be45f2e06f0617a2f5325e68.js"></script>
note_command.dart

For this notebook app, actions considered reversible are all about mutating the state of a note. So only one concrete command is needed. However, nothing prevents you from extending it to other situations.

<script src="https://gist.github.com/xinthink/bfb348de0e95da409d4622d9f0cd5c18.js"></script>
note_state_command.dart

Next, we produce and consume commands, for example:

<script src="https://gist.github.com/xinthink/5a292f5d1048bfd8577b2c41b861f51f.js"></script>
Using commands

Done, we’ve made the dangerous operations reversible!

## Hero transition
Now we have a working note editor, let’s take a step further, by adding a beautiful transition animation between the ```HomeScreen``` and the ```NoteEditor```.

[](https://iswift.ru/images/1_DsydtXamxtPWtUva2NNvag.gif)

We can see that a note item grows to the size of the entire screen, from where it located in the grid. In Flutter, that’s called a Hero Animation.
The usage is simple. First, we wrap the note item in the grid or list with a ```Hero``` widget:

<script src="https://gist.github.com/xinthink/4c823df06de3a325ccf2d0d09fa71b7c.js"></script>
Hero note item

Then wrap the editor widget too:
<script src="https://gist.github.com/xinthink/50837a0b043d6f884979078f067d5dfa.js"></script>
Hero note editor

In the above snippets, the two tags passed to the ```Hero``` widgets must be identical.
And the ```DefaultTextStyle``` widgets are applied to avoid the big underlined text during screen transition on the iOS platform.
Now we can leave the rest to the Flutter framework.
What noticeable is that the standard screen transition animation is platform-specific. You could make a custom transition as you need, but it is beyond the scope of this article. Please refer to this [cookbook](https://flutter.dev/docs/cookbook/animation/page-route-animation).

## Tips
In an example app, we don’t bother to apply patterns like [BLOC](https://bloclibrary.dev/). But there are still ways to keep the code clean and avoid boilerplates.
For example, we can move the reversible operation handling procedure to a stand-alone [mixin](https://dart.dev/guides/language/language-tour#adding-features-to-a-class-mixins), to make a cleaner separation between UI and logic, and also make it reusable.

<script src="https://gist.github.com/xinthink/55d100dbcce13b11a025701218b3caf6.js"></script>
command_handler.dart

Whenever you need to handle commands, just mix it in:

<script src="https://gist.github.com/xinthink/50d2e408173bd08c6fe3c0d5c1dc72ba.js"></script>
Mixin CommandHandler

In addition, with Dart SDK ```2.7.0``` or later, we can leverage [extension methods](https://dart.dev/guides/language/extension-methods) to do things magical.
We can augment the ```Note``` model with FireStore related functionalities:

<script src="https://gist.github.com/xinthink/e8f2a9fc6671bc6e139fa34d76aa2db7.js"></script>
note_store.dart

Which makes persisting a note as easy as a method call:
<script src="https://gist.github.com/xinthink/f3d3176563b11cbfc5d8c9965acfe64b.js"></script>
Using note extensions

We can even add properties and methods to an enumeration, which is impossible in the declaration of enumerated types:
<script src="https://gist.github.com/xinthink/1f98ec3bca2fe7e8af8c161ffd9ad827.js"></script>
Extensions on an enum

That saves us a lot of repeated code!
Wrapping it up, we’ve delivered a working plain-text note editor in this iteration. We’ve even added features like reversible actions and Hero transitions. Please find the complete code example in this [GitHub repo](https://github.com/xinthink/flutter-keep).
In the next part, I’d like to introduce how to query different subsets of notes from FireStore, and the issue of composite indexes.

Thank you for reading! 🙌

