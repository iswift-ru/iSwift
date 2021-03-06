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

Теперь, давайте напишим некоторый код для взаимодействия с нашим списком. Сделаем простейшее взаимодействие с помощью onTap. Когда кто то нажимает на элемент списка он будет переведён на другую страницу где увидит профиль с фотографией.


Откройте файл main.dart и модифицируйте его как показно ниже.

<script src="https://gist.github.com/iswift-ru/47022cea476b65de3dd47421eb00317b.js"></script>

Я создал новый класс под названием SecondPage в котором я в экране разместил Box. Я сделал границу этого Box, а так же радиус в половину значения высоты и ширины контейнера, который представляет box. Это означает что теперь будет граница по кругу.


Затем я просто создаю страницу Material с навигацией и передаю данные элемента списка. Затем он перехватывается конструктором в классе SecondPage и хранится. 

Теперь если вы запустите приложение, вы увидите экран как ниже.

Например, давайте коснёмся первого элемента.

![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-22-e1526746401874.png)


Попробуем нажать на другой элемент списка. Картинка другого элемента так же поменялась.

![](https://iswift.ru/images/Screenshot_2018-05-19-21-36-31-e1526746448563.png)

Правда, круто?

В этой статье мы могли увидеть очень простую структуру и представили данные которые мы получили по http запросу.
Если вы сочли эту статью полезной, просьба поделиться ею с другими.


Присоединяйтесь к [Флаттер сообществу](https://discord.gg/bCSDgVG).

Смасибо, что читали. Всем Мир!
