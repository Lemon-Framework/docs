# Configuration

Most of lemon components are using configuration. This can be accesed via staticky using zest `\Lemon\Config` or function `config()`.

## Setting config value:

```php
\Lemon\Config::set('service.bar.baz', true);
config('service.bar.baz', true);
```

Now lets cover the key, as you can see it consists of words separated with dots. The first word is service which the other words belong. Ant the other words are nested keys, so in config like this:

```php
[
    'bar' => [
        'baz' => false
    ]
]
```

bar.baz would be false.


## Getting config value

```php
\Lemon\Config::set('service.bar.baz');
config('service.bar.baz');
```

## Getting file

```php
\Lemon\Config::file('service.bar.baz');
```

This will return path to file under this key from perspektive of the project folder.

## Loading from files

We can use multiple config files, where each will represent each service. Load them using `load()` method where the argument is config directory.
