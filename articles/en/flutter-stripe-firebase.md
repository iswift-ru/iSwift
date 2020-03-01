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
StripeSource.setPublishableKey("pk_test");```
**The publishable key is the Publishable key from the API keys (Stripe)….**
![API keys (Stripe)](https://iswift.ru/images/1_ujzs7Q_h-RJ3LZc7CcNsNA.png)

