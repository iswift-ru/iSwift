# Flutter - Firebase Realtime действия с базой данных
Firebase Realtime Database это база данных в облочном хостинге, она помогает хранить и синхронизировать данные с базой данных NoSQL в реальном времени при каждом подключении клиентской платформы,  таких как **Android, iOS, and Web**
![flutter firebase database](https://iswift.ru/images/flutter_firebase_database.png "flutter firebase database")

Nowadays NoSQL databases are gaining popularity and Firebase Realtime Database is one of the NoSQL databases. Firebase store all the data in JSON format and any changes in data reflects immediately by performing a sync operation across all the platforms. This allows us to build a flexible realtime app easily with minimal efforts. 

## Firebase Realtime Database feature 
* **Realtime**: – It means the data is synchronized with all connected devices within milliseconds of changes being made to the data.
* **Offline**: - The Firebase Realtime Database persists data on your device. The data is always available, even app offline. The data is automatically synced once the app comes back online.
* **Security**: – You can define your security rules which controls who has what access to your Firebase database. You can also integrate Firebase Authentication to control access to your data.


[![youtube](https://iswift.ru/images/2020-02-28_12-09-19.png)](https://youtu.be/SjgaBYGsEYw)

In this post, we'll see the basics integration of Firebase realtime database and build a Flutter App that allows us to perform the CRUD operations with Firebase Database in a ListView. The final output of the example look like above video.  

If you are new to Firebase, I suggest you read our other post about Firebase configuration and Firebase Auth to improve your knowledge. On the basis of the previous post, we assuming you have configured Flutter app with Firebase console. 

Before dive into the application development, i would like to give you basic information about performing CRUD operations and other aspects of the realtime database. Later we’ll combine all these concepts together to build a simple app with Firebase realtime database as back-end. 

## How data store on Firebase cloud?
As we know, the Firebase realtime database is a schema-less and NoSQL based database. We can store the entire database in a big JSON tree with multiple nodes format. So, before start application development. We need to prepare the JSON structure in a way that the data is accessible in an easier way by avoiding nesting of child nodes. As you can see below,  we have prepared a structure for this example application. We storing a list of user profiles and user count in the JSON tree. 

![NoSQL](https://iswift.ru/images/nosql.png "NoSQL")

### How can enable offline persistence in Flutter?
Firebase provides great support when your application in offline mode. It automatically stores the data offline when there is no internet connection. When the device connects to the internet, all the data will be pushed to the realtime database. Disk persistence can be enabled by calling below code. 


**Persistence Setting**

```
FirebaseDatabase database = new FirebaseDatabase();
 database.setPersistenceEnabled(true);
 database.setPersistenceCacheSizeBytes(10000000);   
 ```

### How can manage Security & Rules?
Firebase provides a great way to make database secure. We can apply a way to identify user role while performing read and write operations. These rules will acts a security layer on the server before perform any CRUD operation. Let's check it.

1. The below rules allow authenticated users can perform read or write operation. 

**Security & Rules**
```
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}       
```
2. Below rules allow everyone can read & write data without authentication. We'll use it in the demo app.

**Security & Rules**
```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
3. You can also use the following rules to validate data before inserting into the database. You can validate the name to be less than 50 chars and email to be valid using email regular expression.

**Security & Rules**
```
{
    "rules": {
        ".read": true,
        ".write": true,
        "users": {
            "$user": {
                "name": {
                    ".validate": "newData.isString() && newData.val().length < 50"
                },
                "email": {
                    ".validate": "newData.isString() && newData.val().matches(/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\\.[A-Z]{2,4}$/i)"
                }
            }
        }
    }
}
```

### How can perform CRUD Operations? 
To perform any operation on to database whether it can be read or write, you need to get the reference of database node. The below code gives you reference to database JSON top node. From here you need to use the child node names to traverse further.

**Getting node refernce**

```
FirebaseDatabase database = new FirebaseDatabase();
DatabaseReference _userRef;=database.reference().child('user');   
```

### Inserting Data
To insert data, you can use set(key, value) method on to database reference path. This will create the value on the path provided to a given reference. As you can see below, we inserting a user information to user reference node. 

**Add a node in Database**
```
addUser(User user) async {
    final TransactionResult transactionResult =
        await _counterRef.runTransaction((MutableData mutableData) async {
      mutableData.value = (mutableData.value ?? 0) + 1;

      return mutableData;
    });

    if (transactionResult.committed) {
      _userRef.push().set(<String, String>{
        "name": "" + user.name,
        "age": "" + user.age,
        "email": "" + user.email,
        "mobile": "" + user.mobile,
      }).then((_) {
        print('Transaction  committed.');
      });
    } else {
      print('Transaction not committed.');
      if (transactionResult.error != null) {
        print(transactionResult.error.message);
      }
    }
  }
```

### Reading Data
To read the data, we can use FirebaseAnimatedList widget of Firebase database plugin. In which, we pass the root reference of a node in the query parameter. This event will be triggered whenever there is a change in data in realtime. As you can see below, as each parameter define its feature. In the itemBuilder parameter, we'll get the actual value of an item.

**FirebaseAnimatedList**

```
body: new FirebaseAnimatedList(
        key: new ValueKey<bool>(_anchorToBottom),
        query: databaseUtil.getUser(),
        reverse: _anchorToBottom,
        sort: _anchorToBottom
            ? (DataSnapshot a, DataSnapshot b) => b.key.compareTo(a.key)
            : null,
        itemBuilder: (BuildContext context, DataSnapshot snapshot,
            Animation<double> animation, int index) {
          return new SizeTransition(
            sizeFactor: animation,
            child: showUser(snapshot),
          );
        },
      ),
```

### Updating Data
To update data, you can use the **update(key, value)** method by passing the new value. 

**Updating data**

```
void updateUser(User user) async {
    await _userRef.child(user.id).update({
      "name": "" + user.name,
      "age": "" + user.age,
      "email": "" + user.email,
      "mobile": "" + user.mobile,
    }).then((_) {
      print('Transaction  committed.');
    });
  }
```  
### Deleting Data
To delete data, you can simply call remove() method on to database reference. 

**delete**

```
 void deleteUser(User user) async {
    await _userRef.child(user.id).remove().then((_) {
      print('Transaction  committed.');
    });
  }
```
Now we have enough knowledge to get started with a Flutter application. Let’s create one and see how to integrate the realtime database with an example app. Below are the steps you need to follow to set up your project for using Firebase realtime database. 

### Creating a new Project
1. Create a new project from File ⇒ New Flutter Project with your development IDE.
2. Now, configure your application with Firebase cloud. As explained here.
3. Open pubspec.yaml file and add firebase_database: ^1.0.5 dependency.
4. Open main.dart file and edit it. As we have set our theme and change debug banner property of Application.

**main.dart**

```
import 'package:flutter/material.dart';
import 'package:flutter_firebase_auth/user_dashboard.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      theme: new ThemeData(
        primaryColor: const Color(0xFF02BB9F),
        primaryColorDark: const Color(0xFF167F67),
        accentColor: const Color(0xFF167F67),
      ),
      debugShowCheckedModeBanner: false,
      title: 'Firebase',
      home: new UserDashboard(),
    );
  }
}
```
5. After that create a user.dart file. It is a POJO model that'll contain user information.

**user.dart**

```
import 'package:firebase_database/firebase_database.dart';

