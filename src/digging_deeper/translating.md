# Translating

Lemon provides simple way of translating your application. We simply define all texts of our webbsite in translation files and then, instead of writing all the text, we just enter what text we want and lemon will write it in curent language.

## Configuring

Create new config file `config/translating.php` (see [configuring](../getting_started/config.md) for more information).

And fill it with this:

```php
<?php

declare(strict_types=1);

return [
    'directory' => 'translations',
    'fallback' => 'en',
];
```

Where directory is folder of all your translations and fallback is default language.

## Creating translation files

Translation file is php file that returns all texts. It is contained in configured directory and its name is language name e.g `en` or `cs`.

Example of `translations/en.php`:

```php
<?php

return [
    'title' => 'Welcome to the Lemon Framework',
    // etc.
];
```

Example of `translations/cs.php`:

```php
<?php

return [
    'title' => 'Vitejte v Citronove Ramopraci',
    // etc.
];
```

## Setting language

To setup language simply use `\Lemon\Translator::locate()` with language as string. Be aware that this will set language only for curent request. To make it more long-lasting, save it into session and use dedicated middleware.

Middleware `\Lemon\Translating\Middlewares\TranslationLocalizer` automaticaly sets language from session key `locale`.

Example of language-setting route:

```php
Route::get('lang/{$lang}', function($lang) {
    \Lemon\Session::set('locale', $lang);
});

Route::collection(function() {
    // every route in this group will have middleware that automaticaly sets language from session
    Route::get('/', fn() => template('home'));
})->middleware(\Lemon\Translating\Middlewares\TranslationLocalizer::class);
```

When we send `get` to url `lang/cs` our language will be set to czech. Once we go to `/` it will load our language from session and our web will use czech translations.

## Retreiving texts

OK, we know how to set the language, but how to actualy retreive texts. We can either use function `text()` so if we do something like:

```php
Route::get('title', function() {
    return text('title');
});
```

and depending on curently set language we will see either `Welcome to the Lemon Framework` or `'Vitejte v Citronove Ramopraci'`

In templates, there is directive `{ text }` which does basicaly the same but you dont need to use quotes:

```juice
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{ text title }</title>
</head>
<body>
    ...
</body>
</html>
```
