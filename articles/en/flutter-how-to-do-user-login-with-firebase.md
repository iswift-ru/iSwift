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
