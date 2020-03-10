# Build a note-taking app with Flutter + Firebase — Part I
The practice of creating a simplified ‘clone’ of Google Keep from scratch
![Build a note-taking app with Flutter + Firebase — Part I](https://iswift.ru/images/1_6HTi-RPUL_cmp-UDURxpAg.gif)
I’m a fan of [Google Keep](https://www.google.com/keep/), I’ve been using it since it was launched. I put pending tasks, reminders for chores, almost anything needs to remember, into Keep. It’s intuitive to use, helps me stay focused on the priorities.
Since I’ve been building [Flutter](https://flutter.dev/) apps during the past two years, I think it might be interesting to make a notebook app like Keep from scratch.

The so-called ‘**Flutter Keep**’ app I made so far looks like:

[![Flutter Keep](https://iswift.ru/images/2020-03-09_22-11-25.png)](https://youtu.be/GXNXodzgbcM)

I’m going to introduce the process in a series of articles. Key components will be added to the app during iterations, to make it easier to understand.
In this first part of the series, we’re going to set up the Flutter project, provide an authentication process, and a simple screen to show the note list.

Let’s get started.

Before creating the project, you may want to enable web support via the command: ```flutter config -- enable-web```, if you want the app to be able to run on Web besides Android and iOS.
Now, execute the command: ```flutter create flt_keep``` to create the Flutter Keep app, ```flt_keep``` is the package name that will be used in the import statements.
For those who are new to Flutter, please follow the [Get started guide](https://flutter.dev/docs/get-started/install) to install the SDK, and get familiar with the project structure.

## Data structure

For a notebook app, the first thing to consider is how the notes should be persisted and queried.
What I concern includes:
* First, the privacy. Notes of different accounts should be separated from each other.
* Second, the app ought to work offline. Users should be able to take notes under any network condition. It is the app’s responsibility to sync the data when network connectivity recovers.
My choice is [Cloud Firestore](https://firebase.google.com/docs/firestore), mainly because of the experience I gained from previous projects, but also because it is straightforward to adapt.

I decided to use a dedicated collection to store each user’s notes, one document for a single note. For the reasons:
* Better segregation of each account’s data
* Easy to query
* Avoids some [limitations](https://firebase.google.com/docs/firestore/quotas#limits) in reading & writing data

This approach also comes with a cost, but it is acceptable for a demonstration purpose. That is, I have to create indexes for each collection dynamically, I’ll discuss the issue in a later article.

For now, the data structure is like:
![Firestore data structure](https://iswift.ru/images/1_K11nEEwAPoEJnSexd22dRg.jpeg)
Firestore data structure

## App architecture
Now it’s time to consider how to organize the app logic. It won’t worth it to apply a ‘real’ architecture to an app mostly for demonstration. But there’s still a need to manage states across multiple screens in the app.
In this case, we’re going to use the [provider](https://pub.dev/packages/provider) package to manage the app state. It allows us to write code in a reactive (or data-driven) style.
The most important screens in the app include:
* The authentication screen, watching the signed-in state, makes sure that only authenticated users can take notes
* The note list screen, displaying the most recent state of the notes, should react to changes to any note
* The note editor should also be reactive to outside modifications of the particular note being edited
Via providers, we can fulfill the above requirements easier, with a cleaner codebase.
With the scheme in mind, let’s begin writing the code.
To use ```provider``` and the Firebase SDKs, we have to add the dependencies to the ```pubspec.yaml``` file:

```
provider: ^4.0.2
firebase_core: ^0.4.4
firebase_auth: ^0.15.4
cloud_firestore: ^0.13.3
google_sign_in: ^4.1.4
```
Please follow the [detailed instructions](https://firebase.google.com/docs/flutter/setup) to integrate Firebase SDKs for both Android and iOS, also the [Web platform](https://firebase.google.com/docs/web/setup).
Entry of the app
Remember that we have to reject unauthenticated users, let’s build a gatekeeper widget to the root.

<script src="https://gist.github.com/xinthink/7d1ad8cc4421f50266d3406342430c10.js"></script>
main.dart

The ```StreamProvider/Consumer``` pair here is to watch the ```onAuthStateChanged```, a stream of Firebase authentication events, whenever the signed-in state has changed, the ```Consumer``` will get notified and re-build the widget according to the current state.
A little trick here is to use a ```CusttentUser``` to wrap the ```FirebaseUser```, to distinguish the default initial and the unauthenticated state, both of which are ```null```.

<script src="https://gist.github.com/xinthink/9d0853544425791c1aee55eb78900b72.js"></script>
current_user.dart

## Google Sign-in & Firebase Auth
I use Google Sign-in as an example because it’s easy to integrate. In fact, it is just one of the many services supported by Firebase Auth. You can enable what you need in the Firebase console.
![Available authentication providers](https://iswift.ru/images/1_p28rWu_gssWRU4xwb5NTQg.png)
Available authentication providers
