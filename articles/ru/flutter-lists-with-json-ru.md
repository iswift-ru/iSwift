# Flutter: выводим список из полученного JSON (Rest API)

Привет друзья,

Эта статья является продолжением моей предыдущей [статьи](http://tphangout.com/flutter-making-http-requests/). Я хотел бы вас попросить пердварительно с ней ознакомиться.

Изучить основы Flutter можно на моих курсах со скидкой [здесь](https://www.udemy.com/learn-flutter-from-scratch/?couponCode=SPECIALOFF).

Другие курсы по Flutter – [здесь](https://click.linksynergy.com/deeplink?id=VLPjcBMbd18&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourses%2Fsearch%2F%3Fq%3Dflutter). 

Давайте приступим.

Видео к этой статье будет находиться здесь.
[![Video](https://iswift.ru/images/2020-03-31_13-16-46.png)](https://youtu.be/POexDexSpKE)


Давайте продолжим с того места где остановились в прошлой статье.

В нашей прошлой статье мы разобрались как делать запрос http и получили JSON ответ, правильно?


Теперь откройте файл main.dart  и модифицируйте его как указано ниже.

<script src="https://gist.github.com/iswift-ru/384f4b90218bb3260ec077da6c3a04f9.js"></script>

Давайте разберёмся в этом ниже.

Во первых, я удалил кнопку из центра экрана.  Вместо кнопки, я сделал автоматический http запрос, когда приложение загружается. Я переопределил функцию initState() и сделал вызов.

Как только я получил данные, я просто создал список с набором плиток и из полученного ответа я использовал имя, телефон, аватарку. 

Now run the app and you will see the below screen. Сейчас мы запустимприложение и вы увидите как на экране ниже.
![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-14-e1526746144606.png)

Круто, верно?

Now let’s write some code to interact with this list. Let’s do a simple onTap interaction. When someone taps a list item, he’ll be taken to a different page where he could see the profile picture or any other details of the particular list item he tapped on.

Open up main.dart and modify it as shown below.

<script src="https://gist.github.com/iswift-ru/47022cea476b65de3dd47421eb00317b.js"></script>

I have created a new class called SecondPage in which I am placing a box on the screen. I am giving a border to this box and then giving a radius of half the value of the height and width of the container in which the box is present. Which means it will now be a circular border.

Then I am simply creating a Material page route and passing the data of the tapped list element. This is then caught by the constructor in the SecondPage class and stored.

Now if you run the app, you will see the below screen.

For instance let’s tap the first element.

![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-22-e1526746401874.png)

Try tapping a different list item. The image of that item would be shown here.

![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-31-e1526746448563.png)

Cool right? That’s it.

In this post we have seen a very simple way to structure and present the data that we receive from a http request.

If you found this helpful, kindly share it with someone and help them too.

Join our Flutter Community – [here](https://discord.gg/bCSDgVG).

Thanks for reading. Peace.. 
