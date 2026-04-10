# phpnomad/singleton

[![Latest Version](https://img.shields.io/packagist/v/phpnomad/singleton.svg)](https://packagist.org/packages/phpnomad/singleton) [![Total Downloads](https://img.shields.io/packagist/dt/phpnomad/singleton.svg)](https://packagist.org/packages/phpnomad/singleton) [![PHP Version](https://img.shields.io/packagist/php-v/phpnomad/singleton.svg)](https://packagist.org/packages/phpnomad/singleton) [![License](https://img.shields.io/packagist/l/phpnomad/singleton.svg)](https://packagist.org/packages/phpnomad/singleton)

`phpnomad/singleton` is a single-trait package for [PHPNomad](https://phpnomad.com). It provides `WithInstance`, a trait that gives any class a static `instance()` method returning a lazily-created, per-class singleton. It has zero runtime dependencies and is what `phpnomad/facade` and the rest of the framework use whenever a class needs a single shared instance without reaching for a container.

## Installation

```bash
composer require phpnomad/singleton
```

## Quick Start

Add the trait to a class, then call `instance()` to get the shared object.

```php
<?php

use PHPNomad\Singleton\Traits\WithInstance;

class Registry
{
    use WithInstance;

    private array $items = [];

    public function register(string $key, $value): void
    {
        $this->items[$key] = $value;
    }

    public function get(string $key)
    {
        return $this->items[$key] ?? null;
    }
}

Registry::instance()->register('theme', 'dark');
$theme = Registry::instance()->get('theme'); // 'dark'
```

Every call to `Registry::instance()` returns the same object for the lifetime of the request.

## Overview

The package is intentionally tiny. The trait adds a protected static `$instance` property and a public static `instance()` method, and nothing else.

- Lazy initialization, so the instance is only built the first time `instance()` is called
- Late static binding (`new static`), so subclasses each get their own singleton rather than sharing the parent's
- Zero runtime dependencies, which means adding it to a project pulls in nothing else
- Lives under the `PHPNomad\Singleton\Traits` namespace, importable via `use PHPNomad\Singleton\Traits\WithInstance;`
- Used by `phpnomad/facade` to back its static service facades across the framework

## Documentation

The full package guide, including inheritance behavior, testing strategies, and when to reach for a DI container instead, lives at [phpnomad.com](https://phpnomad.com).

## License

MIT, see [LICENSE.txt](LICENSE.txt) for the full text.
