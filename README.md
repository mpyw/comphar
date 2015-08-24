# Comphar

*Composer + Phar*

Pack all composer dependencies into a single phar file.

## Installation

Install via [Packagist](https://packagist.org/packages/mpyw/comphar).

```ShellSession
example@localhost:~$ composer global require mpyw/comphar:@dev
Changed current directory to /Users/mpyw/.composer
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing mpyw/comphar (dev-master XXXXXXX)
    Cloning XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

Writing lock file
Generating autoload files
example@localhost:~$
```

If not yet, you must add **`~/.composer/vendor/bin`** to `$PATH`.  
Append the following statement to `~/.bashrc`, `~/.zshrc` and so on.

```shell
export PATH="~/.composer/vendor/bin:$PATH"
```

## Usage

```ShellSession
example@localhost:~$ comphar -h
Usage: ./comphar [options]
Options:
  -o, --out <value>      Output archive name. Default to "vendor.phar".
  -d, --dir <value>      Project root directory. Default to getcwd().
      --yes              Without confirmation.
  -h, --help             Show help.
example@localhost:~$
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
            "mpyw\\MyNewPackage\\": "src/"
        }
    }
}
```

Let's generate to require `vendor.phar`.

```ShellSession
example@localhost:~/my-new-package$ composer update
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Writing lock file
Generating autoload files
example@localhost:~/my-new-package$ comphar
Create vendor.phar from ~/my-new-package? [y/n]: y
example@localhost:~/my-new-package$
```

```php
<?php
require 'vendor.phar';
use mpyw\\MyNewPackage\\Foo;
$foo = new Foo('bar');
```
