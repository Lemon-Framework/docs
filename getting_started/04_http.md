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

There are several classes representing response with different body. All of them inherit class \Lemon\Http\Response. Each class acts differently while sending body.

#### Creating Response in routing

Response is automaticaly created from return type of routing callback using ResponseFactory.

If you return scalar type (string, bool, float, int) it will act normaly - just display it.

Returning array or class that implements \Lemon\Http\Jsonable will send it as json (with content-type application/json). 

```php
Route::get('/api/users', function() {
    return ['majkel', 'coolfido'];
});
```

If you return template (see [templating]()) it will render it.

```php
Route::get('/home', function() {
    return template('home');
});
```

You can also return any Response object which is handy if we want to tweak the response.

#### Creating Response Manualy

As said before, class \Lemon\ResponseFactory is able to create route using callback. But not just that. This was done thanks to `::make()` method, there is another similar, which instead of callback takes any value and its called `::resolve()`. 

Another method is `error()` this method takes http status code and returns http error response.

```php
Route::get('/home', function() {
    return \Lemon\ResponseFactory::error(401);
});
```

This will send simple 401 status page.

To create own pages you can simple create templates. They will be stored in `templates/errors` and each named as `code.juice`.

Also, you can create handlers, theese will be executed to create the response similary as routing callbacks.

```php
\Lemon\Response::handle(401, function() {
    return 401;
});

Route::get('/home', function() {
    return \Lemon\ResponseFactory::error(401);
});
```

Now we will see just 401.

To make it simpler you can use `error()` function.

To create redirect, you can use function `redirect(path)`. This method returns RedirectResponse which is dedicated to redirecting.

Also, you can create all of this by hand, let's see all default responses. They are all located under `\Lemon\Http\Reponses\`

EmptyResponse wont contain any body, its mostly returned by middlewares.

HtmlResponse is most basic response which contains html body that will be send.

JsonResponse contains body that will be send as json.

RedirectResponse is similar to EmptyResponse but is used for redirecting.

TemplateResponse is similar to HtmlResponse but before sending it renders template from its body.

TextResponse sends body as plain text.

#### Manipulating response

You can set basicaly every parameter of response. 

Theese are methods to work with headers.

```php
$response->header('Content-Type', 'text/html'); // sets given header to given value
$response->header('Content-Type'); // returns value of given header (in this case text/html)

$response->location('/'); // Sets location header to /
$response->redirect('/'); // alias to location
```

To work with status code there is `code()` method

```php
$response->code(404) // sets code to given value (404)
$response->code() // returns status code (404)
```

To set body there is `body()` method

```php
$response->body('Hello!');
```

To send cookie there is `cookie()` method.

```php
$response->cookie('REMEMBER', true, 10000); // sets cookie REMEMBER to true for 10000 seconds
```


To sum it up, if we want to for example send json with status code 404, we can do that by:

```php
return \Lemon\Response::resolve(['code' => 404])
    ->code(404)
;
```

We just make response (this is best way as it directly returns the response and you don't have to mind all the response objects) and then edit it.
