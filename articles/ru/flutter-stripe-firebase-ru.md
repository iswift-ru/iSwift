# Flutter + Stripe + Firebase делаем оплату банковской картой
*“Деньги” — это настоящий мёд….*

[![Youtube Flutter + Stripe + Firebase](https://iswift.ru/images/2020-03-01_20-29-23.png)](https://youtu.be/Ax4f0YPQpJ4)
>В этот праздничный период, люди много тратяться (в интернете / или в магазинах). Это натолкнуло меня на мысль, а почему бы не сделать оплату используя Flutter.

![Detail screen](https://iswift.ru/images/1_fjQfWoG-5iaFOcdg5F10Fw.png "Detail screen")

## Приступим…
В этой статье используется [Stripe SDK](https://stripe.com/sg) и [Cloud Firestore](https://firebase.google.com/docs/firestore/quickstart) для хранения необходимой платёжной информации ….
Для основной информации используется Cloud Firestore, [пожалуйста, ознакомьтесь с предыдущей статьёй](http://flatteredwithflutter.com/firebase-firestore-and-flutter/).

## Что нам необходимо…
1. Аккаунт в Stripe (естественно бесплатный)
2. Как только создали переходим Developer -> Api keys -> Reveal test key
Вы должны отправить этот ключ используя Firebase команду
> firebase functions:config:set stripe.token=”Ваш токен здесь"
Этот токен вы должны были получить во 2ом шаге…
3. Добавьте Stripe в список модулей, используя
> npm install stripe
Если необходимо, добавьте stripe в package.json
4. Для получения списка API предоставляющего Stripe, перейдите по [ссылке](https://stripe.com/docs/api/cards/create?lang=node).
5. Flutter необходимо импортировать пакет [stripe_payment](https://pub.dartlang.org/packages/stripe_payment)

```import 'package:stripe_payment/stripe_payment.dart';
StripeSource.setPublishableKey("pk_test");
```

**publishable ключ это Publishable ключ из API ключей (Stripe)….**
![API ключи Stripe](https://iswift.ru/images/1_ujzs7Q_h-RJ3LZc7CcNsNA.png "API ключи Stripe")

На стороне cloud, вам нужно сделать тольк это:
> const stripe = require(‘stripe’)(functions.config().stripe.token);

**Дальше…**
Для начала коммуникации с Stripe, **Вам необходимо иметь токен…**(который Stripe рекомендует генерировать на клиентской стороне используя эти библиотеки)…
В нашем случае, Flutter обрабатывает пакеты внутри (**когда мы добавляем карту**)… 

![ADD a card](https://iswift.ru/images/1_9qSmfi5TyqN5VPhjTXr4yQ.png "Add a card")

```
StripeSource.addSource().then((String token) {
    print(token); //your stripe card source token
});
```
Здесь, вы получаете токен, который вы должны передать / сохранить для следующей транзакции в Stripe…

**Stripe Call 1 :**

> stripe.customers.createSource(customer, { source: token });

When you create a new card, you must specify a customer or recipient on which to create it. Когда вы создаёте новую карту, вы должны определить клиент или получатель
**токен** : на картинке выше , **клиент** : вы индентифицировали покупателя (мы используем Firebase Auth by Google)

**Stripe Call 2 :**

> stripe.charges.create(charge, { idempotency_key: idempotentKey });

![Flutter + Stripe + Firebase— Buy](https://iswift.ru/images/1_lhbywA30vmts6PcCgmiMjQ.png "Flutter + Stripe + Firebase— Buy")

Оплата кредитной картой или другой платёжный способ, вы можете создать с помощью ```Charge``` объекта.

**charge** = { amount, currency, customer, description };

**Если ключ API находится в тестовом режиме, предоставленный источник платежа (например, карта) фактически не будет списано**


> API поддерживают идемпотентность для безопасной обработку задублировавшихся запросов. Например, если
>запрос на создание оплаты не выполнен из-за ошибки сетевого подключения, можно повторить запрос с тем же ключом идемпотентности и
>гарантировано будет создание только одной оплаты.

Больше информации об идемпотентности [ссылка](https://stripe.com/docs/api/idempotent_requests?lang=node).

**Stripe Call 3 :**

> stripe.charges.list({// limit: 3,customer: cust_id});

![Flutter + Stripe + Firebase — Все оплаты..](https://iswift.ru/images/1_nxF4tIz6GAssV5227sKdQg.png "Flutter + Stripe + Firebase — Все оплаты..")

Вернёмся к списку способов оплаты которые мы уже создали. Оплаты возвращаются в отсортированном порядке, при этом сначала появляются самые последние оплаты.

>где cust_id такой же как и в Stripe call 1…..

**Stripe Call 4 :**

> stripe.refunds.create({ charge: chargeId });

![Charge ID…](https://iswift.ru/images/1_E6lTz3d0zKSBWHtr4OL35Q.png)

При создании нового возврата необходимо указать сумму, на которую оно будет создано. Идентификатор платежа - это идентификатор, полученный для транзакций.
## Финал…
Мы добавили карту, оплатили с карты, перечислили способы оплаты и осуществили возврат денежных средств по транзакции….**Phew…**


Исходный код:
https://github.com/AseemWangoo/flutter_programs/blob/master/Stripe_and_Flutter.zip
