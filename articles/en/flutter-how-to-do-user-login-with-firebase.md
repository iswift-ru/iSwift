# Flutter : How to do user login with Firebase
![Flutter login with firebase](https://iswift.ru/images/1_9nmjA-EfVfu86LeGAsHlJQ.png "Flutter login with firebase")

## Release
25/12/18 — Updated latest code snippet after refactoring and clean up.
24/01/19 — Duplicated link to github at the top of article.
23/07/19 — Added trim method to email and password value
13/10/19 — Updated code and article after cleanup and packages update

## Source Code

In case you want to skip the whole mumbo jumbo, you can grab the source code here 👇
https://github.com/tattwei46/flutter_login_demo
A sample Flutter project to show case user login and signup process with Firebase authentication. It also shows how to…
github.com

## What is Flutter?

Flutter is an open source mobile SDK developed by Google to build high quality applications for Android and iOS. It allows developers to not only build application with beautiful design, smooth animation and fast performance, but also able to integrate new features quickly. Flutter offers high velocity development with its stateful hot reload and hot restart. With only one code base to manage, you get to save a lot of cost comparing to managing both Android and iOS projects as Flutter compiles it to native ARM code. Flutter uses Dart programming language which is also developed by Google.

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
<p textaligen="center">TextFormField validator in action</p>
