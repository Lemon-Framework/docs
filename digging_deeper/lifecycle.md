# Lemon Lifecycle

As we know, brain of every application is class `\Lemon\Kernel\Application`. Till now we've been using `Application::init()` which works for smaller apps, however its better to split it into multiple function calls.

So what does it function actualy do?

`Application::init()` does process called "bootstrapping", it setups the app and automaticaly boots it. We will setup the app in separate file outside of folder public, and use index.php to only run the app. So at first, let's create file `init.php`.

Now, lets include all composer libraries.

```php
include __DIR__.'/vendor/autoload.php';
```

Then, create application instance.

```php
include __DIR__.'/vendor/autoload.php';

use Lemon\Kernel\Application;

$application = new Application(__DIR__);

```

After that, we have to load all services. This requires another key concept called `Dependency injection`.

## Dependency injeciton

Dependency injeciton is way of managing multiple components of single app and it works like this. We have central class, called container, in this case its Application, but Application actually inherits \Lemon\Kernel\Container. 

So what is the container?

Container is basicaly class which automaticaly creates instances.

Let's see it on example

```php

class Foo
{
    public function sayHi(string $name): string
    {
        return 'Hi '.$name;
    }
}

$container = new \Lemon\Kernel\Container();

$container->add(Foo::class);

echo $container->get(Foo::class)->sayHi('CoolFido'); // Hi CoolFido

```

In this example, you can see that we've added service `Foo` to the container and then get it. The most important thing is, that instance of service we want to obtain is created only once and when we first get it.


```php

class Foo
{
    public function sayHi(string $name): string
    {
        return 'Hi '.$name;
    }
}

class Bar
{

}

$container = new \Lemon\Kernel\Container();

$container->add(Foo::class);
$container->add(Bar::class);

echo $container->get(Foo::class)->sayHi('CoolFido'); // Hi CoolFido
echo $container->get(Foo::class)->sayHi('Majkel'); // Hi Majkel

```

In this example we've added another service `Bar`, but since we've never accessed it, the instance will never be created. We can also see that we've used the same class twice, and in both examples it was the same instance.

But the most important of this, that we can actually inject other services.

If we want to for example use our method `sayHi` inside class `Bar`. we simply inject it via type in constructor.

```php

class Foo
{
    public function sayHi(string $name): string
    {
        return 'Hi '.$name;
    }
}

class Bar
{
    public function __construct(
        private Foo $foo
    ) {

    }

    public function sayHiLouder($name): string
    {
        return $this->foo->sayHi($name).'!!!';
    }
}

$container = new \Lemon\Kernel\Container();

$container->add(Foo::class);
$container->add(Bar::class);

echo $container->get(Foo::class)->sayHi('CoolFido'); // Hi CoolFido
echo $container->get(Bar::class)->sayHiLouder('Majkel'); // Hi Majkel!!!

```

And since every service in lemon is written like this (other services are injected) we have to add all of them into our container (which is in this case Application). luckily, there is method that automaticaly loads it.

```php
include __DIR__.'/vendor/autoload.php';

use Lemon\Kernel\Aplication;

$application = new Application(__DIR__);

$application->loadServices();

```

We can also inject into callbacks such as route actions.

## Zests

So if we want to use some service, we can inject it, but while reading you've came across Zests. Zests are nothing but classes that let you staticaly call methods on services. And every service in lemon has its Zest. Every zest is located right under `\Lemon` namespace thanks to which they are simpler to reach.

But to use them, we have to load them.


```php
include __DIR__.'/vendor/autoload.php';

use Lemon\Kernel\Aplication;

$application = new Application(__DIR__);

$application->loadServices();

$application->loadZests();

```


Another important step is handling errors, to do that, simply load lemon handler using `loadHandler()`.

```php
include __DIR__.'/vendor/autoload.php';

use Lemon\Kernel\Aplication;

$application = new Application(__DIR__);

$application->loadServices();

$application->loadZests();

$application->loadHandler();

```


Now we just have to return the app.

```php
include __DIR__.'/vendor/autoload.php';

use Lemon\Kernel\Aplication;

$application = new Application(__DIR__);

$application->loadServices();

$application->loadZests();

$application->loadHandler();

return $application;

```

In this file you can also do other bootstrapping stuff such as loading config or routes.

Now in our index file we simply require init file and boot our app.

```php

<?php

use Lemon\Http\Request;

/** @var \Lemon\Kernel\Application $application */
$application = include __DIR__.'/../init.php';

$application->add(Request::class, Request::capture()->injectApplication($application));
$application->alias('request', Request::class);

$application->boot();
```

We also have to capture the request and add it to the container.

Now what about routes? Where to put them? Lets see [routing in depth]()