class User {

  String _id;
  String _name;
  String _email;
  String _age;
  String _mobile;

  User(this._id,this._name, this._email, this._age, this._mobile);

  String get name => _name;

  String get email => _email;

  String get age => _age;

  String get mobile => _mobile;

  String get id => _id;

  User.fromSnapshot(DataSnapshot snapshot) {
    _id = snapshot.key;
    _name = snapshot.value['name'];
    _email = snapshot.value['email'];
    _age = snapshot.value['age'];
    _mobile = snapshot.value['mobile'];
  }
}
```

6. For code simplicity, i have created a separate file firebase_database_util.dart. Here, i have written all CRUD methods that we'll use to interact with firebase database. Before calling any method of this util. We have to call the initState method to initialize it. 

**firebase_database_util.dart**

```
import 'dart:async';
import 'package:firebase_database/firebase_database.dart';
import 'package:flutter_firebase_auth/user.dart';

class FirebaseDatabaseUtil {
  DatabaseReference _counterRef;
  DatabaseReference _userRef;
  StreamSubscription<Event> _counterSubscription;
  StreamSubscription<Event> _messagesSubscription;
  FirebaseDatabase database = new FirebaseDatabase();
  int _counter;
  DatabaseError error;

  static final FirebaseDatabaseUtil _instance =
      new FirebaseDatabaseUtil.internal();

