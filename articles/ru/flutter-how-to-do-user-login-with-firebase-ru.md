# Flutter : Как сделать авторизацию с помощью Firebase
![Flutter login with firebase](https://iswift.ru/images/1_9nmjA-EfVfu86LeGAsHlJQ.png "Flutter login with firebase")

## Выпуск
25/12/18 — Обновление последней версии кода после рефакторинга и чистки.

24/01/19 — Дублирование ссылки на GitHub в верхней части статьи.

23/07/19 — Добавление trim метода в email и password.

13/10/19 — Обновление кода и статьи после чистки и обнолвниея пакетов.

## Исходный код

В случае, если вы хотите просмотреть целиком всю эту абрукадабру вы можете взять исходный код здесь 👇

https://github.com/tattwei46/flutter_login_demo

Пример проекта Flutter - вход для пользователя в систему и процесса регистрации с аутентификацией Firebase

## Что такой Flutter?

Flutter - это мобильный SDK (Пакет программ для разработки програмного обеспечения) с открытым исходным кодом, разработанный компанией Google для построения высококачественных приложений для Android и iOS. Он позволяет разработчикам не только создавать приложения с красивым дизайном, плавной анимацией и быстрым выполнением, но и так же возможность быстрой интеграции новых функций. Флаттер предлагает быструю разработку с его горячим перезапуском приложения. Имея только одну кодовую базу для управления, вы можете сэкономить много времени по сравнению с отдельной разработкой Android и iOS проектов, поскольку Flutter компилирует его с собственным ARM кодом. Флаттер использует язык программирования Дарт, который также разработал Гугл.

## Почему Dart?

* Если коротко то это строго типизированный, объектно ориентированный язык.
* Поддерживает just-in-time (JIT) и ahead-of-time (AOT) компиляцию.
* JIT позволяет Flutter перекомпилировать код непосредственно на устройстве, пока приложение еще работает.
* Обеспечивает быструю разработку и стабильную "горячую перезагрузку" в течение секунды. 
* AOT позволяет компилировать код непосредственно в собственный ARM-код, что обеспечивает быстрый запуск и предсказуемую производительность.

## Что такое Firebase

Firebase это платформа для мобильной и web разработки, которая позволяет разработчику делать широкий ассортимент продуктов. Сегодня мы будем изучать как мы сможем сделать наше первое Флаттер приложение спомощью Firebase авторизации и realtime database (RTDB).  Это приложение позволит пользователям регистрироваться или входить в аккаунт и совершать CRUD (создание, чтение, обновление, удаление) действия над записями спомощью Firebase. В этом статье, мы сфокусируемся только на регистрации и авторизации пользователей.

## Какие версии интсрументов используются?

Здесь  перечислены версии инструментов которые изпользуются в статье и в гитхабе.
* Flutter Version: flutter_macos_v1.9.1+hotfix.4-stable
* Android Studio Version: 3.5
* Xcode Version: 11.1

## Как установить окружающую среду?

* Следуйте инструкция по ссылке https://flutter.dev/docs/get-started/install чтобы установить Flutter SDK. Вам следует установить или Android Studio или VSCode с Flutter и Dart плагинами.
* Запустите Flutter doctor для установки зависимостей

```flutter doctor```

* Для использования IOS эмулятора сделайте команду:

```open -a Simulator```

* для использования android эмулятора, выполните следующие действия: Следуйте **Android Studio > tools > AVD Manager** и выбирите создание **Virtual Device.**

## Создание Flutter приложения

You can get the complete source code in the GitHub link at the bottom of the post. Вы можете использовать готовый исходный код на ГитХабе по ссылке в верхней части статьи. Ниже показано как мы написали исходный код для флаттер приложения опубликованный на ГитХабе.

### 👉Шаг 1: Создание нового Flutter проекта под названием "Flutter login demo"
Запускаем эмулятор и запускаем проект с помощью Флаттер.

```flutter run```

Для запуска Android и iOS эмулятора выполните следующую команду.

```flutter run -d all```

Вы должны увидеть симулятор экранов Андройд и ИОС.
![Android](https://iswift.ru/images/1_s5aA2j3HJAONXq1RrBLpGA.png "Android")![IOS](https://iswift.ru/images/1_VdFSnH_gOFIG2QGBY_N9ZA.png "IOS")


>Если вам интересно знать как сделать скриншоты в ваших симуляторах;
>
>Для Android: Просто нажмите на иконку "камера" на левой стороне панели инструментов. Картинка будет сохранена на рабочем столе.
>
>Для iOS: [Вариант 1] Нажмите вместе кнопки command + shift + 4. Нажмите клавишу пробела, чтобы изменить указатель мыши на значок камеры.
>
>Наведите курсор на симулятор iOS, щелкните, чтобы сделать снимок экрана. Картинка будет сохранена на рабочий стол.
>
>Для iOS: [Вариант 2] Выбирите симулятор и нажмите command + S. Спасибо JerryZhou за то, что поделился этой информацией.

### 👉Шаг 2: Замена оригинального кода на Привет Мир (Hello World)
At main.dart, erase all contents and add the following boilerplate to your file.В файле main.dart удалите все содержимое и добавьте в файл следующующий шаблон. Вы должны создать новый файл под названием login_signup_page.dart в котором будет LoginSignupPage класс. В терминале нажмите клавишу R, чтобы выполнить горячую перезагрузку, и вы должны увидеть "Hello World" на экране.

<script src="https://gist.github.com/tattwei46/4f5a72c294781b89f835f969acc20267.js"></script>
<script src="https://gist.github.com/tattwei46/43ad1fbda618e436a2ecb115786c00bc.js"></script>

![Hello world](https://iswift.ru/images/1_gG7KSUYl1Elmn9OyjPP-Vg.png "Hello world")

### 👉Шаг 3: Меняем состояние виджета с stateless на stateful
<script src="https://gist.github.com/tattwei46/33cf7585688ad6d964b49a89da47a514.js"></script>

### 👉Шаг 4: Замена Hello World на Stack
Внутри Scaffold body, заменим текстовый виджет Hello Word на stack виджет. Stack виджет позволит нам наложить один виджет на другой.  Идея в том, чтобы показывать круговой процесс загрузки , когда кто то выполняет действие регистрирации или авторизации. В Stack мы будем иметь Форму и кргуовой индикатор прогресса. Внутри Form мы добавим ListView, который позволит нам вставить массив виджетов. Таким образом, мы можем выполнить рефакторинг нескольких компонентов пользовательского интерфейса (UI) и поместить их в ListView. 

***Профессиональный совет*** : *Всякий раз, когда мы используем ввод текста, лучше обернуть его виджетом ListView, чтобы предотвратить ошибку рендеринга (отрисовки) при появлении виртуальной клавиатуры из-за переполнения пикселов.*

<script src="https://gist.github.com/tattwei46/bac22f40a192fa6677263d2a6a29363d.js"></script>

### 👉Шаг 5: Делаем другие компоненты пользовательского интерфейса
Мы начали с построения нашего индикатора прогресса загрузки. Спасибо Флаттеру, это доступно через вызов виджета ```CircularProgressIndicator``` . Мы будем использовать ```bool _isLoading``` не зависимо от того будет ли показываться ```CircularProgressIndicator``` или нет. 

<script src="https://gist.github.com/tattwei46/e6157ce651461de1b8171c8cb5e2f8fb.js"></script>

Дальше, мы сделаем логотип для нашей Form. В репозитории имеется папка asset со следующим файлом flutter-icon.png. Чтобы импортировать в наш проект, добавьте следующую строку в pubspec.yaml, а затем команду "flutter packages get".

<script src="https://gist.github.com/tattwei46/6f5beb00e244f28dc0676b5af217e9bf.js"></script>
<script src="https://gist.github.com/tattwei46/3f0b3b871aba0f067817e6ceabc3181e.js"></script>

Дальше мы добавим два *TextFormField* для нашего email and password. TextFormField позволит нам добавить ```validator``` и ```onSaved```. Эти методы обратного вызова будут инициированы, когда ```form.validate()``` и ```form.save()``` (TextFormField находятся внутри Form). Итак, для примера если метод ```form.save()``` вызван, значение из текстового поля копируется в другую локальную переменную.
Для ```validator```, мы будем проверять на пустые поля при отправке м обновлять ```_errorMessage``` для отображения пользователю. We also need to create variables Мы так же должны создать переменные ```_email``` и ```_password``` хронящие значения, которые будут использованы для авторизации в дальнейшем
Для password, мы установим ```obsecureText: true``` для скрытия пользовательского пароля.
Для фиксации высоты текстового поля ```maxLines``` устанавливается значение 1.
```autofocus``` установим ```false``` чтобы предотвратить фокусировку поля при загрузке страницы.
Оба значения email и password  ```trim()``` очистим от пробелов.

<script src="https://gist.github.com/tattwei46/98a6a7411263966ae939d15e95b2e483.js"></script>

Далее создадим две кнопки. Первая для пользователей выполняющих действия вход/регистрация и вторая для смены режима формы(между входом и авторизацией). Для отслеживания режима формы мы используем boolean ```_isLoginForm```
При обнаружении события нажатия основной кнопки инициируется метод ```_ validateAndSubmit```, который проверяет ввод *TextFormField* и выполняет вход/регистрацию.

<script src="https://gist.github.com/tattwei46/87856f547b4708b46fc290d67bb11744.js"></script>

Дальше мы сделаем вторую кнопку для выбора между входом и регистрацией. Мы используем ```FlatButton``` вместо ```RaisedButton``` в первой кнопке. Резоннее использовать 2 кнопки, нежели одну особенную (общую) ```RaisedButton``` это правильный выбор так как она привлекает больше внимание чем ```FlatButton```.

<script src="https://gist.github.com/tattwei46/58eb89108e73923037f058f3fb58ba5a.js"></script>

Для переключения между входом и авторизацией выполняется определение состояния bool ```_isLoginMode```. Так же у нас есть ```resetForm ```метод очищающий введёные данные *TextFormField (эта информация хранится в GlobalKey  в FormState об этом я объяснено попозже).*

<script src="https://gist.github.com/tattwei46/d39a7135b80a50913bd70ab48dc9b3d1.js"></script>

Дальше, мы создадим пользовательский интерфейс выводящий ошибку для пользователей. These messages could be error thrown by Firebase or invalid form input. Эти сообщения могут выдаваться Firebase или при недопустимых значениях в форме ввода. Если появляется новое сообщение об ошибке, меняется состояние ```setstate``` с новым значением ```_errorMessage```. 

<script src="https://gist.github.com/tattwei46/5747f69581a87278c22f7c556de59174.js"></script>

Наконец то вернёмся к нашей Form, нам нужно создать GlobalKey для отлавливания состояния нашей формы. Это позволяет отслеживать введенный пользователем адрес электронной почты и пароль.

Добавьте следующее в класс ```_LoginSignupPageState``` 

```final _formKey = new GlobalKey<FormState>();```

И свяжите их под _showForm(), ключевые свойства

```Widget _showForm() {
  return new Container(
      padding: EdgeInsets.all(16.0),
      child: new Form(
        key: _formKey,
        child: new ListView(
          shrinkWrap: true,
          children: <Widget>[
            showLogo(),
            showEmailInput(),
            showPasswordInput(),
            showPrimaryButton(),
            showSecondaryButton(),
            showErrorMessage(),
          ],
        ),
      ));
}```

Давайте расположим эти отдельные компоненты пользовательского интерфейса и вызовим их в наш *ListView*.

<script src="https://gist.github.com/tattwei46/399aaee36ae6d91d705d19a30d7aa192.js"></script>

Теперь попытаемся запустить наш проект используя команду ```flutter run```

![Flutter login demo](https://iswift.ru/images/1_dgisA_6Dmtsdhz_GbmFrBg.png "Flutter login demo")
<p align="center">TextFormField валидатор действий</p>

### 👉Шаг 6: Регистрируем проект в Firebase
Перейдите https://console.firebase.google.com и зарегистрируйте новый проект.
Для android, нажмите на иконку android. Ввведите Ваш package name, который вы можете найти ```android/app/src/main/AndroidManifest.xml```
Скачайте конфигурационный файл ```google-services.json``` (Android).
Перетащите ```google-services.json``` в папку приложения в представлении проекта.

![google-services.json](https://iswift.ru/images/1_Jom5-2bcarPMsrIEbMK3mw.png "google-services.json")

Нам нужно добавить Google Services Gradle плагин для чтения google-services.json. В строке ```/android/app/build.grade``` добавьте следующую строку к последней строке файла.

```apply plugin: 'com.google.gms.google-services'```

```In android/build.gradle```, внутри buildscript добавьте новую зависимость.
```buildscript {
   repositories {
      //...
}
dependencies {
   //...
   classpath 'com.google.gms:google-services:3.2.1'
}```
Для iOS, откройте ```ios/Runner.xcworkspace``` запустите Xcode. Имя пакета можно найти в пакете идентификации Runner.
Скачайте конфигурационный файл ```GoogleService-info.plist``` (iOS).
Переместите ```GoogleService-info.plist``` в Runner подпапку внутри Runner как показано ниже.

### 👉Шаг 7: Добавьте зависимости в pubspec.yaml
Дальше мы должны добавить *firebase_auth* и *firebase_database* зависимости в pubspec.yaml. Что бы получить номер последней версии, перейдите по ссылке https://pub.dartlang.org/ и найдите необходимую зависимость.
```firebase_auth: ^0.14.0+5
firebase_database: ^3.0.7```

### 👉Шаг 8: Импорт Firebase Auth

```import 'package:firebase_auth/firebase_auth.dart';```

### 👉Шаг 9: Выбирите авторизацию используя email and password в Firebase

![Sign up](https://iswift.ru/images/1_UCo6SN3eK-Rn30YMSSWaVA.png "sign up")

### 👉Шаг 10: Создайте абстрактный класс BaseAuth

Создайте новый файл с именем authentication.dart. Мы собираемся создать абстрактный класс BaseAuth. Класс содержит только сигнатуру методов и это обеспечивает минимальные изменения, если мы решили перейти от Firebase к чему-то другому.

<script src="https://gist.github.com/tattwei46/35f58c7faebce33c56bddedb1d99cf93.js"></script>

### 👉Шаг 11: Создание класса Auth, реализующего BaseAuth
Здесь мы определяем, что делают методы абстрактного класса.

<script src="https://gist.github.com/tattwei46/90e0006da3043a7322f351545879085c.js"></script>

### 👉Шаг 12: Создайте HomePage
После успешного входа пользователя он будет направлен на главную страницу.

<script src="https://gist.github.com/tattwei46/9a4892eb2a344ad1e0f556009b4942ca.js"></script>

### 👉Шаг 13: Создайте RootPage
Итак, RootPage является stateful страницей и фактически определяет, показывать ли пользователю LoginSignupPage или HomePage, на основе их состояния аутентификации.
Таким образом, мы отслеживаем состояние аутентификации с помощью:
```enum AuthStatus {
  NOT_DETERMINED,
  NOT_LOGGED_IN,
  LOGGED_IN,
}```
Когда RootPage загрузится, мы пробуем получить userid и установить ```AuthStatus```
<script src="https://gist.github.com/tattwei46/346239c19e9bd8ec24caa4bc440ccea9.js"></script>

У нас есть две возвращающие функции Вход и Выход.  Когда пользователь находится на LoginSignupPage и успешно входит в систему, он инициирует обратный вызов входа в систему на RootPage, которая устанавливает для ```AuthStatus``` значение ```LOGINED _ IN``` и затем показывает пользователю HomePage.
То же самое происходит когда пользователь удачно выходит из системы со страницы HomePage.

<script src="https://gist.github.com/tattwei46/ee5bc1e09a7864a65110ad76c9dd36ad.js"></script>

Вот часть, показывающая пользователю правильную страницу в соответствии с его состоянием AuthStatus. More explaination on auth in the subsequent steps. Дополнительная информация об авторизации будет на последующих шагах.

<script src="https://gist.github.com/tattwei46/743be7c679296c04db1b57d22ae53e99.js"></script>

### 👉Шаг 14: Инициализация Auth в main
На странице main.dart когда мы вызываем RootPage, мы инициализируем новый метод ```Auth()``` и переходим в RootPage как показано на картинке.
 

<script src="https://gist.github.com/tattwei46/df8645225b187220f372f94fff96519d.js"></script>

In root_page.dart, we receive the initialized auth as follows:
<script src="https://gist.github.com/tattwei46/72c47af73bd30767e227e9a01f944a6e.js"></script>

### 👉Шаг 15: Соединим LoginSignupPage и RootPage
 В классе RootPage мы вызовим функцию LoginSignupPage и проходим авторизацию, которая была передана ранее из main.dart а так же соединим наши функции обратнго вызова. Мы используем widget.auth вместо авторизации, потому что эта переменная была передана в класс RootPage из класса MyApp вместо инициализации в RootPage.

Ниже приведён снимок кода класса RootPage который вызывает LoginSignupPage

<script src="https://gist.github.com/tattwei46/14260d7c0f430314758bb6318841523c.js"></script>

Ниже приведён снимок кода метода LoginSignupPage который принимает авторизацию и loginCallback
<script src="https://gist.github.com/tattwei46/bea89f750c620f6246d72f7c228fff7b.js"></script>

### 👉Шаг 16: Используем Auth внутри LoginSignupPage

Для входа пользователя в систему, мы используем ```widget.auth.signIn```, который был реализован согласно абстрактному классу BaseAuth. Базовый метод использует Firebase ```signInWithEmailAndPassword``` который возвращает будущие значения. Будущее является частью асинхронной операции, которая не блокирует основной поток. Будущий класс связан с ключевыми славами такие как **async** и **await**. Следовательно, метод должен быть ожидаемым, а внешняя функция обертки должна иметь асинхронный режим. Мы включаем методы авторизации и регистрации с возможностью поймать блокировку. Если выпадает ошибка, наш улавливатель ошибок способен захватить сообщение ошибки и вывести на монитор пользователю.

<script src="https://gist.github.com/tattwei46/28d2e93b434fcf02553aba3417036fd2.js"></script>

### 👉Шаг 17: Попытка регистрации пользователя
Попытаемся зарегистрировать пользователя вводя email and password.

>Если Вы встречали ошибку, как указано ниже, это потому что имеется лишний пробел в конце email. Поэтому я хочу добавить очистку (trim) email. 

```I/flutter (14294): Error PlatformException(exception, The email address is badly formatted., null)```

>Если вы встретили ошибку, как указано, ниже, необходимо сделать пароль длинной не меньше 6 символов.

```I/flutter (14294): Error PlatformException(exception, The given password is invalid. [ Password should be at least 6 characters ], null)```
Наконец, после успеха, вы можете увидеть в терминале следующую строчку. Набор символов это идентификатор пользователя (id)


```I/flutter (14294): Signed up JSwpKsCFxPZHEqeuIO4axCsmWuP2```

Аналогично, если мы пытаемся Войти тем же самым пользователем, которого мы зарегистрировали, мы должны получить что-то подобное:

```I/flutter (14294): Signed in JSwpKsCFxPZHEqeuIO4axCsmWuP2```

![Debug banner removed](https://iswift.ru/images/1_hfrGL0FlyfbmVCrZPBKFmw.png "Удаляем банер отладки")
<p align="center">Удаляем банер отладки</p>

**Профессиональный совет:** Уведомление о режиме отладки в правом верхнем углу приложения, вы можете легко удалить добавив следующую строку внутри MaterialApp виджета на странице main.dart
```debugShowCheckedModeBanner: false,```

🎊🎉Ура. Победа!🎉🎊

![Success](https://iswift.ru/images/1_lEkb1HvKFvsCmSseNFZLhA.gif "Success")
