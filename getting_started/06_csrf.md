# Csrf

In previous chapters you might get 400 status code while sending post request on post route. This is because of lemon by default protects your page from csrf vulnerability. To protect you, Lemon on every request creates csrf token, saves it to session and on post request (and other) checks the session token with sended and if it doesn't match it throws 400. So this means we have to send the csrf token in every form. To do this, simply include `{ csrf }` in every form:

```juice

<form method="POST">
    { csrf }
    <input type="text" name="name">
    <input type="password" name="password">
</form>

```

## How to turn it of?

Its common that in e.g. API you don't want csrf protection. To do that, you need to include all the routes in collection and then exclude it. Collections will be covered in next chapters, so for now just follow this patern:

```php

Route::coolection(function() {
    // routes without csrf protection
    Route::post('no-csrf-route', fn() => 'use only for apis');
    // etc
})->exclude(\Lemon\Protection\Middlewares\Csrf::class);

```
