# Flutter - Firebase Realtime действия с базой данных
Firebase Realtime Database это база данных в облочном хостинге, она помогает хранить и синхронизировать данные с базой данных NoSQL в реальном времени при каждом подключении клиентской платформы,  таких как **Android, iOS, and Web**

![flutter firebase database](https://iswift.ru/images/flutter_firebase_database.png "flutter firebase database")

Nowadays NoSQL databases are gaining popularity and Firebase Realtime Database is one of the NoSQL databases. В наше время базы данных NoSQL набирают популярность и Firebase Realtime Database одна из этих NoSQL баз. Firebase хранит все данные в JSON формате и любые изменения в данных немедленно отражаются путем выполнения операции синхронизации на всех платформах. Это позволяет нам легко создавать гибкое приложение в реальном времени с минимальными усилиями.

## Firebase Realtime Database особенности
* **Реальное время**: – Это означает что данные синхронизируются со всеми подключёнными устройствами в течение миллисикунд после изменения данных.
* **Offline**: - Firebase Realtime Database сохраняет данные на вашем устрйостве. Данные всегда доступны, даже в автономном режиме. Данные автоматически синхронизируются, когда приложение вернётся в интернет.
* **Security**: – You can define your security rules which controls who has what access to your Firebase database. Вы можете управлять правилами безопасности, для определения кто имеет доступ к вашей базе данных Firebase. Вы также можете интегрировать Firebase Авторизацию для контроля получения доступа к данным.


[![youtube](https://iswift.ru/images/2020-02-28_12-09-19.png)](https://youtu.be/SjgaBYGsEYw)

В этой статье, мы рассмотрим базовую интеграцию с Firebase Realtime базой данных и создадим Флаттер приложение, которое позволит нам выполнять CRUD действия в виджете ListView в базе данных Firebase.  

Если Вы новичёк в Firebase, я советую вам прочитать нашу другую статью о Конфигурации и авторизации Firebase, что улучшит понимание материала. Исходя из предыдущего предложения, мы предполагаем, что вы настроили приложение Flutter с консолью Firebase. 

Перед погружением в разработку приложения, я хотел бы дать вам базовую информацию о выполнении CRUD действий и других аспектов работы в базе данных в режиме реального времени. Позднее мы объединим все эти понятия вместе для создания простого приложения с Firebase realtime базой данных как бэк-енд.

## Как данные хранятся в Firebase облаке?
Как нам известно, база данных Firebase realtime это отсутствие схем и базирование на NoSQL базе данных. Мы можем хранить всю базу данных в большом JSON дереве с различными узлами. Итак, перед началом разработки приложения. Мы должны подготовить JSON структуру таким способом, чтобы данные были легко доступны для обхода вложенных дочерних узлов. Как вы увидите ниже, мы приготовили структуру для этого примера приложения. Мы храним список и подсчитываем кол-во пользовательских профилей.  

![NoSQL](https://iswift.ru/images/nosql.png "NoSQL")

### Как включить автономное сохранение в Flutter?
Firebase обеспечивает великолепную поддержку когда ваше приложение в автономном режиме. Он автоматически сохраняет данные, когда вы в режиме оффлайн, т.е. нету интернет подключения. When the device connects to the internet, all the data will be pushed to the realtime database. Когда устройство подключается к интернету, все данные отправляются в базу данных Realtime. Disk persistence can be enabled by calling below code. Сохранение на диск может быть включено путем вызова кода ниже.


**Persistence Setting**

```
FirebaseDatabase database = new FirebaseDatabase();
 database.setPersistenceEnabled(true);
 database.setPersistenceCacheSizeBytes(10000000);   
 ```

### Как управлять Безопасностью и Правами?
Firebase имеет отличный способ обеспечить безопасность базы данных. Мы можем использовать способ определения роли пользователя при выполнении операций чтения и записи. Эти правила будут действовать на уровне безопасности сервера перед выполнением каких либо CRUD операций. Давайте проверим это.

1. Ниже указаны правила позволяющие **авторизованным** пользователям проводить операции чтения и записи.

**Security & Rules**
```
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}       
```
2. Ниже указаны правила, при которых ни кто не может читать и записывать данные в базу без прохождения авторизации. Мы будем использовать эти правила в демо приложении. 

**Security & Rules**
```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
3. Вы так же можете использовать следующие правила для проверки данных перед вставкой в базу данных. Вы можете проверять имя до 50 символов и email используя регулярные выражения.

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

### Как можно выполнить CRUD действия?
Для выполнения какой либо операции с базой данных назависимо от того чтение это или запись, вы должны получить ссылку на узел в базе данных. The below code gives you reference to database JSON top node. Ниже приведён код который даёт вам ссылку на базу данных в JSON на узел верхнего уровня. Отсюда необходимо использовать имена дочерних узлов для дальнейшего перехода.

**Getting node refernce**

```
FirebaseDatabase database = new FirebaseDatabase();
DatabaseReference _userRef;=database.reference().child('user');   
```

### Вставка данных
Для вставки данных вы должны использовать метод отправки (ключ, значение) указывая путь в базе данных. Это создаст значение в указанном месте. Как вы можете видеть ниже, мы вставили информацию о пользователе в узел пользователя.

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

### Чтение данных
Прочитать данные вы можете используя FirebaseAnimatedList виджет плагина от базы данных Firebase.  В котором мы передаём главную ссылку на узел в параметре запроса. Это событие будет вызываться каждый раз когда будут изменены данные в реальном времени.  Как вы можете видеть ниже каждый параметр определяет свой элемент. В itemBuilder параметре вы будите получать актуальное значение элемента.

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

### Обновление данных
Для обновления данных **update(key, value)** используется метод передачи нового значения.

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
### Удаление дынных
Для удаления данных, вы можете просто вызвать метод remove() в базе данных по ссылке.

**delete**

```
 void deleteUser(User user) async {
    await _userRef.child(user.id).remove().then((_) {
      print('Transaction  committed.');
    });
  }
```
Теперь вы имеете достаточно знаний для начала работы с Флаттер приложением. Давайте создадим его и посмотрим, как интегрировать базу данных в реальном времени с примером приложением. Ниже приведены шаги, которые необходимо выполнить чтобы запутить наш проект используя Firebase realtime database.

### Создаём новый проект
№1. Создаём новый проект в вашей среде разработки IDE, File ⇒ New Flutter Project.
№2. Теперь настроим ваше приложение с облаком Firebase. Как объяснено здесь.
№3. Откройте pubspec.yaml файл и добавьте зависимость firebase_database: ^1.0.5 
№4. Откройте файл main.dart и отредактируйте его. Мы задали свою тему и удалили вывеску дебаг в правом верхнем углу приложения (приложение в разработке).

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
№5. After that create a user.dart file. It is a POJO model that'll contain user information. После этого создайте файл user.dart. Это POJO макет, который будет содержать информацию о пользователе. 

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

№6. Для простоты, я создал отдельный файл firebase_database_util.dart. Здесь я записал все CRUD методы, который мы будем использовать для взаимодействия с базой данных Firebase. Перед вызовом любого метода этого util. Для инициализации мы вызываем метод initState.

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

№7. Теперь начнём разработку пользовательского интерфейса. Для вывода на экран списка пользователей создадим другой файл user_dashboard.dart. Для лучшего понимания я закоментировал сноски в документе. Поэтому, читатйте внимательно, для лучшего понимания в следующем виджете.

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

Когда вы запустите проект, выведуться данные, как указано ниже на фотографии.

![flutter firebase database list](https://iswift.ru/images/flutter_firebase_database_list.png "flutter firebase database list")

8. Let's see final code snippet of this project. We are going to create a pop-up widget. Посмотрите финальную отрывок кода этого проекта. В нем отображаются поля ввода информации о пользователе. При добавлении и обновлении сведений о пользователе.

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

Окончательные выходные данные вышеуказанного виджета выглядят так, как показано ниже в ссылках.

![flutter firebase database popup](https://iswift.ru/images/firebase_database_entery_popup.png "flutter firebase database popup")

[Source code](https://github.com/DeveloperLibs/flutter_firebase_database "Source code")

[Android APK](https://drive.google.com/open?id=11elwEPvt9J1LhaKbCkJg-2IuOGE1zxdt "Android APK")
