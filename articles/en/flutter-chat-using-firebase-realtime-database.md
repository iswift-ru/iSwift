# Flutter Chat using Firebase Realtime Database
![flutter firebase database tutorial](https://iswift.ru/images/flutter-firebase-database-tutorial.jpg)
Let’s explore firebase realtime database by creating a simple chat app. where any one can post message publically

## Step 1: Configure Firebase & Flutter
Before starting this, You must complete: [Firebase Flutter Setup](https://flutterowl.com/flutter-firebase-setup)

## Step 2 : Adding Dependencies
Once you have completed setup. now we can add required packages with your ```pubspec.yaml```. In our case we need to add realtime database (```firebase_database```), ```intl``` (for timestamp handling)

```
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^0.1.2
  firebase_core: ^0.4.0+6
  firebase_database: ^3.0.4
  intl: ^0.15.8
  ```
  
 ## Step 3: Working with Code
Now we can start writing code with our ```main.dart```

Let’s create a db reference ```chat``` to store our messages. & ```_txtCtrl``` for handling user inputs
INSERT DATA TO FIREBASE REALTIME DATABASE
```sendMessage() {
    _firebaseRef.push().set({
        "message": _txtCtrl.text,
        "timestamp": DateTime.now().millisecondsSinceEpoch
    });
}
```

this code can insert(push) data into firebase

DELETE DATA TO FIREBASE REALTIME DATABASE

```
deleteMessage(key) {
    _firebaseRef.child(key).remove();
}
```

Using this code, we delete child with key

UPDATE DATA TO FIREBASE REALTIME DATABASE
```
updateTimeStamp(key) {
    _firebaseRef
        .child(key)
        .update({"timestamp": DateTime.now().millisecondsSinceEpoch});
}
```

Using this code, we’re updating timestamp of particular message based on key

READ DATA FROM FIREBASE REALTIME DATABASE
To display firebase data into our app, we need ```StreamBuilder.```

```
StreamBuilder(
    stream: _firebaseRef.onValue,
    builder: (context, snap) {

        if (snap.hasData && !snap.hasError && snap.data.snapshot.value != null) {
            
            Map data = snap.data.snapshot.value;
            List item = [];
            
            data.forEach((index, data) => item.add({"key": index, ...data}));

            return ListView.builder(
                    itemCount: item.length,
                    itemBuilder: (context, index) {
                    return ListTile(
                        title: Text(item[index]['message']),
                        trailing: Text(DateFormat("hh:mm:ss")
                            .format(DateTime.fromMicrosecondsSinceEpoch(
                                item[index]['timestamp'] * 1000))
                            .toString()),
                        onTap: () =>
                            updateTimeStamp(item[index]['key']),
                        onLongPress: () =>
                            deleteMessage(item[index]['key']),
                    );
                    },
                );
        }
        else
            return Text("No data");
    },
)
```

**Explanation:**

* We created StreamBuilder based on _firebaseRef.onValue, which means, we update stream builder whenever data changes on firebase (CRUD)
* If there is an error with data or null, we display Text Widget with “No data”
* If there is no error, we’re storing all messages (snap.data.snapshot.value) into Map data
* Since firebase key are unpredictable, we have used forEach to push data into item

Before
```
 {-Lk30mSI-ObTUuy730c4: {message: gbrjv dl, timestamp: 1563435680036}, -Lk30mXd97oFihH_5MVR: {message: gbrjv dl, timestamp: 1563435679414}, -Lk30izl-cQ7IdtKeo4i: {message: ghrhr, timestamp: 1563435678204}}
 ```
 After
 ```
 [{key: -Lk30mSI-ObTUuy730c4, message: gbrjv dl, timestamp: 1563435680036}, {key: -Lk30mXd97oFihH_5MVR, message: gbrjv dl, timestamp: 1563435679414}, {key: -Lk30izl-cQ7IdtKeo4i, message: ghrhr, timestamp: 1563435678204}]
 ```
* Once everything is fine, we use ListView.builder() & ListTile for displaying data
* When user Tap, we update timestamp & with longpress, we delete data
This code we display a TextField with Buttons

```
Container( child: Row(children: <Widget>[
        Expanded(child: TextField(controller: _txtCtrl)),
        SizedBox(
            width: 80,
            child: OutlineButton(child: Text("Add"), onPressed: () => sendMessage()))
    ])
);
```
![chat](https://iswift.ru/images/chat1.png)![chat](https://iswift.ru/images/chat2.png)
![chat](https://iswift.ru/images/chat3.png)![chat](https://iswift.ru/images/chat4.png)
