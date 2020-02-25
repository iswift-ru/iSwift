# Запись, чтение, обновление и удаление информации в Realtime database во Flutter
## Cloud firestore < Realtime database
![Cloud firestore](https://iswift.ru/images/1_Ld2cOA4_fiEhejTmQixYkQ.jpeg)
<p align="center">Realtime db и cloud firestore</p>

 Я знаю какая будет у вас реакция на это
![Cloud firestore](https://iswift.ru/images/reaciya.png)  

> Cloud Firestore это новый флагман среди баз данных для разработки мобильных приложений от компании Firebase. 
Благодаря новой, более интуитивно понятной модели данных Realtime Database становится более удобной.
Cloud Firestore также обладает более богатыми, быстрыми запросами и масштабируется лучше, чем Realtime Database.

Так как мы все знаем, что cloud firestore является базой данных нового поколения, как это возможно, что realtime database бывает лучше, чем cloud firestore. Вот почему я здесь.

Давайте рассмотрим несколько ситуаций, когда realtime database более разумно использовать чем Firestore —

1. Первая ситуация - это ситуация, когда ваше приложение похоже на приложение рисования в реальном времени или что-то такое, в котором несколько пользователей обновляют базу данных и считывают данные из базы данных одновременно каждую долю секунд. В Firestore это может стоить Вам тонну денег, потому что Firestore берёт оплату за превышение лимита количества запросов на чтение и запись.

2. Во-вторых, realtime database - это зрелый продукт, и вы можете ожидать его стабильной работы т.к. это испытанный и провереныый продукт.

3. В третьем случае можно использовать обе базы данных вместе в одном приложении или проекте Firebase, чтобы уменьшить счет.

4. Первый и третий случаи применяются, если вас беспокоит ваш счет за Firebase, потому что realtime database взимает плату только за пропускную способность и хранение, а не за каждое чтение и запись.


Думаю, сейчас вы поняли

![Cloud firestore](https://iswift.ru/images/2020-02-21_15-54-07.png)  
<p align="center">Да, теперь давай и скажи, что ты хочешь сказать!</p>

*<strong>Давайте посмотрим, как мы можем использовать realtime database. Во Flutter всё делается достаточно просто.</strong>*
### Создание

```
//you can use "push" if you want an automated id, I'm giving my document id here.
FirebaseDatabase.instance.reference().child('recent').child('id')
.set({
'title': 'Realtime db rocks',
'created_at': time
});
```

### Чтение

```
//database referene.
var recentJobsRef =  recentJobsRef = FirebaseDatabase.instance
.reference()
.child('recent')
.orderByChild('created_at')  //order by creation time.
.limitToFirst(10);           //limited to get only 10 documents.
//Now you can use stream builder to get the data.
StreamBuilder(
stream: recentJobsRef.onValue,
builder: (context, snap) {
if (snap.hasData && !snap.hasError && snap.data.snapshot.value!=null) {
 
//taking the data snapshot.
DataSnapshot snapshot = snap.data.snapshot;
List item=[];
List _list=[];
//it gives all the documents in this list.
_list=snapshot.value; 
//Now we're just checking if document is not null then add it to another list called "item".
//I faced this problem it works fine without null check until you remove a document and then your stream reads data including the removed one with a null value(if you have some better approach let me know).
_list.forEach((f){
if(f!=null){
item.add(f);
     }
    }
  );
return snap.data.snapshot.value == null
//return sizedbox if there's nothing in database.
? SizedBox()
//otherwise return a list of widgets.
: ListView.builder(
scrollDirection: Axis.horizontal,
itemCount: item.length,
itemBuilder: (context, index) {
return _containerForRecentJobs(
item[index]['title']
);
},
);
} else {
return   Center(child: CircularProgressIndicator());
}
},
),
```

### Обновление

```
FirebaseDatabase.reference()
.child('recent')
.child('id')
.update({
'title':'sadab is amazing'   //yes I know.
});
```

### Удаление

```
//remove() is equivalent to calling set(null)
recentJobRef.child('id').remove();
```

**Вот и всё, если у вас есть какие-то предложения или критика, я с нетерпением жду их.**


Я знаю, что после прочтения этой статьи вы будете изучать *Как натренировать своего дракона.*

![Cloud firestore](https://iswift.ru/images/2020-02-21_16-14-31.png)
<p align="center"> Я знаю Вас… </p>

Но подождите секунду и оцените или покритикуйте эту статью.

А теперь идите вперёд беззубый дракон уже ждёт вас

Автор Md Sadab Wasim
