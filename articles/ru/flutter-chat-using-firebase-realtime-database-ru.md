# Создаём чат на Flutter используя базу данных Firebase Realtime

![flutter firebase database tutorial](https://iswift.ru/images/flutter-firebase-database-tutorial.jpg)

Давайте создадим простое приложение с чатом используя базу данных Firebase Realtime. В котором кто то может опубликовать сообщения

## Шаг 1: Конфигурация Firebase и Flutter
Для начала вы должны завершить: [Firebase Flutter установка](https://flutterowl.com/flutter-firebase-setup)

## Шаг 2 : Добавление зависимостей
После завершения установки. Теперь нам необходимо добавить нужный пакет в ```pubspec.yaml```. В нашем слeчае, необходимо добавить Realtime Database(```firebase_database```), ```intl``` (для обработки меток времени)

```
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^0.1.2
  firebase_core: ^0.4.0+6
  firebase_database: ^3.0.4
  intl: ^0.15.8
  ```
  
## Шаг 3: Работаем с кодом
 
Сейчас мы можем начать писать код в нашем файле ```main.dart```

Теперь создадим базу данных ссылок ```chat``` для хранения наших сообщений. И ```_txtCtrl``` для обработки входных данных пользователя

ВСТАВКА ДАННЫХ В FIREBASE REALTIME БАЗУ ДАННЫХ

```sendMessage() {
    _firebaseRef.push().set({
        "message": _txtCtrl.text,
        "timestamp": DateTime.now().millisecondsSinceEpoch
    });
}
```

этот код мозволяет отправлять данные в Firebase

УДАЛЕНИЕ ДАННЫХ ИЗ FIREBASE REALTIME БАЗЫ ДАННЫХ

```
deleteMessage(key) {
    _firebaseRef.child(key).remove();
}
```

Используя этот код, мы удаляем ребёнка с ключём

ОБНОВЛЕНИЕ ДАННЫХ В FIREBASE REALTIME БАЗЕ ДАННЫХ
```
updateTimeStamp(key) {
    _firebaseRef
        .child(key)
        .update({"timestamp": DateTime.now().millisecondsSinceEpoch});
}
```


Используя этот коды, мы обновляем метку времени определённого сообщения на основе ключа

ЧТЕНИЕ ДАННЫХ ИЗ FIREBASE REALTIME БАЗЫ ДАННЫХ
Для вывода содержимого Firebase в наше приложение, нам потребуется ```StreamBuilder.```


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

**Объяснение:**

* Мы создали StreamBuilder основываясь на _firebaseRef.onValue, что означает, мы обновим  stream builder каждый раз как только поменяются данные в firebase (CRUD Запись, чтение, обновление, удаление)

* При наличии ошибки с данными или их нет, на экран выведится виджет “No data”
* Если ошибки нет, мы сохраним все сообщения (snap.data.snapshot.value) в Map (карту) данных
* Так как firebase ключи непредсказуемые, мы использовали forEach для отправки данных в элемент

До
```
 {-Lk30mSI-ObTUuy730c4: {message: gbrjv dl, timestamp: 1563435680036}, -Lk30mXd97oFihH_5MVR: {message: gbrjv dl, timestamp: 1563435679414}, -Lk30izl-cQ7IdtKeo4i: {message: ghrhr, timestamp: 1563435678204}}
 ```
 После
 ```
 [{key: -Lk30mSI-ObTUuy730c4, message: gbrjv dl, timestamp: 1563435680036}, {key: -Lk30mXd97oFihH_5MVR, message: gbrjv dl, timestamp: 1563435679414}, {key: -Lk30izl-cQ7IdtKeo4i, message: ghrhr, timestamp: 1563435678204}]
 ```
* Чтобы всё выводилось хорошо мы использовали ListView.builder() и ListTile
* Когда пользователь Нажимает кнопку, мы обновляем метку времени а при длительном нажатии мы удаляем данные 
Этот код выводит TextField и Buttons

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


