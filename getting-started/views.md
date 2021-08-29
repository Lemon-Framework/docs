# Views

Lemon has its own **views** system inspired by jinja. Views are perfect way to write cleaner and efficient code. They are just files with html content, that is rendered in our route callbacks.

## Get started with views 
First of all, setup your views folder. To do that, just put this in your `public/index.php`:

```php
// public/index.php

<?php
// ...
use Lemon\Views\ViewCompiler;

ViewCompiler::setDirectory(__DIR__ . "/../views");

// ...

```
Then create the folder in your project directory, in this example it will be folder `views`.

Our project directory should look like this:
```
project/
├── vendor/
├── public/
│   ├── index.php
│   └── .htaccess
├── views/
├── lemonade
└── composer.json
```

Views are just files with html content. In Lemon every view must have suffix `.lemon.php`. So lets create one!

Lets create view welcome.lemon.php:
```
project/
├── vendor/
├── public/
│   ├── index.php
│   └── .htaccess
├── views/
│   └── welcome.lemon.php
├── lemonade
└── composer.json
```
// ───────────────
Now add some html:
```html
<!-- views/welcome.lemon.php -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lemon Land</title>
</head>
<body>
    <h1>Lemon Land 🍋</h1>
    Welcome in Lemon Land!
</body>
</html>
```

Perfect! Now we have to render the view. Show it to the world!

For rendering views has Lemon function called `view()`.

In practice we use it like this:
```php
// public/index.php

// ...

Route::get("/", function(){
    return view("welcome");
});

// ...

```
Yes. We don't need to add the `.lemon.php` part.


## Digging deeper

Main feature of views is rendering dynamic content. Lemon view Kernel allows us to add variables inside views and lot more!

### Passing variables

Function `view` has second optional argument. This argument is array of variables that we want to pass. This array is in format `["variable_name"=>"variable_value"]`. To create this array you can use [`compact()`](https://www.php.net/manual/en/function.compact.php) function.

#### Syntax

- `{{ variable_name }}` Displays variable content and prevents [xss](https://en.wikipedia.org/wiki/Cross-site_scripting)
- `{! variable_name !}` Displays variable content, but it won't prevent [xss](https://en.wikipedia.org/wiki/Cross-site_scripting)

> 💡 You can also pass functions/methods that return valid type (String, int).

Lets look at some example
```php
// public/index.php

// ..

Route::get("/", function(){
    $date = date("d. m. Y");
    return view("welcome", ["date"=>$date]); // Or return view("welcome", compact($date));
});

// ...

```

```html
<!-- views/welcome.lemon.php -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lemon Land</title>
</head>
<body>
    <h1>Lemon Land 🍋</h1>
    Welcome in Lemon Land!
    <br>
    Today its a nice day! Its {{ $date }}
</body>
</html>
```

### Statements

Lemon View Kernel also provides passing statements like conditions and loops. It is based on [php escaping](https://www.php.net/manual/en/language.basic-syntax.phpmode.php)

#### Syntax

- Conditions:

    You can make conditional statements using `{% if %}`, `{% else: %}`, `{% else if (condition): %}` and finally close the statement with `{% endif; %}`.

    Example:

    ```jinja
    {% if ($user == "CoolFido"): %}
        Podvodník spotted!
    {% else: %}
        Hello {{ $user }}
    {% endif %}
    ```
- For-each:

    You can iterate over items using {% foreach %}.

    Example:

    ```jinja
    // Returns every item of $arr array
    {% foreach($arr as $item): %}
        {{ $item }}
    {% endfor; %}
    ```

### Mappings

There are also some mappings that just replace the keywords with text.

- `@csrf` -> `<input type="hidden" value="CSRF TOKEN LOADED FROM SESSION" name="csrf_token">'` [More about CSRF]()

> 💡 You can fit any php code inside `{% %}` but it was designed for [escaping](https://www.php.net/manual/en/language.basic-syntax.phpmode.php). Passed variables **won't** be printed out.
