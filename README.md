Polr API
========

A Restful API for the Polr URL Shortener

Features
--------

This package gives access to all Polr features through a Restful API, excepted the following.

- User creation and deletion.
- Password change.
- Link redirection.

Installation
------------

Add the Polr API package to the `composer.json` file of your Polr installation, and run `composer update`.

```json
{
    "require": {
        "lagdo/polr-api": "~0.1"
    }
}
```

Register the service provider in the `bootstrap/app.php`.

```php
$app->register(\Lagdo\Polr\Api\PolrApiServiceProvider::class);
```

Documentation
-------------

The API endpoints are [documented here](docs/api.md). The documentation is generated with [API Doc](http://apidocjs.com/).

Known issues
------------

Sometimes the requests to the API return with errors due to CSRF verification failure.
That's because Polr has disabled CSRF verification only for its own API endpoints.

This API endpoints then need to be also added in the `app/Http/Middleware\VerifyCsrfToken.php` file.

```php
    public function handle($request, \Closure $next) {
        if ($request->is('api/v*') &&
            !$request->is('api/v*/admin/*') &&
            !$request->is('api/v*/link_avail_check')) {
            // Exclude public API from CSRF protection
            // but do not exclude private API endpoints
            return $next($request);
        }

        return parent::handle($request, $next);
    }
```