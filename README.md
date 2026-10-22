# iDev Easy Admin

Admin package with easy CRUD, data import, and data export features.

---

## Installation

### 1. Add Package to `composer.json`

Before running the require command, configure your main project to find this custom repository by adding a `repositories` block, then run the require command:

```json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/farchanw/easyadmin"
        }
    ]
}

```

```bash
composer require idevsmg/easyadmin:dev-main

```

### 2. Add Autoload

Update the `autoload` section in your `composer.json`:

```json
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "Idev\\EasyAdmin\\": "vendor/idevsmg/easyadmin/src/"
    }
}

```

### 3. Register Service Provider

Depending on your Laravel version, register the service provider in one of the following locations:

* **Laravel 10 and below (`config/app.php`)**:
```php
'providers' => ServiceProvider::defaultProviders()->merge([
    // ...
    Idev\EasyAdmin\EasyAdminServiceProvider::class,
])->toArray(),

```


* **Laravel 11 (`bootstrap/providers.php`)**:
```php
return [
    // ...
    Idev\EasyAdmin\EasyAdminServiceProvider::class,
];

```



### 4. Run Installation Commands

Run the autoload dump and publish the package assets:

```bash
composer dump-autoload
php artisan vendor:publish --tag=install-idev --force

```

After running these commands, proceed with your database migrations and seeders.

---

## CRUD Generator

You can easily generate your CRUD controllers.

### Register the Command

* **Laravel 10 and below (`app/Console/Kernel.php`)**:
```php
protected $commands = [
    // ...
    \Idev\EasyAdmin\app\Console\Commands\ControllerMaker::class,
];

```


* **Laravel 11 (`bootstrap/app.php`)**:
```php
->withCommands([
    Idev\EasyAdmin\app\Console\Commands\ControllerMaker::class,
])

```



### Run the Generator

Make sure your table migration has been created and migrated. Then run the artisan command:

```bash
php artisan idev:controller-maker --slug=your-route --table=your_table

```

**Example:**
If you have a `master_products` table and want to access the page at `/master-product`:

```bash
php artisan idev:controller-maker --slug=master-product --table=master_products

```

---

## Middleware CRUD (Optional)

An optional middleware is available for access control in your CRUD features.

### 1. Register the Middleware

Add the middleware alias to your kernel/application setup:

* **Laravel 10 and below (`app/Console/Kernel.php`)**:
```php
protected $middlewareAliases = [
    // ...
    'middlewareByAccess' => \Idev\EasyAdmin\app\Http\Middleware\MiddlewareByAccess::class
];

```

* **Laravel 11 (`bootstrap/app.php`)**:
```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->alias([
        'middlewareByAccess' => \Idev\EasyAdmin\app\Http\Middleware\MiddlewareByAccess::class,
    ]);
})
```

### 2. Apply to Routes

Implement the middleware in your route definition:

```php
Route::group(['middleware' => ['web', 'auth', 'middlewareByAccess']], function () {
    // ...
});

```

---

## Sample CRUD (Optional)

If you want to view a sample CRUD controller implementation, publish the sample files:

```bash
php artisan vendor:publish --tag=sample-crud --force

```

Then configure your routes like this:

```php
Route::group(['middleware' => ['web', 'auth']], function () {
    Route::resource('sample-data', SampleDataController::class);
    Route::get('sample-data-api', [SampleDataController::class, 'indexApi'])->name('sample-data.listapi');
    Route::get('sample-data-export-pdf-default', [SampleDataController::class, 'exportPdf'])->name('sample-data.export-pdf-default');
    Route::get('sample-data-export-excel-default', [SampleDataController::class, 'exportExcel'])->name('sample-data.export-excel-default');
    Route::post('sample-data-import-excel-default', [SampleDataController::class, 'importExcel'])->name('sample-data.import-excel-default');
});

```

---
