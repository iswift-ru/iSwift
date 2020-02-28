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

```Persistence Setting


FirebaseDatabase database = new FirebaseDatabase();
 database.setPersistenceEnabled(true);
 database.setPersistenceCacheSizeBytes(10000000);   
 ```
