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

