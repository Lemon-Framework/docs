# Session

Lemon has basic session managing class. To use it, you have to require namespace: `use Lemon\Sessions\Session;`.

## Session class structure

- `::setName(name)` Sets session name
- `::start()` Starts session

## Example:

```php
<?php

//...

use Lemon\Sessions\Session;

Session::setName("LEMON_LAND_SESSION");
Session::start();

Route::get("/", function(){
    $_SESSION["id"] = uniqid();
    return "Hello! 👋";
});

// ...
```


# CSRF

Lemon brings class `Csrf` for preventing [CSRF attack](https://en.wikipedia.org/wiki/Cross-site_request_forgery). To use it require namespace: `use Lemon\Sessions\Csrf;`.

## Csrf class structure

- `::setToken()` Saves new token to session ❗ `Session::start()` is required for this method!
- `::check()` Compares token from session with the one from request data. If they doesn't match, it will throw 400.
- `::getToken()` Returns token from session

Every form with method **POST** must contain `@csrf`

## Example:

```php
<?php
// public/index.php

//...

use Lemon\Sessions\Session;
use Lemon\Sessions\Csrf;
use Lemon\Http\Request;

Session::setName("LEMON_LAND_SESSION");
Session::start();
Csrf::setToken(); // Setting token to user

Route::get("/", function(){
    return view("welcome");
});

Route::post("/", function(Request $request){
    Csrf::check(); // Checking if sent token matches session token
    if ($request->password == "fidojesus")
        return redirect("https://www.youtube.com/watch?v=dQw4w9WgXcQ");
    return redirect("/");
});

// ...
```

```jinja
<!-- views/welcome.lemon.php -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Super secret page</title>
</head>
<body>
    <form method="POST">
        @csrf <!-- Because we're looking dor CSRF token in the callback we have to put this mapping -->
        <input type="text" name="password">
    </form>
</body>
</html>
```