  FirebaseDatabaseUtil.internal();

  factory FirebaseDatabaseUtil() {
    return _instance;
  }

  void initState() {
    // Demonstrates configuring to the database using a file
    _counterRef = FirebaseDatabase.instance.reference().child('counter');
    // Demonstrates configuring the database directly

    _userRef = database.reference().child('user');
    database.reference().child('counter').once().then((DataSnapshot snapshot) {
      print('Connected to second database and read ${snapshot.value}');
    });
    database.setPersistenceEnabled(true);
    database.setPersistenceCacheSizeBytes(10000000);
    _counterRef.keepSynced(true);

    _counterSubscription = _counterRef.onValue.listen((Event event) {
      error = null;
      _counter = event.snapshot.value ?? 0;
    }, onError: (Object o) {
      error = o;
    });
  }

  DatabaseError getError() {
    return error;
  }

  int getCounter() {
    return _counter;
  }

  DatabaseReference getUser() {
    return _userRef;
  }

  addUser(User user) async {
    final TransactionResult transactionResult =
        await _counterRef.runTransaction((MutableData mutableData) async {
      mutableData.value = (mutableData.value ?? 0) + 1;

      return mutableData;
    });

    if (transactionResult.committed) {
      _userRef.push().set(<String, String>{
        "name": "" + user.name,
        "age": "" + user.age,
        "email": "" + user.email,
        "mobile": "" + user.mobile,
      }).then((_) {
        print('Transaction  committed.');
      });
    } else {
      print('Transaction not committed.');
      if (transactionResult.error != null) {
        print(transactionResult.error.message);
      }
    }
  }

  void deleteUser(User user) async {
    await _userRef.child(user.id).remove().then((_) {
      print('Transaction  committed.');
    });
  }

  void updateUser(User user) async {
    await _userRef.child(user.id).update({
      "name": "" + user.name,
      "age": "" + user.age,
      "email": "" + user.email,
      "mobile": "" + user.mobile,
    }).then((_) {
      print('Transaction  committed.');
    });
  }

  void dispose() {
    _messagesSubscription.cancel();
    _counterSubscription.cancel();
  }
}
```

7. Now start the development of user interface.  To display a list of the user, we have created another file user_dashboard.dart. To understand it deeply, we have written the doc in yellow color of each snippet in this file. So,  read it carefully to the better understanding of the following widget.

**user_dashboard.dart**

```
import 'package:firebase_database/firebase_database.dart';
import 'package:firebase_database/ui/firebase_animated_list.dart';
import 'package:flutter/material.dart';
import 'package:flutter_firebase_auth/add_user_dialog.dart';
import 'package:flutter_firebase_auth/firebase_database_util.dart';
import 'package:flutter_firebase_auth/user.dart';

