# Делаем блокнот с заметками на Flutter и Firebase часть 1

Практически создаём упрощённый "клон" Google Keep с нуля
![Build a note-taking app with Flutter + Firebase — Part I](https://iswift.ru/images/1_6HTi-RPUL_cmp-UDURxpAg.gif)
Я поклонник [Google Keep](https://www.google.com/keep/), Я использую его с тех пор как он был запущен. В Keep я помещаю задачи с напоминанием по работе и другие необходимые напоминалки. Он интуитивный в использовании и помогает мне сфокусироваться на приорететах.
Так как я делаю [Flutter](https://flutter.dev/) приложения в течении двух последних лет, Я думаю, могло бы быть интересным сделать нечто похожее на Keep с самого нуля.

Так называемое ‘**Flutter Keep**’ приложение, которое я делаю, будет выглядеть следующим образом.

[![Flutter Keep](https://iswift.ru/images/2020-03-09_22-11-25.png)](https://youtu.be/GXNXodzgbcM)

Я представлю этот процесс разработки в серии статей. Ключевые компоненты будут добавлены в приложение в несколько циклов, это сделано для более лёгкого восприятия.

В первой части мы создадим Flutter проект, реализуем процесс аутенфикации и простой экран для показа списка записей.

Итак, приступим.

После создания проекта вы можете включить web поддержку с помощью команды: ```flutter config -- enable-web```, если вы хотите приложение, которое бы запускалось в Web помимо Android и iOS.  
Сейчас, выполните команду: ```flutter create flt_keep``` для создания Flutter Keep app, ```flt_keep``` это имя пакета, которое будет использовано при импорте

Те кто новички во Flutter, перейдите по ссылке [Get started guide](https://flutter.dev/docs/get-started/install) для установки SDK, и ознакомьтесь со структурой проекта.

## Структура

Для приложения "записная книжка" для начала надо определиться куда будут сохраняться и как будут вызываться записи.

Вот, что меня волнует:

* Первое. Конфидициальность. Записи других аккаунтов должны отделяться друг от друга.
* Второе. Приложени должно работать в офлайн. Пользователи, могли бы делать записи не находясь в зоне покрытия интернет. В обязанности приложения входит синхронизация данных при восстановлении сетевого подключения.

Мой выбор это [Cloud Firestore](https://firebase.google.com/docs/firestore), в основном потому что у меня был опыт использования в предыдущих приложениях а так же потмоу что он просто адаптируется.

Я решил использовать специальную коллекцию для хранения всех записей пользователей, одна заметка в одном документе.
Причины:
* Лучшая сегригация между данными аккаунтов.
* Легче делать запросы
* Постараться не выходить за [лимиты](https://firebase.google.com/docs/firestore/quotas#limits) на запись и чтение данных

Такой подход также сопровождается затратами, но он приемлем для демонстрационных целей. Т.е. я создам днамические индексы  для каждой коллекции, я обговорю эту проблему в следующем статье.

На данный момент структура данных выглядит следующим образом.
![Firestore data structure](https://iswift.ru/images/1_K11nEEwAPoEJnSexd22dRg.jpeg)
Firestore структура данных

## Архитектура приложения

Сейчас время рассмотреть как организовать логику в приложении. Не стоит применять "настоящую" архитектуру, данный пример структуры приложения для демонстрации. Но по-прежнему необходимо управлять состояниями на нескольких экранах приложения.

В нашем случаем, мы будем использовать пакет [provider](https://pub.dev/packages/provider) для управления состоянием приложения.  Это позволит нам писать код в стиле reactive (или data-driven) 

Наиболее важные экраны приложения:

* Экран авторизации, показывает состояние входа в систему, гарантирует, что только аутентифицированные пользователи могут делать заметки
* Экран со списком заметок, показывает последнее состояние заметок, должен реагировать на изменения в заметке.
* Редактор заметок также должен реагировать на внешние изменения редактируемой заметки.

Via providers, we can fulfill the above requirements easier, with a cleaner codebase.
С помощью Provider мы можем выполнять указанные выше требования с чистой кодовой базой.

С учётом схемы, начнём писать код.

Что бы использовать ```provider``` и Firebase SDKs, мы должны добавить зависимости в файл ```pubspec.yaml```:

```
provider: ^4.0.2
firebase_core: ^0.4.4
firebase_auth: ^0.15.4
cloud_firestore: ^0.13.3
google_sign_in: ^4.1.4
```
Пожалуйста перейдите по ссылке [Детальные инструкции](https://firebase.google.com/docs/flutter/setup) для интеграции Firebase SDKs с  Android и iOS, а так же с [Web платформой](https://firebase.google.com/docs/web/setup).

Войдите в приложение
Помните, что мы должны отклонить неавторизованных пользователей, давайте сделаем корневой виджет.


<script src="https://gist.github.com/xinthink/7d1ad8cc4421f50266d3406342430c10.js"></script>
main.dart

```StreamProvider/Consumer``` здесь чтобы следить за ```onAuthStateChanged```, потоком событий авторизаций в Firebase, каждый раз когда меняется статус авторизации, ```Consumer``` будет принимать событие и перестраивать виджет согласно настоящего статуса.

Небольшая хитрость в том чтобы использовать ```CusttentUser``` переносить ```FirebaseUser```, чтобы различать начальное и неавторизованное состояния по умолчанию, оба из которых имеют значение ```null```.


<script src="https://gist.github.com/xinthink/9d0853544425791c1aee55eb78900b72.js"></script>
current_user.dart

## Google вход и Firebase авторизация
Я использую Google вход в примере, потмоу что его легко интегрировать. По сути, это всего лишь один из многочисленных сервисов, поддерживаемых Firebase авторизация. Вы можете включить что вам надо в Firebase консоли.
![Available authentication providers](https://iswift.ru/images/1_p28rWu_gssWRU4xwb5NTQg.png)
Available authentication providers

**The code snippet to authenticate using Google Sign-In:**

<script src="https://gist.github.com/xinthink/3c2e2a93b54871ba72a4235ccf2f0554.js"></script>
login_screen.dart

Just ask for a Google Sign-In credential, and then use it to authenticate with Firebase.
You can see that there’s nothing to do after the authentication finished successfully. In this situation, the ```FirebaseAuth.onAuthStateChanged``` stream emits a ‘Signed-in’ event, which triggers a re-build of the root gatekeeper widget so that the ```HomeScreen``` is rendered.
The above is an example of [Reactive Programming](https://en.wikipedia.org/wiki/Reactive_programming): just mutate the state, the listeners who concern about the state will do the remaining jobs.
Back to the project, before testing your login screen, please make sure you’re not ignoring the following settings:
* For the Android platform, you must [specify the SHA-1 fingerprint](https://firebase.google.com/docs/auth/android/google-signin#before_you_begin) in the Firebase console
* For the iOS platform, you have to [add a custom URL scheme](https://firebase.google.com/docs/auth/ios/google-signin#2_implement_google_sign-in) to the Xcode project
* For the Web platform, add a meta tag like ```<meta name="google-signin-client_id" content="{web_client_id}">``` to ```web/index.html```, you can find the Web client id in the ‘OAuth 2.0 Client IDs’ section of your project’s [credentials page](https://console.cloud.google.com/apis/credentials) in the Google Cloud console

## Querying notes

With an authenticated user, we can now enter the main screen of the app, the note list.
But how can you add the first note without a note editor? You can do this with the Firebase Console:

![Firestore console](https://iswift.ru/images/1_JA5qv8JPFfjNA9zAQ3Nhcg.png)
Firestore console

Please name the collection as ```notes-{user_id}```, and you can find your user id in the *Authentication* page of the Firebase Console.
To reinforce privacy security, you may also want to set access rules against the dataset, making sure that users can only see & edit their own notes.

![Firestore data access rules](https://iswift.ru/images/1_8bNXjjHd9lQqAE-BKtPEsQ.png)
Firestore data access rules

Before we can retrieve notes from Firestore, we need a model that represents an individual note, and functions to transform between the Firestore model and our own.

<script src="https://gist.github.com/xinthink/209fa3e9e37de1d9ae098101c12e2e5d.js"></script>

Again we’re going to use a ```StreamProvider``` in the ```HomeScreen```, which watches the notes query result, so that any changes happen to the backend reflect here instantly. The Firestore SDK also delivers the [offline capabilities](https://firebase.google.com/docs/firestore/manage-data/enable-offline) we need, we don’t have to change the code used to access the data.
And thanks for the gatekeeper widget we built previously, which enables us to retrieve the authentication info any time via ```Provider.of<CurrentUser>.```

<script src="https://gist.github.com/xinthink/4e04f2a3ecb3b5097fe0912fca898337.js"></script>
home_screen.dart

The code is a little bit verbose, for I provide here a floating ```AppBar``` looks like the one in Google Keep.
For ```NotesGrid``` and ```NotesList```, they are much similar: just kind of a wrapper of a ```SliverGrid``` and a ```SliverList``` respectively.

<script src="https://gist.github.com/xinthink/e972c4944bf197e60c98e19125f395bc.js"></script>
notes_grid.dart

I’m not posting all the detailed code here. Please find the full example in my [GitHub repo](https://github.com/xinthink/flutter-keep).
If everything goes fine, you should now be able to see the first note in your self-made ***Flutter Keep*** app!

![Flutter Keep screenshot](https://iswift.ru/images/1_kov0KSVUbhuqVP3pCoebRw.png)
Flutter Keep screenshot

We’re doing well so far. We’ve built a simple reactive-styled app by using the ```provider``` package, and also learned how to use the Firebase toolkits.

However, the app is less than useful without a note editor. We’ll add more functionalities to it in the next parts of the series.

Thank you for reading! 🙌

WRITTEN BY Yingxin Wu
