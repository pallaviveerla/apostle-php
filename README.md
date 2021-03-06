# Apostle PHP

[![Build Status](https://travis-ci.org/apostle/apostle-php.png?branch=master)](https://travis-ci.org/apostle/apostle-php)
[![Latest Stable Version](https://poser.pugx.org/apostle/apostle-php/v/stable.png)](https://packagist.org/packages/apostle/apostle-php)

PHP bindings for [Apostle.io](http://apostle.io)

## Installation

### With Composer

Add `apostle/apostle-php` to `composer.json`.

```json
{
    "require": {
        "apostle/apostle-php": "v0.1.4"
    }
}
```

### Without Composer

Download the [latest release](https://github.com/apostle/apostle-php/releases). Ensure `src` is in your autoload path. If you’re not using auto loading, require the following files:

* Apostle.php
* Apostle\Queue.php
* Apostle\Mail.php
* Aposlte\UninitializedException.php

#### Prerequisites

* [Guzzle](http://docs.guzzlephp.org/en/latest/)

## Domain Key

You will need to provide your apostle domain key to send emails.

```php
Apostle::setup("your-domain-key");
```

## Sending Email
Sending a single email is easy, the first param is your template's slug, and the second is an array of data.

```php
use Apostle\Mail;

$mail = new Mail(
	"template-slug",
	array("email" => "mal@apostle.io", "name" => "Mal Curtis")
);

$mail->deliver();
```

You don‘t have to add the data at initialization time, feel free to add it after. You can add in any data your template needs too.

```php
$mail = new Mail("template-slug");
$mail->email = "mal@apostle.io";
$mail->name = "Mal Curtis";
$mail->from = "support@apostle.io";
$mail->replyTo = "doreply@apostle.io";
$mail->website = "apostle.io"; // You can add any data your template needs

$mail->deliver();
```

### Failure

Pass a variable for failure information to the `deliver` method.

```php
$mail = new Apostle\Mail("template-slug");

echo $mail->deliver($failure);
// false

echo $failure;
// No email provided
```

## Sending Multiple Emails

To speed up processing, you can send more than one email at a time.

```php

use Apostle\Mail;
use Apostle\Queue;

$queue = new Queue();

for($i=0;$i<5;$i++){
	$mail = new Mail("template-slug");
	$mail->email = "user" . $i . "@example.org";
	$queue->add($mail);
}

$queue->deliver();
```

### Failures

If any `Mail` object fails validation then no emails will be sent. To retrieve failed objects, you can supply a variable to be populated.

```php

use Apostle\Mail;
use Apostle\Queue;

$queue = new Queue();

$mail = new Mail("template-slug");
$queue->add($mail);


$mail = new Mail(null, ["email" => "user@example.org"]);
$queue->add($mail);


echo $queue->deliver($failures);
// false

echo count($failures);
// 2

echo $failures[0]->deliveryError();
// "No email provided"

echo $failures[1]->deliveryError();
// "No template provided"
```

## Requirements

* PHP 5.3+

## Who
Created with ♥ by [Mal Curtis](http://github.com/snikch) ([@snikchnz](http://twitter.com/snikchnz))


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request





