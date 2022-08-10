# HTTP abstraction

Lemon has builtin http abstraction which helps you work with request and response.

## Request

Class \Lemon\Http\Request is created from sent data before booting (curently done with Lifecycle::init()). 

While creating routing callback, we can `inject` this request into the function (injecting will be covered in lifecycle chapter), we simply do that by specifying the type in the function.

```php
Route::post("/login", function(\Lemon\Http\Request $request) {
//... 
});
```

The thing that matters here is the type, not order so we can use it with dynamic parameters which are using name to pass it.

```php
Route::post("/posts/edit/{post}", function(\Lemon\Http\Request $request, $post) {
//... 
});

// will work the same as

Route::post("/posts/edit/{post}", function($post, \Lemon\Http\Request $request) {
//... 
});

```

Alternative is again Zest \Lemon\Request.

### Working with request

The most basic things can be reached using properties, let's say we've sent post to /posts/edit/foo?darkmode=true

```php
Route::post("/posts/edit/{post}", function($post, \Lemon\Http\Request $request) {
    $request->path // /posts/edit/foo
    $request->query // ?darkmode=true
    $request->method // POST
    $request->headers // ["Content-Type" => "application/x-www-form-urlencoded"]
    $request->body // title=something
    $request->cookies // ["CSRF_TOKEN" => "lmoyiVzP82ZvSUSXx0FaOvpwdv1h4uxj"]
});
```

#### Obtaining data

To obtain data there are several methods.

To receive data from query there is query() method. This method returns either single value or all data as array. So let's say we've sent request to /?foo=bar&bar=baz

```php
$request->query(); // ["foo" => "bar", "bar" => "baz"]
$request->query("foo"); // "bar"
```

While obtaining data from body (e.g. in POST) we can either get all the data as array with method `data()` or get single value using either method `get(key)` or just as property.

```php
$request->data(); // ["foo" => "hello", "bar" => "baz"]
$request->get("foo"); // "hello"
$request->foo; // "hello"
```

You can also check if value exists using `has(key)`

```php
$request->data(); // ["foo" => "hello"]
$request->has("foo"); // true
$request->has("bar"); // false
```

Data from body are parsed automaticaly depending on content type.

You can also check the content type using `is(type)`

#### Working with headers

Headers operarions are similar to body:

```php
$request->headers(); // ["Content-Type" => "application/x-www-form-urlencoded"]
$request->hasHeader("Content-Type"); // true
$request->hasHeader("foo"); // false
$request->header("Content-Type"); // application/x-www-form-urlencoded
```

#### Cookies

And the same goes for cookies

```php
$request->cookies(); // ["CSRF_TOKEN" => "lmoyiVzP82ZvSUSXx0FaOvpwdv1h4uxj"]
$request->hasCookie("CSRF_TOKEN"); // true
$request->hasHeader("foo"); // false
$request->cookie("CSRF_TOKEN"); // lmoyiVzP82ZvSUSXx0FaOvpwdv1h4uxj
```

### Response


