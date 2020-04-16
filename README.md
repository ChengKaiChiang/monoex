# Monoex

[![Build Status](https://travis-ci.com/MilesChou/monoex.svg?branch=master)](https://travis-ci.com/MilesChou/monoex)

Monolog extensions.

## Use on Laravel

This package implements [Package Discovery](https://laravel.com/docs/7.x/packages#package-discovery), and the following PSR-17 / PSR-18 driver must be register:

* `Psr\Http\Client\ClientInterface`
* `Psr\Http\Message\RequestFactoryInterface`
* `Psr\Http\Message\StreamFactoryInterface`

Example, use `laminas/laminas-diactoros` and `symfony/http-client`:

```php
$app->singleton(RequestFactoryInterface::class, new \Laminas\Diactoros\RequestFactory());
$app->singleton(ResponseFactoryInterface::class, new \Laminas\Diactoros\ResponseFactory());
$app->singleton(StreamFactoryInterface::class, new \Laminas\Diactoros\StreamFactory());

$app->singleton(ClientInterface::class, function($app) {
    return new \Symfony\Component\HttpClient\Psr18Client(
        null,
        $app->make(ResponseFactoryInterface::class),
        $app->make(StreamFactoryInterface::class)
    );
});
```

Finally, the `logging.php` config can use by new driver `psr18slack`:

```php
return [
    'channels' => [
        'stack' => [
            'driver' => 'psr18slack',
            // same as slack driver
            'url' => env('LOG_SLACK_WEBHOOK_URL'),
            'username' => 'Laravel Log',
            'emoji' => ':boom:',
            'level' => 'critical',
        ],
    ],
];
```

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.
