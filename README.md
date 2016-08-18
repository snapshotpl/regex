# Making regex great again

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/regex.svg?style=flat-square)](https://packagist.org/packages/spatie/regex)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/spatie/regex/master.svg?style=flat-square)](https://travis-ci.org/spatie/regex)
[![SensioLabsInsight](https://img.shields.io/sensiolabs/i/33e33c17-13e8-48ff-8a0d-eeb7190715f6.svg?style=flat-square)](https://insight.sensiolabs.com/projects/33e33c17-13e8-48ff-8a0d-eeb7190715f6)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/regex.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/regex)
[![StyleCI](https://styleci.io/repos/65915598/shield)](https://styleci.io/repos/65915598)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/regex.svg?style=flat-square)](https://packagist.org/packages/spatie/regex)

Php's built in `preg_*` functions require some odd patterns like passing variables by reference and treating `false` or `null` values as errors. `spatie/regex` provides a cleaner interface for `preg_match`, `preg_match_all`, `preg_replace` and `preg_replace_callback`.

```php
use Spatie\Regex\Regex;

// Using `match`
Regex::match('/a/', 'abc'); // `MatchResult` object
Regex::match('/a/', 'abc')->hasMatch(); // true
Regex::match('/a/', 'abc')->result(); // 'a'

// Capturing groups with `match`
Regex::match('/a(b)/', 'abc')->result(); // 'ab'
Regex::match('/a(b)/', 'abc')->group(1); // 'a'

// Using `matchAll`
Regex::matchAll('/a/', 'abcabc')->hasMatch(); // true
Regex::matchAll('/a/', 'abcabc')->results(); // Array of `MatchResult` objects

// Using replace
Regex::replace('/a/', 'b', 'abc')->result(); // 'bbc';
Regex::replace('/a/', function (MatchResult $result) {
    return $result->result() . 'Hello!';
}, 'abc')->result(); // 'aHello!bc';
```

Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## Postcardware

You're free to use this package (it's [MIT-licensed](LICENSE.md)), but if it makes it to your production environment you are required to send us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

The best postcards will get published on the open source page on our website.

## Installation

You can install the package via composer:

``` bash
composer require spatie/regex
```

## Usage

### `Regex::match(): MatchResult`

Matches a pattern on a subject. Returns a `MatchResult` object for the first match.

```php
/**
 * @param string $pattern
 * @param string $subject
 *
 * @return \Spatie\Regex\MatchResult
 */
Regex::match(string $pattern, string $subject): MatchResult
```

#### `MatchResult::hasMatch(): bool`

Checks if the pattern matches the subject.

```php
Regex::match('/abc/', 'abc')->hasMatch(); // true
Regex::match('/def/', 'abc')->hasMatch(); // false
```

#### `MatchResult::result(): string`

Return the full match that was made. Returns `null` if no match was made.

```php
Regex::match('/abc/', 'abc')->result(); // 'abc'
Regex::match('/def/', 'abc')->result(); // null
```

#### `MatchResult::group(int $id): string`

Return the contents of a captured group (with a 1-based index). Throws a `RegexFailed` exception if the group doesn't exist.

```php
Regex::match('/a(b)c/', 'abc')->group(1); // 'b'
Regex::match('/a(b)c/', 'abc')->group(2); // `RegexFailed` exception
```

### `Regex::matchAll(): MatchAllResult`

Matches a pattern on a subject. Returns a `MatchAllResult` object containing all matches.

```php
/**
 * @param string $pattern
 * @param string $subject
 *
 * @return \Spatie\Regex\MatchAllResult
 */
public static function matchAll(string $pattern, string $subject): MatchAllResult
```

#### `MatchAllResult::hasMatch(): bool`

Checks if the pattern matches the subject.

```php
Regex::matchAll('/abc/', 'abc')->hasMatch(); // true
Regex::matchAll('/abc/', 'abcabc')->hasMatch(); // true
Regex::matchAll('/def/', 'abc')->hasMatch(); // false
```

#### `MatchAllResult::results(): array`

Returns an array of `MatchResult` objects.

```php
$results = Regex::matchAll('/ab([a-z])/', 'abcabd')->results();

$results[0]->result(); // 'abc'
$results[0]->group(1); // 'c'
$results[1]->result(); // 'abd'
$results[1]->group(1); // 'd'
```

### `Regex::replace(): ReplaceResult`

Replaces a pattern in a subject. Returns a `ReplaceResult` object.

```php
/**
 * @param string|array $pattern
 * @param string|array|callable $replacement
 * @param string|array $subject
 * @param int $limit
 *
 * @return \Spatie\Regex\ReplaceResult
 */
public static function replace($pattern, $replacement, $subject, $limit = -1): ReplaceResult
```

#### `ReplaceResult::result(): mixed`

```php
Regex::replace('/a/', 'b', 'abc')->result(); // 'bbc'
```

`Regex::replace` also works with callables. The callable will receive a `MatchResult` instance as it's argument.

```php
Regex::replace('/a/', function (MatchResult $matchResult) {
    return str_repeat($matchResult->result(), 2);
}, 'abc')->result(); // 'aabc'
```

Patterns, replacements and subjects can also be arrays. `Regex::replace` behaves exactly like [`preg_replace`](http://php.net/manual/en/function.preg-replace.php) in those instances.

### Error handling

If anything goes wrong in a `Regex` method, a `RegexFailed` exception gets thrown. No need for checking `preg_last_error()`.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Credits

- [Sebastian De Deyne](https://github.com/sebastiandedeyne)
- [All Contributors](../../contributors)

## About Spatie

Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
