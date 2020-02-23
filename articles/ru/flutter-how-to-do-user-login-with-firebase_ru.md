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

## Why Dart?

* A terse, strongly-typed, object-oriented language.
* Supports just-in-time (JIT) and ahead-of-time (AOT) compilation.
* JIT allows Flutter to recompile code directly on the device while the app is still running.
* Enable fast development and enables sub-second stable hot reloading.
* AOT allows code to be compiled directly into native ARM code leading to fast startup and predictable performance.

## What is Firebase

Firebase is a mobile and web development platform that provides developer with wide range of products. Today we will be looking on how we can build our first Flutter application with Firebase authentication and realtime database (RTDB). This application allows user to perform account sign up/login and CRUD actions on todo items with Firebase. On this post, we are going to solely focus on the user signup and login part.

## What are the tools version?

Here are the versions of the tools used for this article and on Github
* Flutter Version: flutter_macos_v1.9.1+hotfix.4-stable
* Android Studio Version: 3.5
* Xcode Version: 11.1

## How to setup environment?

* Follow through the instructions in https://flutter.dev/docs/get-started/install to get your Flutter SDK. You should be installing either Android Studio or VSCode with Flutter and Dart plugins.
* Run Flutter doctor to install any dependencies

```flutter doctor```

* To launch iOS simulator use the following command:

```open -a Simulator```

* To launch android emulator, do the following: Launch **Android Studio > tools > AVD Manager** and select create **Virtual Device.**

## Building Flutter App

You can get the complete source code in the GitHub link at the bottom of the post. The following shows how do we derive from Flutter sample project to complete source code in GitHub.

### 👉Step 1: Create a new Flutter project call flutter login demo
Launch simulator and run project using Flutter.

```flutter run```

If you have both Android emulator and iOS Simulator running, run the following command to execute on both.

```flutter run -d all```

You should see similar screens on both Android Emulator and iOS Simulator.
![Android](https://iswift.ru/images/1_s5aA2j3HJAONXq1RrBLpGA.png "Android")![IOS](https://iswift.ru/images/1_VdFSnH_gOFIG2QGBY_N9ZA.png "IOS")


>If you’re interested to know how to get screenshots at your simulators;
>For Android: Simply click the camera icon on the left side of the tool pane. The image will be saved to desktop
>For iOS: [Option 1] Hold and press command + shift + 4. Press the space bar to change Mouse pointer to camera icon. Point to iOS >simulator, click to take screenshot. The image will be saved to desktop.
>For iOS: [Option 2] Select Simulator and press command + S. Thanks JerryZhou for sharing this information.

### 👉Step 2: Replace original code with Hello World
At main.dart, erase all contents and add the following boilerplate to your file. We are going to create a new file called login_signup_page.dart which has LoginSignupPage class. On your terminal, hit the R key to perform hot reload and you should see “Hello World” on the screen.

<script src="https://gist.github.com/tattwei46/4f5a72c294781b89f835f969acc20267.js"></script>
<script src="https://gist.github.com/tattwei46/43ad1fbda618e436a2ecb115786c00bc.js"></script>

![Hello world](https://iswift.ru/images/1_gG7KSUYl1Elmn9OyjPP-Vg.png "Hello world")

### 👉Step 3: Changing from stateless to stateful
<script src="https://gist.github.com/tattwei46/33cf7585688ad6d964b49a89da47a514.js"></script>

### 👉Step 4: Replace Hello World with Stack
Inside the Scaffold body, let’s replace the Hello Word text widget to a stack widget. The stack widget allows us to overlay one widget above the other. The idea is to show circular progress bar when any login or sign up activity is running. In the stack, we will have the Form and the circular progress bar. Inside the Form, we will add a ListView that allows us to put an array of widgets. With that, we are able to refactor out several UI Components and put them inside the ListView.
***Pro Tip*** : *Whenever we are using text input, it is better to wrap it around a ListView to prevent rendering error when the soft keyboard shows up due to overflow pixels.*

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
