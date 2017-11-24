# auto-swagger-ui
Give your project some swagger docs in a matter of seconds. Simply just install the package and service providers,
optionally adding the configuration file, then BAM you now have a swagger ui endpoint. 

Also integrates with swagger php annotations, allowing you scan your annotations and generate a json object at 
a specified endpoint. 1, 2, 3, too much swag. ;)

## Install
```bash
composer require kevupton/auto-swagger-ui
```

## Setup

### Add to the Service Providers:
#### Laravel:
In `config/app.php` add `\Kevupton\AutoSwaggerUI\Providers\AutoSwaggerUIServiceProvider::class,` to `providers`.

#### Lumen:
In `bootstrap/app.php` add `$app->register(\Kevupton\AutoSwaggerUI\Providers\AutoSwaggerUIServiceProvider::class);`


## Config

public the configuration file:

```bash
php artisan vendor:publish
```
Then the config pieces can be edited how you please.
```php
<?php

return array(
    // whether or not the swagger ui is enabled
    'ui_enabled' => env('SWAGGER_UI_ENABLED', true),

    // the path to the swagger ui implementation. By default will use its own swagger ui
    'path' => env('SWAGGER_UI_PATH'),

    'urls' => [
        // where the swagger ui will be located
        'ui' => env('SWAGGER_UI_URL', '/api/swagger'),

        // where the json will be located
        'json' => env('SWAGGER_JSON_URL', '/api/swagger.json')
    ],

    // whether or not to enable the swagger scanner
    'scanner_enabled' => env('SWAGGER_SCAN_ENABLED', true),

    'scanner' => [
        // if you want to enable swagger scan then specify an endpoint
        'output_url' => env('SWAGGER_SCANNER_OUTPUT_URL', '/api/swagger.json'),

        // the directory to scan
        'paths' => env('SWAGGER_SCANNER_PATH', '/app/Http/Controllers'),

        // the default scanner which passes the json
        'handler' => env('SWAGGER_SCANNER_HANDLER', '\Kevupton\LaravelSwagger\scan'),

        // the options that is passed into the scanner
        'options' => [

            // the models to include in the scan
            'models' => []
        ],

        // how long to save the scanned json for
        'cache_duration' => env('SWAGGER_SCANNER_CACHE_DURATION', null),

        // headers in the JSON response (for CORS)
        'headers' => [
            'Access-Control-Allow-Origin: *',
            'Access-Control-Allow-Methods: GET, POST, OPTIONS, PUT, DELETE',
            'Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Authorization'
        ]
    ]

);
```

If you do not wish to have the scanning functionality, simply comment out the scanner. By default it will already 
be commented out.

*Note: Lumen will probably need to copy the config from the config file in this package, to `swagger.php` and 
register the configuration `$app->configure('swagger');` in the `bootstrap/app.js`.*
