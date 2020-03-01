# Flutter + Stripe + Firebase
*“Money” — real honey….*

[![Youtube Flutter + Stripe + Firebase](https://iswift.ru/images/2020-03-01_20-29-23.png)](https://youtu.be/Ax4f0YPQpJ4)
>In this festive season, people often spend (be online / shopping etc). This gave me an idea, why not to try payments using Flutter.

![Detail screen](https://iswift.ru/images/1_fjQfWoG-5iaFOcdg5F10Fw.png "Detail screen")

## Begin…
This article uses the [Stripe SDK](https://stripe.com/sg) and [Cloud Firestore](https://firebase.google.com/docs/firestore/quickstart) for storing the necessary payment information….
For basic information related to Cloud Firestore, [please refer my previous article](http://flatteredwithflutter.com/firebase-firestore-and-flutter/).

## Things needed…
1. Stripe account (obviously free)
2. Once created go to Developer -> Api keys -> Reveal test key
You need to set this key using the Firebase command…
> firebase functions:config:set stripe.token=”Your token here"
This token, you’ll get from Step 2…
3. Add Stripe to the node modules, using
> npm install stripe
If necessary, add stripe in package.json
4. For the list of apis provided by Stripe, refer the link.
5. From flutter side, you need the package stripe_payment

```import 'package:stripe_payment/stripe_payment.dart';
StripeSource.setPublishableKey("pk_test");
```

**The publishable key is the Publishable key from the API keys (Stripe)….**
![API keys Stripe](https://iswift.ru/images/1_ujzs7Q_h-RJ3LZc7CcNsNA.png "API keys Stripe")

In the cloud function side, you need only this :
> const stripe = require(‘stripe’)(functions.config().stripe.token);

**Next…**
For starting communication with Stripe, **you need to have a token…**(which Stripe recommends to generate from the Client side using its libraries)…
In our case, flutter package handles it internally (**when we add card**)…

![ADD a card](https://iswift.ru/images/1_9qSmfi5TyqN5VPhjTXr4yQ.png "Add a card")

```
StripeSource.addSource().then((String token) {
    print(token); //your stripe card source token
});
```
Here, you receive the token, which you need to pass / store for successive transactions with Stripe…

**Stripe Call 1 :**

> stripe.customers.createSource(customer, { source: token });

When you create a new card, you must specify a customer or recipient on which to create it.
where token : from above image , customer : your authenticated customer (we are using Firebase Auth by Google)

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

![your payment](https://stripe.com/docs/api/idempotent_requests?lang=node)
