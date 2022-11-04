# Terminal in depth

Untill we started spliting our app, we were able to use index as cli - `php public/index.php` but this doesn't work anymore, we have to implement this by our own. Create file `lemonade`, actually you can name it as you want. This file will be our cli. We have to put here code that will actually run our commands, based on Terminal component.

```php
#!/usr/bin/php
<?php

/** @var \Lemon\Kernel\Application $app */
$app = include __DIR__.'/init.php';

// --- Loading commands ---
$app->loadCommands();

$app->get('terminal')->run(array_slice($argv, 1));
```

This code takes terminal component from app and runs it with arguments.

Now if we do `php lemonade` it will work as we know.

## Custom commands

You can also add your own commands. To do that, create file with commands (e.g `commands.php`) and load it:

```php
#!/usr/bin/php
<?php

/** @var \Lemon\Kernel\Application $app */
$app = include __DIR__.'/init.php';

// --- Loading commands ---
$app->loadCommands();

require __DIR__.'/commands.php';

$app->get('terminal')->run(array_slice($argv, 1));
```

To define command, use `Terminal::command()`:

```php
<?php

use Lemon\Terminal;

Terminal::command('say:hi {name}', function($name) {
    echo 'Hi '.$name;
}, 'says hi');
```

`{name}` is required command argument, to make optional, use `{name?}`. Then there is function which is executed and description.

Now, if we type `php lemonade say:hi name=majkel` we will get `Hi majkel`.

## Html rendering

To make our commands more beautiful, we can render text with colours. Regulary, to do e.g red text, you would have to do `\033[33mHello\033[0m`. Lemon provides html rendering inspired by [termwind]().

To render you have to use `$this->out()`:

```php
<?php

use Lemon\Terminal;

Terminal::command('say:hi {name}', function($name) {
    $this->out("<div class='text-red'>Hi {$name}</div>");
}, 'says hi');
```

Supported tags:
    - div
    - h1
    - hr 
    - b 
    - i
    - u 
    - br 
    - p

Supported classes:
    - text-black
    - text-red
    - text-green
    - text-yellow
    - text-blue
    - text-magenta
    - text-cyan
    - text-white
    - bg-black
    - bg-red
    - bg-green
    - bg-yellow
    - bg-blue
    - bg-magenta
    - bg-cyan
    - bg-white
