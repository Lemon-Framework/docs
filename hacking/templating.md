# Hacking juice

Juice was designed to be hackable. Let's see how it actualy works and how you can hack it. This part refers to [sourcecode](https://github.com/Lemon-Framework/Lemon/tree/master/src/Lemon/Templating/Juice), so its recommended to have oppened it.

Juice transpiler has 2 parts, lexer and parser, you can technicaly write your own transpiler based on Juice and write own lexer/parser.


## Lexer

Lexer uses regular expressions to convert template into array of tokens. Theese regexes are loaded from config from `templating.juice.syntax` as class `\Lemon\Templating\Juice\Syntax`.

## Parser

Parser takes all the tokens and iteratively converts each token back into php file. Text tokens are rendered as they are, but their context is determined so we can have context-aware escaping. 

Output tokens are parsed using `\Lemon\Templating\Juice\Compilers\OutputCompiler`.

Directives are parsed using `\Lemon\Templating\Juice\Compilers\DirectiveCompiler` and each lives in its own class.

### Adding directive

Using method `Compiler::addDirective()` (which is also service so `\Lemon\Juice::addDirective()`) we can add custom directive. Every directive must implement `\Lemon\Templating\Juice\Compilers\Directives\Directive`, if it has method `compileClosing` its closable directive.
