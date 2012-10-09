PHP-StripeOAuth
===============

PHP library which helps perform an [OAuth2](http://oauth.net/2/) flow for [Stripe](https://stripe.com/), which is used when creating and interacting with a [Stripe Application](https://stripe.com/docs/connect).

### Why I made this
While integration the Stripe API (for our new payment flow at [Skillshare](http://www.skillshare.com/?on)), I found one thing specifically lacking.  
That was, a way to initiate an OAuth flow for a Stripe Application, and to access needed data, in PHP.

While it could be done manually by initiating a curl call (as can be seen in [this gist](https://gist.github.com/3507366), which was posted fairly recently), I wanted to make use of an established OAuth library to make sure I wasn't recreating the wheel.

### Dependencies
Both `StripeOAuth.class.php` and `StripeOAuth2Client.class.php` must be loaded.  
`StripeOAuth2Client.class.php` has the class `OAuth2Client` as a dependency.  
For that, please see [quizlet](https://github.com/quizlet)'s [oauth2-php](https://github.com/quizlet/oauth2-php) library.

### Quick Example

Assuming you've got all your files loaded, you should get pretty far with the following:

``` php
<?php

    // redirect to proper application OAuth url
    $oauth = (new StripeOAuth(
        'ca_********************************',
        'sk_*****************************'
    ));
    $url = $oauth->getAuthorizeUri();
    header('Location: ' . ($url));
    exit(0);

    // ...

    // from the callback, after a person has linked their Stripe account with your Stripe application
    $oauth = (new StripeOAuth(
        'ca_********************************',
        'sk_*****************************'
    ));
    $token = $oauth->getAccessToken($_GET['code']);
    $key = $oauth->getPublishableKey($_GET['code']);
    echo 'Access token: ' . ($token) . '<br />Key: ' . ($key);
    exit(0);

```

The `code` parameter is accessed from the `_GET` array, as after a person connects their Stripe account to your Stripe application, this code is passed along in the url. In most conventional applications, this is more-or-less an request token, and we use it to get an access token, and subsequently all the data we want.

### More
Will update this page shortly with a few more examples on some of the more advanced workings of the Stripe OAuth flow.
