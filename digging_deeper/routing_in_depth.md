# Routing in depth

Now, when our index is there just to run our app, we have to move the routes.

To do that, create folder routes (again, outside of public). And in the folder create file that will contain main routes, for example web.php.

This file will contain routes created the same as before. But we have to somehow load this file. So go back to init.php and add

```php
/** @var \Lemon\Routing\Router $router */
$router = $application->get('routing');

$router->file('routes.web')
    ->middleware(Csrf::class)
;

return $application;
```

Its important to add the CsrfMiddleware there. But wait, what is Middleware?


## Middlewares

Middleware is basicaly method that is executed with route action. If the middleware returns response, it will be used instead of the one that is created from the route action.

Middlewares are typicaly located in `src/Middlewares`, so create this folders. Also we have to load `src`, but because this folder will have only classes we can use composer autoloader. So add this to composer.json and type `composer dump-autoload`

```json
    "autoload": {
        "psr-4": {
            "App\\": "src"
        }
    }
```

Now let's create our first middleware, for example middleware that lets only authenticated people

```php

namespace App\Middlewares;

class Auth
{
    public function onlyAuthenticated(Session $session)
    {
        if (!$session->has('name')) {
            return redirect('/login');
        }
    }
}

```

Here we can see middleware that checks if session key exists, if not, it redirects the user to 'login' (returns RedirectResponse), however if the key is set, it returns nothing, which means it will go to another middleware.

We can also inject "prototype" of the response, which means we get response that will be send, this is useful if we want to for example only change headers.

To add middleware to route, simply use `middleware()` method.

```php
// routes/web.php

use \App\Middlewares\Auth;

Route::template('/', 'home')->middleware(Auth::class);
```

## Controllers

## Collections
