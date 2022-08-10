# Lemon Architecture

This article covers Lemon architecture and its dedicated to people who already understand frameworks.

Lemon uses psr-4 autoload along with psr-12 coding standart and strict types. Each component lives in its own directory and most of them have its own zest (facade) located under \Lemon\ for easier usage. Main class is \Lemon\Kernel\Lifecycle, it is based on \Lemon\Kernel\Container which is simple psr-complaint DI container and has some features for managing the whole app. To make it friendlier to beginers there is ::init() which sets everything up and boots the app using `register_shutdown_function`.

For testing, Lemon is using phpunit and for static analysis phpstan with level 5.
