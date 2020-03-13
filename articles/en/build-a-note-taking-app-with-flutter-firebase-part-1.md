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

## Entry of the app
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
