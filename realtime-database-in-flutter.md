<h1 align="center">Realtime database in flutter</h1>
<h2 align="center">Cloud firestore < Realtime database</h2>
![Cloud firestore](https://iswift.ru/images/1_Ld2cOA4_fiEhejTmQixYkQ.jpeg)
<p align="center">Realtime db vs cloud firestore</p>
I know your reaction would be like this
![Cloud firestore](https://iswift.ru/images/reaciya.png)  

> Cloud Firestore is Firebase’s new flagship database for mobile app development. It improves on the successes of the Realtime Database with a new, more intuitive data model. Cloud Firestore also features richer, faster queries and scales better than the Realtime Database.

As we all know cloud firestore is firebase next-generation database how is it possible that the realtime database becomes better that cloud firestore. That’s why I’m here.

Let’s see some situations where realtime database becomes more resonable to use than firestore —

1. The first situation is when your app is like a realtime painting app or something, in which multiple users are updating the database and reading from the database simultaneously within every fraction of seconds. In firestore it can cost you a ton of money because firestore bill is directly propotional to the number of reads and writes.

2. The second one is that realtime database is a mature product and you can expect the stability, and it’s a tried-and-true product.

3. The third case, it is possible to use both of the databases together within the same Firebase app or project to reduce your firebase bill.

4. The first and third case is applied if you’re concerned about your firebase bill that’s because realtime database charges only for bandwidth and storage and not on each read and write.

I think you got the point now

![Cloud firestore](https://iswift.ru/images/2020-02-21_15-54-07.png)  
<p align="center">Yeah got it now go ahead and tell what you want to say!!</p>

*<strong>Let’s see how can we use realtime database in the simplest way possible because it’s in flutter(which makes everything simple).</strong>*
### Create

```
//you can use "push" if you want an automated id, I'm giving my document id here.
FirebaseDatabase.instance.reference().child('recent').child('id')
.set({
'title': 'Realtime db rocks',
'created_at': time
});
```

### Read

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

### Update

```
FirebaseDatabase.reference()
.child('recent')
.child('id')
.update({
'title':'sadab is amazing'   //yes I know.
});
```

### Delete

```
//remove() is equivalent to calling set(null)
recentJobRef.child('id').remove();
```

**That’s it, if you have some suggestion or criticism, I’m eagerly waiting for it.**

I know after reading this article you’re going to watch *How to train your dragon.*

![Cloud firestore](https://iswift.ru/images/2020-02-21_16-14-31.png)
<p align="center"> I know you… </p>

But wait for a second, and appreciate or criticize this article.
Go ahead toothless dragon is waiting for you.

WRITTEN BY Md Sadab Wasim
