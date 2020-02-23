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

**👉Step 1: Create a new Flutter project call flutter login demo**
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

**👉Step 2: Replace original code with Hello World**
At main.dart, erase all contents and add the following boilerplate to your file. We are going to create a new file called login_signup_page.dart which has LoginSignupPage class. On your terminal, hit the R key to perform hot reload and you should see “Hello World” on the screen.

<script src="https://gist.github.com/tattwei46/4f5a72c294781b89f835f969acc20267.js"></script>
