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

When you create a new card, you must specify a customer or recipient on which to create it.
**токен** : на картинке выше , **клиент** : вы индентифицировали покупателя (мы используем Firebase Auth by Google)

**Stripe Call 2 :**

> stripe.charges.create(charge, { idempotency_key: idempotentKey });

![Flutter + Stripe + Firebase— Buy](https://iswift.ru/images/1_lhbywA30vmts6PcCgmiMjQ.png "Flutter + Stripe + Firebase— Buy")

To charge a credit card or other payment source, you create a ```Charge``` object.

**charge** = { amount, currency, customer, description };

**If your API key is in test mode, the supplied payment source (e.g., card) won’t actually be charged**

> The API supports idempotency for safely retrying requests without accidentally performing the same operation twice. For example, if a > request to create a charge fails due to a network connection error, you can retry the request with the same idempotency key to 
> guarantee that only a single charge is created.

For more info on idempotency refer [link](https://stripe.com/docs/api/idempotent_requests?lang=node).

**Stripe Call 3 :**

> stripe.charges.list({// limit: 3,customer: cust_id});

![Flutter + Stripe + Firebase— All charges..](https://iswift.ru/images/1_nxF4tIz6GAssV5227sKdQg.png "Flutter + Stripe + Firebase— All charges..")

Returns a list of charges you’ve previously created. The charges are returned in sorted order, with the most recent charges appearing first.

>where cust_id is the same as the one in Stripe call 1…..

**Stripe Call 4 :**

> stripe.refunds.create({ charge: chargeId });

![Charge ID…](https://iswift.ru/images/1_E6lTz3d0zKSBWHtr4OL35Q.png)

When you create a new refund, you must specify a charge on which to create it. The charge ID is the id which we received for transactions..
## Finally…
We did adding card, buying from the card, listing all our payments and refunded some transactions….**Phew…**

Source code :
https://github.com/AseemWangoo/flutter_programs/blob/master/Stripe_and_Flutter.zip
