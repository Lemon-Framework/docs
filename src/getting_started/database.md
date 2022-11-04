# Database

Working safely with database is a bit painful. Lemon shrinks all the lines to make it work to just one line.

## Configuring

First of all we have to configure it.

To do that simply start filling .env

> **Warning**
>
> Dont create .env in folder public, make in the parent folder.

First of all we have to setup database driver.

```env
DB_DRIVER=sqlite
```

### Sqlite

To configure sqlite, simply set database file.

```env
DB_DRIVER=sqlite
DB_FILE=database.sqlite
```

### Mysql

To configure mysql, you have to provide host, port, database name, user and password.

```env
DB_DRIVER=mysql
DB_HOST=localhost
DB_PORT=3306
DB_NAME=web
DB_USER=root
DB_PASSWORD=*****
```

You can also provide `DB_UNIX_SOCKET` and `DB_CHARSET`, they both have default value so you dont have to provide them.

### Postgre

Postgre has same configuration as mysql, however instead of `DB_UNIX_SOCKET` and `DB_CHARSET` we can configure `DB_SSLMODE`, which is set to `prefer` by default.

## Running queries

All database manipulation is done by class `\Lemon\DB`. 

To run query, simply use `query()` method.

```php
\Lemon\DB::query('SELECT * FROM users');
```

This method automaticaly connects, keeps one connection for the request and prepares the statement, so its safe.

If you want to add some data inside the query, don't use string interpolation. Just don't do that. Do it like this:

```php
\Lemon\DB::query('SELECT * FROM users WHERE name=?', $name);
```

We can also use named parameters instead of `?`

```php
\Lemon\DB::query('SELECT * FROM users WHERE name=:name', name: $name);
```

And thats all, one line, less pain, safe.
