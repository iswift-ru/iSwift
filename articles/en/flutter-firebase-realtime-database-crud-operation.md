# Flutter - Firebase Realtime Database CRUD Operation
Firebase Realtime Database is a cloud-hosted database that helps us to store and sync data with NoSQL database in realtime to every connected client platforms like **Android, iOS, and Web**.
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

```
Persistence Setting

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
