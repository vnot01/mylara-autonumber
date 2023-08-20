# Laravel AutoNumber

[![Latest Stable Version](https://poser.pugx.org/vnot01/mylara-autonumber/v/stable)](https://packagist.org/packages/vnot01/mylara-autonumber)
[![Total Downloads](https://poser.pugx.org/vnot01/mylara-autonumber/downloads)](https://packagist.org/packages/vnot01/mylara-autonumber)
[![StyleCI](https://styleci.io/repos/99206904/shield)](https://styleci.io/repos/680790804)
[![License](https://poser.pugx.org/vnot01/mylara-autonumber/license)](https://packagist.org/packages/vnot01/mylara-autonumber)


Laravel package to create autonumber for Eloquent model

# Installation

You can install the package via composer:

```
composer require vnot01/mylara-autonumber
```

Register the ServiceProvider in `config/app.php`

```php
'providers' => [
    // ...
    MyLara\AutoNumber\AutoNumberServiceProvider::class,
],
```

Publish the default configuration

```
php artisan vendor:publish --provider='MyLara\AutoNumber\AutoNumberServiceProvider'
```

Running migration

```
php artisan migrate
```

# Usage

Your Eloquent models should use the `MyLara\AutoNumber\AutoNumberTrait` trait

The trait contains an abstract method `getAutoNumberOptions()` that you must implement yourself.


```php
use MyLara\AutoNumber\AutoNumberTrait;
    
class Order extends Model
{
    use AutoNumberTrait;
    
    /**
     * Return the autonumber configuration array for this model.
     *
     * @return array
     */
    public function getAutoNumberOptions()
    {
        return [
            'order_number' => [
                'format' => 'SO.?', // autonumber format. '?' will be replaced with the generated number.
                'length' => 5 // The number of digits in an autonumber
            ]
        ];
    }

}
```

You can also pass a `closure` for the format value.

```php
public function getAutoNumberOptions()
{
    return [
        'order_number' => [
            'format' => function () {
                return 'SO/' . date('Ymd') . '/?'; // autonumber format. '?' will be replaced with the generated number.
            },
            'length' => 5 // The number of digits in the autonumber
        ]
    ];
}
```

## Saving Model

```php
$order = Order::create([
    'customer' => 'Mr. X',
]);
```

The order_number will be automatically generated based on the format given when saving the Order model.

```php
echo $order->order_number;

// SO/20170803/00001
```

## License

Laravel-autonumber is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

## Contributing

Please report any issue you find in the issues page. Pull requests are more than welcome.
