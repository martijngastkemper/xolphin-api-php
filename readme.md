# Xolphin API wrapper for PHP

## Library installation

Library can be installed via [Composer](https://getcomposer.org/)

```
composer require xolphin/xolphin-api-php
```

And updated via

```
composer update xolphin/xolphin-api-php
```

## Usage

### Client initialization

```php
<?php

require 'vendor/autoload.php';

$client = new Xolphin\Client('<username>', '<password>');
```

### Requests

#### Getting list of requests

```php
$requests = $client->request()->all();
foreach($requests as $request) {
    echo $request->id . "\n";
}
```

### Getting request by ID

```php
$request = $client->request()->get(1234);
echo $request->id;
```

### Request certificate

```php
$products = $client->support()->products();

// request Comodo EssentialSSL certificate for 1 year
$request = $client->request()->create($products[1]->id, 1, '<csr_string>', 'EMAIL')
    ->setAddress("Address")
    ->setApproverFirstName("FirstName")
    ->setApproverLastName("LastName")
    ->setApproverPhone("+12345678901")
    ->setZipcode("123456")
    ->setCity("City")
    ->setCompany("Company")
    ->setApproverEmail('email@domain.com')
    ->addSubjectAlternativeNames('test1.domain.com')
    ->addSubjectAlternativeNames('test2.domain.com')
    ->addSubjectAlternativeNames('test3.domain.com')
    ->addDcv(new \Xolphin\Requests\RequestDCV('test1.domain.com', 'EMAIL', 'email1@domain.com'))
    ->addDcv(new \Xolphin\Requests\RequestDCV('test2.domain.com', 'EMAIL', 'email2@domain.com'));

$client->request()->send($request);
```

### Certificate

#### Certificates list and expirations

```php
$certificates = $client->certificate()->all();
foreach($certificates as $certificate) {
    echo $certificate->id . ' - ' . $certificate->isExpired() . "\n";
}
```

#### Download certificate

```php
$certificates = $client->certificate()->all();
$cert = $client->certificate()->download($certificates[0]->id);
file_put_contents('cert.crt', $cert);
```

### Support

#### Products list

```php
$products = $client->support()->products();
foreach($products as $product) {
    echo $product->id . "\n";
}
```

#### Decode CSR

```php
$csr = $client->support()->decodeCSR('<your csr string>');
echo $csr->type;
```