class UserDashboard extends StatefulWidget {
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<UserDashboard> implements AddUserCallback {

  bool _anchorToBottom = false;

 // instance of util class

 FirebaseDatabaseUtil databaseUtil;

  @override
  void initState() {
    super.initState();
    databaseUtil = new FirebaseDatabaseUtil();
    databaseUtil.initState();
  }

  @override
  void dispose() {
    super.dispose();
    databaseUtil.dispose();
  }

  @override
  Widget build(BuildContext context) {

   // it will show title of screen

   Widget _buildTitle(BuildContext context) {
      return new InkWell(
        child: new Padding(
          padding: const EdgeInsets.symmetric(horizontal: 12.0),
          child: new Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              new Text(
                'Firebase Database',
                style: new TextStyle(
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
              ),
            ],
          ),
        ),
      );
    }
//It will show new user icon
    List<Widget> _buildActions() {
      return <Widget>[
        new IconButton(
          icon: const Icon(
            Icons.group_add,
            color: Colors.white,
          ),               // display pop for new entry
          onPressed: () => showEditWidget(null, false),
        ),
      ];
    }

    return new Scaffold(
      appBar: new AppBar(
        title: _buildTitle(context),
        actions: _buildActions(),
      ), 

    // Firebase predefile list widget. It will get user info from firebase database
      body: new FirebaseAnimatedList(
        key: new ValueKey<bool>(_anchorToBottom),
        query: databaseUtil.getUser(),
        reverse: _anchorToBottom,
        sort: _anchorToBottom
            ? (DataSnapshot a, DataSnapshot b) => b.key.compareTo(a.key)
            : null,
        itemBuilder: (BuildContext context, DataSnapshot snapshot,
            Animation<double> animation, int index) {
          return new SizeTransition(
            sizeFactor: animation,
            child: showUser(snapshot),
          );
        },
      ),
    );
  } 

 @override // Call util method for add user information
  void addUser(User user) {
    setState(() {
      databaseUtil.addUser(user);
    });
  }

  @override // call util method for update old data.
  void update(User user) {
    setState(() {
      databaseUtil.updateUser(user);
    });
  } 

 //It will display a item in the list of users.

 Widget showUser(DataSnapshot res) {
    User user = User.fromSnapshot(res);

    var item = new Card(
      child: new Container(
          child: new Center(
            child: new Row(
              children: <Widget>[
                new CircleAvatar(
                  radius: 30.0,
                  child: new Text(getShortName(user)),
                  backgroundColor: const Color(0xFF20283e),
                ),
                new Expanded(
                  child: new Padding(
                    padding: EdgeInsets.all(10.0),
                    child: new Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: <Widget>[
                        new Text(
                          user.name,
                          // set some style to text
                          style: new TextStyle(
                              fontSize: 20.0, color: Colors.lightBlueAccent),
                        ),
                        new Text(
                          user.email,
                          // set some style to text
                          style: new TextStyle(
                              fontSize: 20.0, color: Colors.lightBlueAccent),
                        ),
                        new Text(
                          user.mobile,
                          // set some style to text
                          style: new TextStyle(
                              fontSize: 20.0, color: Colors.amber),
                        ),
                      ],
                    ),
                  ),
                ),
                new Column(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: <Widget>[
                    new IconButton(
                      icon: const Icon(
                        Icons.edit,
                        color: const Color(0xFF167F67),
                      ),
                      onPressed: () => showEditWidget(user, true),
                    ),
                    new IconButton(
                      icon: const Icon(Icons.delete_forever,
                          color: const Color(0xFF167F67)),
                      onPressed: () => deleteUser(user),
                    ),
                  ],
                ),
              ],
            ),
          ),
          padding: const EdgeInsets.fromLTRB(10.0, 0.0, 0.0, 0.0)),
    );

