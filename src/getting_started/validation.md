# Validation

Lemon has tool that simplifies validating. Let's start by validating request.

To validate request, there is method `validate()`.

```php
$validation = $request->validate([
    'name' => 'max:16'
]);

if (!$validation) {
    return template('foo', error: 'Bad data');
}
```

In this method we provide validating rules. Theese rules are in format request-key => rule. If some rule doesn't pass, this method returns false. If the key does not exist, it also returns false.

So if we send request with data `name=foo`, it will pass, however, if we send `name=foobarbazfoobarbazfoobarbaz` or `foo=barr`, it won't.

All the rules are separated by `|` and arguments for the rules are provided using `:`.

Let's see all the rules.

| Rule | Description |
|------|-------------|
| numeric | Value is number |
| notNumeric | Value is not number (but it can contain numbers) |
| email | Value is email |
| url | Value is url |
| color | Value is hexadecimal color |
| max:number | Value is smaller than given number |
| min:number | Value is bigger than given number |
| regex | Value matches regex |
| notRegex | Value doesn't match regex |
| contains | Value contains regex |
| doesntContain | Value doesn't contain regex |

## Custom rules

To create custom rule, we first have to access the rules, to do that simply use `\Lemon\Validator::rules()` method. and then add the rule using `rule()` method.

```php
\Lemon\Validator::rules()->rule('upperCase', function(string $target) {
    return ctype_upper($target);
});
```
