# Installation

Installation is provided via composer, but to simplify your installation, its recommended to use this command in your empty project folder

` curl -s 'https://raw.githubusercontent.com/Lemon-Framework/ProjectBuilder/master/builder' | bash`

It downloads Lemon and creates basic files. Don't worry, these are files that you will need in every project. More about that later.

This is directory structure that you'll get:
```
project/
├── vendor/
├── public/
│   ├── index.php
│   └── .htaccess
├── lemonade
└── composer.json

```

# Deployment

Lemon was already tested on local and Apache deployment.

## Development deployment

For local deployment you can simply use `php lemonade serve` in your directory folder. It will start you basic development server for testing your app locally. More about this command later.

> ❗ Its not recommended to use it in production.

## Apache deployment

For Apache is already `.htaccess` in your `public` folder, so just enable htaccess and `public` folder in your apache config.

## Nginx deployment

Nginx was not tested yet, so contribution is welcomed here. It is possible that [Laravel deployment](https://laravel.com/docs/8.x/deployment#nginx) will work here
