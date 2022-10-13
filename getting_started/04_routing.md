# Routing

Since in Lemon we don't follow traditional aproach file=route, there is other way. As said, lemon Application automaticaly found route depending on the reqest, but first we have to define them. For that there is class `Lemon\Route`. So let's define first route. This is simple definition:

```
<?php

use Lemon\Kernel\Application;
use Lemon\Route;

include __DIR__.'/../vendor/autoload.php';

Application::init(__DIR__);

Route::get('/', function() {
   return 'Hello world!';
});

```

In this example, when user sends get to path /, defined function will be executed and its result will be send back to user. Now what happens when user sends request to other path? Since the path is not registered user will receive 404 error page. However if we send request to existing path, but with method that ain't registered, he will get 400. This means we can have multiple callbacks for one path and each will be callen depending on method. 

```
<?php

use Lemon\Kernel\Application;
use Lemon\Route;

include __DIR__.'/../vendor/autoload.php';

Application::init(__DIR__);

Route::get('/', function() {
   return 'Hello world!';
});

Route::post('/', function() {
   return 'Hello post world!';
});

```

At this point, if you sent POST to / and received 400, its ok, its because of CSRF protection which will be covered later.

This can be obviously applied to any method.

If we want to have route that will support all methods we can use `Route::any()`

## Dynamic routing

While defining routes path you can add dynamic parts. 

```
<?php

use Lemon\Kernel\Application;
use Lemon\Route;

include __DIR__.'/../vendor/autoload.php';

Application::init(__DIR__);

Route::get('/users/{user}', function($user) {
   return 'Hello '.$user;
});


```

In this example, if we send request to `/users/Fido/` we will see `Hello Fido`. So dynamic routing is basicaly about creating parts in route that user can fill with everyrhing they want. Content of the path will be passed into to function as argument. We can have multiple parts like this.

Now let's talk about http.