    return item;
  }
  //Get first letter from the name of user
  String getShortName(User user) {
    String shortName = "";
    if (!user.name.isEmpty) {
      shortName = user.name.substring(0, 1);
    }
    return shortName;
  }
  //Display popup in user info update mode.
  showEditWidget(User user, bool isEdit) {
    showDialog(
      context: context,
      builder: (BuildContext context) =>
          new AddUserDialog().buildAboutDialog(context, this, isEdit, user),
    );
  }
 //Delete a entry from the Firebase console.
  deleteUser(User user) {
    setState(() {
      databaseUtil.deleteUser(user);
    });
  }
}
```

The output of the above widget looks like below when you'll run this project.

![flutter firebase database list](https://iswift.ru/images/flutter_firebase_database_list.png "flutter firebase database list")

8. Let's see final code snippet of this project. We are going to create a pop-up widget. It will show user info entry fields. When we add and update user info.

**add_user_dialog.dart**

```
import 'package:flutter/material.dart';
import 'package:flutter_firebase_auth/user.dart';

class AddUserDialog {
  final teName = TextEditingController();
  final teEmail = TextEditingController();
  final teAge = TextEditingController();
  final teMobile = TextEditingController();
  User user;

  static const TextStyle linkStyle = const TextStyle(
    color: Colors.blue,
    decoration: TextDecoration.underline,
  );

  Widget buildAboutDialog(BuildContext context,
      AddUserCallback _myHomePageState, bool isEdit, User user) {
    if (user != null) {
      this.user = user;
      teName.text = user.name;
      teEmail.text = user.email;
      teAge.text = user.age;
      teMobile.text = user.mobile;
    }

    return new AlertDialog(
      title: new Text(isEdit ? 'Edit detail!' : 'Add new user!'),
      content: new SingleChildScrollView(
        child: new Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            getTextField("Name", teName),
            getTextField("Email", teEmail),
            getTextField("Age", teAge),
            getTextField("Mobile", teMobile),
            new GestureDetector(
              onTap: () => onTap(isEdit, _myHomePageState, context),
              child: new Container(
                margin: EdgeInsets.fromLTRB(10.0, 0.0, 10.0, 0.0),
                child: getAppBorderButton(isEdit ? "Edit" : "Add",
                    EdgeInsets.fromLTRB(0.0, 10.0, 0.0, 0.0)),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget getTextField(
      String inputBoxName, TextEditingController inputBoxController) {
    var loginBtn = new Padding(
      padding: const EdgeInsets.all(5.0),
      child: new TextFormField(
        controller: inputBoxController,
        decoration: new InputDecoration(
          hintText: inputBoxName,
        ),
      ),
    );

    return loginBtn;
  }

  Widget getAppBorderButton(String buttonLabel, EdgeInsets margin) {
    var loginBtn = new Container(
      margin: margin,
      padding: EdgeInsets.all(8.0),
      alignment: FractionalOffset.center,
      decoration: new BoxDecoration(
        border: Border.all(color: const Color(0xFF28324E)),
        borderRadius: new BorderRadius.all(const Radius.circular(6.0)),
      ),
      child: new Text(
        buttonLabel,
        style: new TextStyle(
          color: const Color(0xFF28324E),
          fontSize: 20.0,
          fontWeight: FontWeight.w300,
          letterSpacing: 0.3,
        ),
      ),
    );
    return loginBtn;
  }

  User getData(bool isEdit) {
    return new User(isEdit ? user.id : "", teName.text, teEmail.text,
        teAge.text, teMobile.text);
  }

  onTap(bool isEdit, AddUserCallback _myHomePageState, BuildContext context) {
    if (isEdit) {
      _myHomePageState.update(getData(isEdit));
    } else {
      _myHomePageState.addUser(getData(isEdit));
    }

   Navigator.of(context).pop(); 

 }
}
//Call back of user dashboad
abstract class AddUserCallback {
  void addUser(User user);

  void update(User user);
}
```

The final output of the above widget looks like below.

![flutter firebase database popup](https://iswift.ru/images/firebase_database_entery_popup.png "flutter firebase database popup")

[Source code](https://github.com/DeveloperLibs/flutter_firebase_database "Source code")

[Android APK](https://drive.google.com/open?id=11elwEPvt9J1LhaKbCkJg-2IuOGE1zxdt "Android APK")
