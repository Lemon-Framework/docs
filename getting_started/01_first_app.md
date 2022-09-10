# First app

Let's create first app. After we've prepared our app (as covered in (installation)[]) we can start writing in our entry point.

First, we include composer libraries.

```php
<?php

include __DIR__.'/../vendor/autoload.php';

```

Now we have to initialize our app. For this there is setup-method:

```php
<?php

use Lemon\Kernel\Application;

include __DIR__.'/../vendor/autoload.php';

Application::init(__DIR__);

```

Now what exactly is going on here?

As we know, in traditional php every file represents single route. In lemon there is visible only one file, to make it simpler we will be using only this file for a while (but don't worry we will split it). Every request is redirected to this file and here we have Lemon. We use Application::init() to start our app, this will setup everyrhing for us (but don't worry we will explain what it does). So everytime there is request, Lemon finds route that matches given request and returns response.
