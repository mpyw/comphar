# Comphar

*Composer + Phar*

Pack all composer dependencies into a single phar file.

## Installation

Install via [Packagist](https://packagist.org/packages/mpyw/comphar).

```ShellSession
example@localhost:~$ composer global require mpyw/comphar:@dev
```

If not yet, you must add **`~/.composer/vendor/bin`** to `$PATH`.  
Append the following statement to `~/.bashrc`, `~/.zshrc` and so on.

```shell
export PATH="~/.composer/vendor/bin:$PATH"
```

## Usage

```ShellSession
example@localhost:~$ comphar -h
Usage: comphar [options] [operands]
Options:
  -o, --out <arg>         Output archive name. Default value is "vendor.phar".
  -d, --dir <arg>         Project root directory. Default value is current directory.
  -h, --help              Show help.
```

## Example

Prepare `composer.json`.

```json
{
    "name": "mpyw/my-new-package",
    "description": "This is a stupid example",
    "require": {
        "mpyw/my-dependency-1": "@dev",
        "mpyw/my-dependency-2": "1.0.0",
    },
    "autoload": {
        "psr-4": {
            "mpyw\\MyNewLibrary\\": "src/"
        }
    }
}
```

Let's generate to require `vendor.phar`.

```ShellSession
example@localhost:~/my-new-package$ composer update
example@localhost:~/my-new-package$ comphar
```

```php
<?php
require 'vendor.phar';
use mpyw\\MyNewLibrary\\Foo;
$foo = new Foo('bar');
```
