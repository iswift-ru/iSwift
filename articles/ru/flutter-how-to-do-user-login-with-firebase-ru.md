# Flutter : Как сделать авторизацию с помощью Firebase
![Flutter login with firebase](https://iswift.ru/images/1_9nmjA-EfVfu86LeGAsHlJQ.png "Flutter login with firebase")

## Выпуск
25/12/18 — Обновление последней версии кода после рефакторинга и чистки.

24/01/19 — Дублирование ссылки на GitHub в верхней части статьи.

23/07/19 — Добавление trim метода в email и password.

13/10/19 — Обновление кода и статьи после чистки и обнолвниея пакетов.

## Исходный код

В случае, если вы хотите просмотреть целиком всю эту абрукадабру вы можете взять исходный код здесь 👇

https://github.com/tattwei46/flutter_login_demo

Пример проекта Flutter - вход для пользователя в систему и процесса регистрации с аутентификацией Firebase

## Что такой Flutter?

Flutter - это мобильный SDK (Пакет программ для разработки програмного обеспечения) с открытым исходным кодом, разработанный компанией Google для построения высококачественных приложений для Android и iOS. Он позволяет разработчикам не только создавать приложения с красивым дизайном, плавной анимацией и быстрым выполнением, но и так же возможность быстрой интеграции новых функций. Флаттер предлагает быструю разработку с его горячим перезапуском приложения. Имея только одну кодовую базу для управления, вы можете сэкономить много времени по сравнению с отдельной разработкой Android и iOS проектов, поскольку Flutter компилирует его с собственным ARM кодом. Флаттер использует язык программирования Дарт, который также разработал Гугл.

## Почему Dart?

* Если коротко то это строго типизированный, объектно ориентированный язык.
* Поддерживает just-in-time (JIT) и ahead-of-time (AOT) компиляцию.
* JIT позволяет Flutter перекомпилировать код непосредственно на устройстве, пока приложение еще работает.
* Обеспечивает быструю разработку и стабильную "горячую перезагрузку" в течение секунды. 
* AOT позволяет компилировать код непосредственно в собственный ARM-код, что обеспечивает быстрый запуск и предсказуемую производительность.

## Что такое Firebase

Firebase это платформа для мобильной и web разработки, которая позволяет разработчику делать широкий ассортимент продуктов. Сегодня мы будем изучать как мы сможем сделать наше первое Флаттер приложение спомощью Firebase авторизации и realtime database (RTDB).  Это приложение позволит пользователям регистрироваться или входить в аккаунт и совершать CRUD (создание, чтение, обновление, удаление) действия над записями спомощью Firebase. В этом статье, мы сфокусируемся только на регистрации и авторизации пользователей.

## Какие версии интсрументов используются?

Здесь  перечислены версии инструментов которые изпользуются в статье и в гитхабе.
* Flutter Version: flutter_macos_v1.9.1+hotfix.4-stable
* Android Studio Version: 3.5
* Xcode Version: 11.1

## Как установить окружающую среду?

* Следуйте инструкция по ссылке https://flutter.dev/docs/get-started/install чтобы установить Flutter SDK. Вам следует установить или Android Studio или VSCode с Flutter и Dart плагинами.
* Запустите Flutter doctor для установки зависимостей

```flutter doctor```

* Для использования IOS эмулятора сделайте команду:

```open -a Simulator```

* для использования android эмулятора, выполните следующие действия: Следуйте **Android Studio > tools > AVD Manager** и выбирите создание **Virtual Device.**

## Создание Flutter приложения

You can get the complete source code in the GitHub link at the bottom of the post. Вы можете использовать готовый исходный код на ГитХабе по ссылке в верхней части статьи. Ниже показано как мы написали исходный код для флаттер приложения опубликованный на ГитХабе.

### 👉Шаг 1: Создание нового Flutter проекта под названием "Flutter login demo"
Запускаем эмулятор и запускаем проект с помощью Флаттер.

```flutter run```

Для запуска Android и iOS эмулятора выполните следующую команду.

```flutter run -d all```

Вы должны увидеть симулятор экранов Андройд и ИОС.
![Android](https://iswift.ru/images/1_s5aA2j3HJAONXq1RrBLpGA.png "Android")![IOS](https://iswift.ru/images/1_VdFSnH_gOFIG2QGBY_N9ZA.png "IOS")


>Если вам интересно знать как сделать скриншоты в ваших симуляторах;
>
>Для Android: Просто нажмите на иконку "камера" на левой стороне панели инструментов. Картинка будет сохранена на рабочем столе.
>
>Для iOS: [Вариант 1] Нажмите вместе кнопки command + shift + 4. Нажмите клавишу пробела, чтобы изменить указатель мыши на значок камеры.
>
>Наведите курсор на симулятор iOS, щелкните, чтобы сделать снимок экрана. Картинка будет сохранена на рабочий стол.
>
>Для iOS: [Вариант 2] Выбирите симулятор и нажмите command + S. Спасибо JerryZhou за то, что поделился этой информацией.

### 👉Шаг 2: Замена оригинального кода на Привет Мир (Hello World)
At main.dart, erase all contents and add the following boilerplate to your file.В файле main.dart удалите все содержимое и добавьте в файл следующующий шаблон. Вы должны создать новый файл под названием login_signup_page.dart в котором будет LoginSignupPage класс. В терминале нажмите клавишу R, чтобы выполнить горячую перезагрузку, и вы должны увидеть "Hello World" на экране.

<script src="https://gist.github.com/tattwei46/4f5a72c294781b89f835f969acc20267.js"></script>
<script src="https://gist.github.com/tattwei46/43ad1fbda618e436a2ecb115786c00bc.js"></script>

![Hello world](https://iswift.ru/images/1_gG7KSUYl1Elmn9OyjPP-Vg.png "Hello world")

### 👉Шаг 3: Меняем состояние виджета с stateless на stateful
<script src="https://gist.github.com/tattwei46/33cf7585688ad6d964b49a89da47a514.js"></script>

### 👉Шаг 4: Replace Hello World with Stack
Внутри Scaffold body, заменим текстовый виджет Hello Word на stack виджет. Stack виджет позволит нам наложить один виджет на другой.  Идея в том, чтобы показывать круговой процесс загрузки , когда кто то выполняет действие регистрирации или авторизации. В Stack мы будем иметь Форму и кргуовой индикатор прогресса. Внутри Form мы добавим ListView, который позволит нам вставить массив виджетов. Таким образом, мы можем выполнить рефакторинг нескольких компонентов пользовательского интерфейса (UI) и поместить их в ListView. 
***Профессиональный совет*** : *Всякий раз, когда мы используем ввод текста, лучше обернуть его виджетом ListView, чтобы предотвратить ошибку рендеринга (отрисовки) при появлении виртуальной клавиатуры из-за переполнения пикселов.*

<script src="https://gist.github.com/tattwei46/bac22f40a192fa6677263d2a6a29363d.js"></script>

### 👉Step 5: Building each UI components
We start with building our circular progress bar. Thanks to Flutter, this is available as built-in widget call ```CircularProgressIndicator``` . We will use ```bool _isLoading``` to determine whether to show the ```CircularProgressIndicator``` or not.

<script src="https://gist.github.com/tattwei46/e6157ce651461de1b8171c8cb5e2f8fb.js"></script>

Next, we will build our form logo. In the repository, there is an asset folder with the following flutter-icon.png file. To import into our project, add the following line in pubspec.yaml followed by ```flutter packages get``` command.

<script src="https://gist.github.com/tattwei46/6f5beb00e244f28dc0676b5af217e9bf.js"></script>
<script src="https://gist.github.com/tattwei46/3f0b3b871aba0f067817e6ceabc3181e.js"></script>

Next we will add 2 *TextFormField* for our email and password. TextFormField allows us to add ```validator``` and ```onSaved```. These are callback methods will be triggered when ```form.validate()``` and ```form.save()``` is called (TextFormField is inside Form). So for example if ```form.save()``` is called, the value in the text form field is copied into another local variable.
For the ```validator```, we will check for empty field inputs and update ```_errorMessage``` to be shown to user. We also need to create variables ```_email``` and ```_password``` to store the values which are then used for authentication later.
For password, we set ```obsecureText: true``` to hide user password.
We set ```maxLines``` to 1 to fix the height of the textformfield.
```autofocus``` is set to ```false``` to prevent the field getting focused when the page is loaded.
Both email and password values are ```trim()``` to remove any whitespaces.

<script src="https://gist.github.com/tattwei46/98a6a7411263966ae939d15e95b2e483.js"></script>

Next we will build 2 buttons. One primary for user action of login/signup and one secondary to switch form mode(between login and signup). For keeping track of the form mode, we use a boolean ```_isLoginForm```
When the primary button press event is detected, we will trigger method ```_validateAndSubmit``` which validates our *TextFormField* inputs and perform login/signup.

<script src="https://gist.github.com/tattwei46/87856f547b4708b46fc290d67bb11744.js"></script>

Next we build secondary button to toggle between login and signup. We use ```FlatButton``` instead of ```RaisedButton``` used in primary button. The reason is if you have 2 buttons and would like to make 1 more distinctive than the other, ```RaisedButton``` is the right choice as it catches more attention compare to ```FlatButton```.

<script src="https://gist.github.com/tattwei46/58eb89108e73923037f058f3fb58ba5a.js"></script>

For switching between login and signup, it is just doing set state with toggled bool ```_isLoginMode```. Also we have a ```resetForm ```method to clear the inputs of the *TextFormField (this is kept in GlobalKey of FormState which will be explained later below).*

<script src="https://gist.github.com/tattwei46/d39a7135b80a50913bd70ab48dc9b3d1.js"></script>

Next, we are build UI to display error messages to user. These messages could be error thrown by Firebase or invalid form input. If there is a new error message, we will ```setstate``` with new values of ```_errorMessage```

<script src="https://gist.github.com/tattwei46/5747f69581a87278c22f7c556de59174.js"></script>

Finally, back to our form, we need to create a GlobalKey to keep our form state. This keeps track of the user typed email and password.
Add the following in class ```_LoginSignupPageState```

```final _formKey = new GlobalKey<FormState>();```

And linked it under _showForm(), key properties

```Widget _showForm() {
  return new Container(
      padding: EdgeInsets.all(16.0),
      child: new Form(
        key: _formKey,
        child: new ListView(
          shrinkWrap: true,
          children: <Widget>[
            showLogo(),
            showEmailInput(),
            showPasswordInput(),
            showPrimaryButton(),
            showSecondaryButton(),
            showErrorMessage(),
          ],
        ),
      ));
}```

Let’s arrange those individual UI components and put it back to our *ListView*.

<script src="https://gist.github.com/tattwei46/399aaee36ae6d91d705d19a30d7aa192.js"></script>

Let’s try to run our project using ```flutter run``` command

![Flutter login demo](https://iswift.ru/images/1_dgisA_6Dmtsdhz_GbmFrBg.png "Flutter login demo")
<p align="center">TextFormField validator in action</p>

### 👉Step 6: Register project with Firebase
Go to https://console.firebase.google.com and register new project.
For android, click the android icon. Enter your package name which can be found in ```android/app/src/main/AndroidManifest.xml```
Download the config file which is ```google-services.json``` (Android).
Drag the ```google-services.json``` into app folder in project view

![google-services.json](https://iswift.ru/images/1_Jom5-2bcarPMsrIEbMK3mw.png "google-services.json")

We need to add the Google Services Gradle plugin to read google-services.json. In the ```/android/app/build.gradle``` add the following to the last line of the file.

```apply plugin: 'com.google.gms.google-services'```

```In android/build.gradle```, inside the buildscript tag, add new dependency.
```buildscript {
   repositories {
      //...
}
dependencies {
   //...
   classpath 'com.google.gms:google-services:3.2.1'
}```
For iOS, open ```ios/Runner.xcworkspace``` to launch Xcode. The package name can be found in bundle identifier at Runner view.
Download the config file which is ```GoogleService-info.plist``` (iOS).
Drag the ```GoogleService-info.plist``` into the Runner subfolder inside Runner as shown below.

### 👉Step 7: Add dependencies in pubspec.yaml
Next we need to add *firebase_auth* and *firebase_database* dependency in pubspec.yaml. To get the latest version number, go to https://pub.dartlang.org/ and search for the dependency.

```firebase_auth: ^0.14.0+5
firebase_database: ^3.0.7```

### 👉Step 8: Import Firebase Auth

```import 'package:firebase_auth/firebase_auth.dart';```

### 👉Step 9: Enable sign up using email and password at Firebase
![Sign up](https://iswift.ru/images/1_UCo6SN3eK-Rn30YMSSWaVA.png "sign up")
### 👉Step 10: Create abstract class BaseAuth
Create new file call authentication.dart. We are going to create abstract class BaseAuth. The class only contains the signature of the methods and this ensures minimal changes if we decided to change from Firebase to something else.
<script src="https://gist.github.com/tattwei46/35f58c7faebce33c56bddedb1d99cf93.js"></script>

### 👉Step 11: Create class Auth implementing BaseAuth
This is where we define what the methods in the abstract class do.

<script src="https://gist.github.com/tattwei46/90e0006da3043a7322f351545879085c.js"></script>

### 👉Step 12: Create HomePage
Once user logs in successfully, they will be directed into home page.

<script src="https://gist.github.com/tattwei46/9a4892eb2a344ad1e0f556009b4942ca.js"></script>

### 👉Step 13: Create RootPage
So RootPage which is a stateful page, actually decides whether to show user LoginSignupPage or HomePage based on their authentication status.
Hence, we keep track of the authentication status using :
```enum AuthStatus {
  NOT_DETERMINED,
  NOT_LOGGED_IN,
  LOGGED_IN,
}```
When the RootPage is loaded, we will try to get the userid and set ```AuthStatus```
<script src="https://gist.github.com/tattwei46/346239c19e9bd8ec24caa4bc440ccea9.js"></script>

We have 2 callback functions for login and logout. So when user is at LoginSignupPage and successfully logs in, it will trigger the login callback in RootPage, that sets the ```AuthStatus``` to ```LOGGED_IN``` and subsequently show user the HomePage.
The same happens when user successfully logs out when in HomePage.

<script src="https://gist.github.com/tattwei46/ee5bc1e09a7864a65110ad76c9dd36ad.js"></script>

Here is the part, on showing user the correct page according to their AuthStatus. More explaination on auth in the subsequent steps.

<script src="https://gist.github.com/tattwei46/743be7c679296c04db1b57d22ae53e99.js"></script>

### 👉Step 14: Initialize Auth in main
In main.dart, when we call new RootPage, we initialize new ```Auth()``` and pass into RootPage as shown.

<script src="https://gist.github.com/tattwei46/df8645225b187220f372f94fff96519d.js"></script>

In root_page.dart, we receive the initialized auth as follows:
<script src="https://gist.github.com/tattwei46/72c47af73bd30767e227e9a01f944a6e.js"></script>

### 👉Step 15: Link up LoginSignupPage and RootPage
In the RootPage class, we call LoginSignupPage and pass in auth that was pass in earlier from main.dart and also linked our callback function. We use widget.auth instead of auth is because this variable was pass into rootpage class from MyApp class instead of initialized in rootpage.
Here is snapshot of code inside RootPage class that calls LoginSignupPage

<script src="https://gist.github.com/tattwei46/14260d7c0f430314758bb6318841523c.js"></script>

Here is snapshot of code inside LoginSignupPage that receives the auth and loginCallback
<script src="https://gist.github.com/tattwei46/bea89f750c620f6246d72f7c228fff7b.js"></script>

### 👉Step 16: Use Auth inside LoginSignupPage

We use ```widget.auth.signIn``` that was implemented according to abstract class BaseAuth to log user in. The underlying method uses Firebase ```signInWithEmailAndPassword``` which returns a future value. A future is part of asynchronous operation that does not block the main thread. Future class is associated with **async** and **await** keyword. Hence the method needs to have await and the external wrapper function needs to have async. So we enclose the login and signup methods with try catch block. If there was an error, our catch block should be able to capture the error message and display to the user.

<script src="https://gist.github.com/tattwei46/28d2e93b434fcf02553aba3417036fd2.js"></script>

### 👉Step 17: Try to sign up a user
Let’s try to sign up a user by entering an email and password.

>If you encounter something like below, this is because there is an extra spacing at the end of your email. That is why I have added >trim to email

```I/flutter (14294): Error PlatformException(exception, The email address is badly formatted., null)```

>If you encounter something like below, change your password to be at least 6 characters long.

```I/flutter (14294): Error PlatformException(exception, The given password is invalid. [ Password should be at least 6 characters ], null)```
Finally once success, you should be able to see in your terminal the following line. The random string is the user ID.

```I/flutter (14294): Signed up JSwpKsCFxPZHEqeuIO4axCsmWuP2```

Similarly if we try to sign in the same user we signed up, we should get something like this:

```I/flutter (14294): Signed in JSwpKsCFxPZHEqeuIO4axCsmWuP2```

![Debug banner removed](https://iswift.ru/images/1_hfrGL0FlyfbmVCrZPBKFmw.png "Debug banner removed")
<p align="center">Debug banner removed</p>

**ProTip:** Notice the debug ribbon at the top right corner of the app, you can easily remove it by adding the following line inside MaterialApp widget in main.dart
```debugShowCheckedModeBanner: false,```

🎊🎉Success🎉🎊

![Success](https://iswift.ru/images/1_lEkb1HvKFvsCmSseNFZLhA.gif "Success")
