# Request

Previously, we've covered routing, but in some routes you need to get request parameters. For this lemon comes with pre-defined class called Request. To obtain instance of this class set first argument of your route callback to Request. Like this:

```php
<?php
// ...
use Lemon\Http\Request;

// This is example of basic api endpoint for creating users
Route::post("/api/users/create", function(Request $request) {
    if ($request->token != "Very secret token trust me")
        return "Wrong token";
    
    file_put_contents("../users.txt", $request->name); // Data file is in separate folder so anyone can access it
    return "Created user " . $request->name;
});

// ...

```

Right now we can send request post to the `/api/users/create` with json `{"token":"Very secret token trust me", "name":"Mia"}`


## Request class structure

### Data

- ->headers All request headers
- ->method Request method
- ->json Returns POST json data as array
- ->input Returns POST input data as array
- ->query Returns GET query data as input
- ->data Returns all data
- ->{key} Returns value by named key from all sent data (Excluding headers)

### Methods

- ->json(key) Returns value by named key from sent json
- ->input(key) Returns value by named key from sent form input
- ->query(key) Returns value by named key from sent GET query
- ->header(name) Returns header value

You can obviously combine dynamic routes with Request, but Request must be the first argument!

✅
```php
Route::get("users/{user}/edit", function(Request $request, $user){ // ...
```

❌
```php
Route::get("users/{user}/edit", function($user, Request $request){ // ...
```

# Response

As we already know, every route has closure that is called when user visits the route. This closure must return some response to the user.

Here is what you can return:
- String
- View (More about them later!)
- Integer
- Array - Array will be returned as json, so you can build json API.

Lemon also contains class Response. This class is mainly used for working with status codes and redirecting.

## Response class structure

### Methods

- ::raise(status_code) Returns basic page or if exists, calls handler. Also sets given status code
- ::handle(status_code, action) Sets handler for given status code
- ::redirect(url) Redirects user to given url

Example of using Response class:

```php
<?php
// ...
use Lemon\Http\Response;

// Creating basic 404 page
Response::handle(404, function(){
    echo "I'm sorry, but we couldn't found this page 🔍😥";
});

// Creating basic 500 page
Response::handle(500, function(){
    echo "This is an error... 🐛";
})

// Example of using redirecting and raising
Route::get("/users/", function(){
    try 
    {
        // ...
        return Response::redirect("https://somewhere.com");
    }
    catch(Exception $e)
    {
        return Response::raise(500);
    }
})

// ... 

```
