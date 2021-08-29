# Routing

Lemon comes with Laravel-like basic routing system. You can start with routing in your `public/index.php`.

## Define your first route

Route has 3 fundamental parts:
- Path - The uri of your route
- Request methods - The required methods for this route
- Action - Closure function that will be callen.

To define new route use one of following methods:
- `Route::get(path, action)` Defines route for method GET
- `Route::post(path, action)` Defines route for method POST
- `Route::any(path, action)` Defines route for methods GET and POST
- `Route::use(methods[], path, action)` Defines route for methods that you set

> ❗ Every route action must have return statement, which contains response data, such as string, view or even array, but more about that later.
    
After you define all the routes, its time to run it. To run your app use `Route::execute()`

Here is example of basic app:

```php
<?php

// Loading dependencies
require __DIR__ . "/../vendor/autoload.php";

// Creating basic route 
Route::get("/", function(){
    return "Hello there!";
});

// Run!
Route::execute();

```

## Lemon Lifecycle explained

Every project has file `index.php` in folder `public`. This file is main point for all requests - Every request is directed to this file.

> Reason why its inside of folder public is to prevent sending request to other files such as .env, and other php files, for now its not necessary, but it is safer.

For example you send request get to "http://example.com/hello", File hello obviously doesn't exist, but thanks to our server settings, it won't throw 404, because its directed to file `public/index.php`. In this file we created simple app. Something like
```php
<?php

require __DIR__ . "/../vendor/autoload.php";

Route::get("/", function(){
    return "Welcome to my fantastic page! ✨";
});

Route::get("/hello", function(){
    return "Hello! 👋";
});

Route::execute();

```

Since we've registered routes `/` and `/hello`, we have saved route action inside special array inside Routing Kernel. Right now if we start our app, it will try to find route that matches request uri (in this case `/hello`). If there is route with matching uri, it will also check request method. For instance, it i create endpoint using method `::get()` it won't accept other methods, but if it matches the request method, it will call given action.

> ❓ But what if i want to do something on the same path but with different request method?

Yes. It is possible, you can have more than 1 route on same path. Here is an example:

```php
<?php

// Loading dependencies
require __DIR__ . "/../vendor/autoload.php";

// Creating endpoint / for method GET
Route::get("/", function(){
    return "You've just sent a GET request!";
});

// Creating endpoint / for method POST
Route::post("/", function(){
    return "You've just sent a POST request!"
});

// Run! 
Route::execute();

```

On this example you can see 1 route, but with different response depending on request method.

This is equal way to do it:

```php
<?php

// Loading dependencies
require __DIR__ . "/../vendor/autoload.php";

/*
 | Since ::any method accepts only GET and post, this is way to create the above example.
 | This way works, but the first one is cleaner 
 */
Route::any("/", function(){
    if ($_SERVER["REQUEST_METHOD"] == "GET")
        return "You've just sent a GET request!";
    else
        return "You've just sent a POST request!";
});


Route::execute();

```

> ❓ What if I sent wrong route name, or used wrong method?

If there is no matching route, it will return status code 404. If dedicated endpoint does not support upcomming request method, it will throw status code 400. More about status codes later.

## Dynamic Routes

You can also create dynamic routes. Such as

```php
<?php
// ...

Route::get("/hello/{name}", function($name){
    return "Hello $name! 👋";
});

// ...

```

On this example you can see route `/hello`. This route will match any uri that starts with hello and has any string after that.

So if we send request get to `/hello/Majkel` it will give us output `Hello Majkel! 👋`

If we want to use dynamic content, we need to add arguments to our callback.

We can obviously build more complex apps like

```php
<?php
// ...

$data = [
    "CoolFido" => ["id" => 1, "editor" => "VSC"]
    "Majkel" => ["id" => 2, "editor" => "Neovim"]
]

// This Route has multiple dynamic statements
Route::get("/people/{name}/{info}", function($name, $info){
    global $data;
    if (!array_key_exists($name, $data))
        return "Person $name not found 😢";

    if ($info == "id")
        return "Id of $name is" . $data[$name][$info];

    if ($info == "editor")
        return "Person $name uses" . $data[$name][$info];

    return "This information is not stored. 😢";
});

// ...

```

Now you should understand basics of Lemon routing. More about routing and Lifecycle will be covered later.

