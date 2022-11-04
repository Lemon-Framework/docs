# Debugging

Lemon has 2 important debugging tools, they both work only if you enable debug mode. To do that, simply setup config value `debug.debug` to true.

```php
config('debug.debug', true);
```

### Reporter

Lemon Reporter is error handling panel. It shows everytime you get an error.

On the top you can see the Error along with hint, this hint is powered by so-called Consultant. 

Under that you can see 3 tabs, the first contains file navigator stack trace. The second has more hints and the last has request.

### Dumper

While debugging you want to display data on the screen, you can obviously use stuff like print_r() or var_dump(), but this isn't that good.

In lemon there are 2 functions. `d()` and `dd()`. The first just dumps data, it uses gruvbox colorscheme and it can be folded. The second dumps the data and exits whole app.

