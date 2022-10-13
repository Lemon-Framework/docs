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

Until now we had all the logic of the routes implemented inside route callbacks. This works, but in longer term is better to use controllers.

Lets say we have route login with get and post method:

```php
Route::get('/login', fn() => template('login'));

Route::post('/login', function(Request $request) {
    // login logic
});
```

But this can get messy, so lets introduce controller.

Controllers live in similar folder as middlewares: `src/Controllers`.

Our controller will then look like this:

```php
<?php

namespace App\Controllers;

use Lemon\Http\Request;

class Login
{
    public function get()
    {
        return template('login');
    }

    public function post(Request $request)
    {
        // login logic
    }
}
```

Now we have to register it, we can to it simply like this:

```php
Route::get('/login', [Login::class, 'get']);

Route::post('/login', [Login::class, 'post']);
```

There is also method called `Route::controller()` which takes base url and controller class and creates routes depending on methods:

```php
Route::controller('/login', Login::class);
```

This does the same thing. This method registers action by its request method.

You can also use so-called resources which are compatible with this shortcut. This is concept from Laravel which is basicaly standart of naming routes and methods:

// TODO table from laravel

`Route::controller()` returns collection of routes, which has some powerfull improvements. But what are collections?

## Collections

Collection is basicaly set of routes, but also collections. Every collection has rules that apply to every route or collection inside.

Let's create collection. We can do this by `Route::collection()` method. This method takes function which defines all the routes:

```php
Route::collection(function() {
    Route::get('/', fn() => 'foo');
});
```
This method returns collection which can be modified.

### Adding middlewares

To apply middleware to every route (and routes in inner collections) in collection you can use `->middleware()` method. Also, middlewares are not actualy set, it assigns it to route when needed, which means better performance.

```php
Route::collection(function() {
    // Found route here have middleware Foo
    Route::get('/foo', fn() => 'foo');
})->middleware(Foo::class);
```


### Excluding middlewares

If we have nested collections, we can exclude middlewares. This is handy if e.g we load file with middleware but we don't want to use it in some routes, we can exclude it:

```php
Route::collection(function() {
    Route::collection(function() {
        // Found route here don't have middleware Foo
        Route::get('/bar', fn() => 'bar');
    })->exclude(Foo::class);

    // Found route here have middleware Foo
    Route::get('/foo', fn() => 'foo');
})->middleware(Foo::class);
```

### Prefixes

Prefixes are strings that are before every route in collection. The main reason to use it is performance. When lemon searches for route, it checks prefix and if request url doesnt match given prefix, the collection will be skipped.


```php
Route::collection(function() {
    // When we send request /foo all of theese routes will be skipped 
    Route::get('/', fn() => 'foo');
})->prefix('api');
```

