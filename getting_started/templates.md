# Templating

Php itself was designed as template engine, problem is that to make it safe you have to be carefull and its more work, Juice solves that. Juice is Lemons template engine with syntax similar to php. Theese templates can't contain any logic and they are only for displaying content. Juice templates are transpiled into clear php and saved which means they are as fast as writing regular php.

## Creating templates

Templates by default are stored in folder templates outside of folder public. Every trmplate has extension .juice

## Displaying templates

To display template, simply return Template instance, this can be obtained with either Zest `\Lemon\Template::make('name', variables)` or function `template('name', key: value)` where name doesn't have extension and . will be replaced with directory separator. Variables you pass (in zest as array, in function using named arguments) will be then available in the template.

## Syntax

Syntax can be divided in two parts, output and directive.

## Output

In raw php you would do something like `<?php echo $variable ?>` however this is unsafe because if our variable has some html content inside it will be rendered which can lead to (xss attack)[https://en.wikipedia.org/wiki/Cross-site_scripting?wprov=sfti1]. In Juice you can use `{{ content }}` syntax which thanks to context-aware escaping (concept from nette) is lot safer as it escapes differently in different parts of html.

If you for some reason don't want to use escaping, there is `{! content !}`. But use it only if you know for 100 % what you are doing.

## Directives

Directives are basicaly control structures such as conditions, etc. They use `{ }` syntax and they are like `<?php ?>` with exception that dey dont use braces. They are divided into pairable (those which must be closed) and unpairable. Closing tags use one of `/` or `end` with name of directive that is being closed.

### Conditions

```juice

{ if $foo === 1 }
    foo
{ elseif $foo > 1 } 
    bar
{ else }
    baz 
{ endif }

```

Instead of `{ endif }` you can use `{ /if }` which as covered before also applies to every pair directive.

For `{ if !something }` there is syntax sugar `{ unless something }`

### Loops

```juice

{ foreach $foo as $item }
    ...
{ /foreach }

{ while $1 }
    ...
{ /while }

{ for $i; $i < 100; ++$i }
    ...
{ /for }

```

### Switch

```juice

{ switch $foo }
    foo
{ case 1} 
    bar
{ case 2 }
    baz 
{ endswitch }

```

### Include

Include directive supports to add content of another template into curent.

```

{ include 'foo' }

```

### Layout
    
Layout are basicaly templates for templates, you define layout template with `yields` which are spots that will be replaced with blocks in template that extends this layout.

```
{-- layout.juice --}

TODO
    
```
