# Comphar

*Composer + Phar*

Pack all composer dependencies into a single phar file.

## Installation

### 1. Execute composer global installation

Install via [Packagist](https://packagist.org/packages/mpyw/comphar).

```ShellSession
example@localhost:~$ composer global require mpyw/comphar:^1.0
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

### 2. Update `$PATH`

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
  -h, --help             Show help.
  -v, --verbose          Verbose output.
  -o, --out <value>      Output archive name. Default to "vendor.phar".
  -d, --dir <value>      Project root directory. Default to getcwd().
      --yes              Without confirmation.
example@localhost:~$
```

## Example

### 1. Prepare your repository

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

### 2. Generate `vendor.phar`

Let's generate `vendor.phar` in that directory.

```ShellSession
example@localhost:~/my-new-package$ composer update
Loading composer repositories with package information
Updating dependencies (including require-dev)
Nothing to install or update
Writing lock file
Generating autoload files
example@localhost:~/my-new-package$ comphar -v
Project directory: ~/my-new-package
Output filename: ~/my-new-package/vendor.phar
Continue? [y/n]: y
Added: ...
Added: ...
Added: ...
Added: ...
Added: ...
example@localhost:~/my-new-package$
```

#### Note

**Even if you have no dependencies**, you have to call `composer install` or `composer update` to adjust your own library to autoloading.

### 3. Enjoy

You can require `vendor.phar` as well as usual `vendor/autoload.php`.

#### Simplest usage

```php
<?php
use mpyw\MyNewPackage\Foo;
require __DIR__ . '/vendor.phar';
$foo = new Foo();
```

#### Add your own autoloading

```php
<?php
use mpyw\MyNewPackage\Foo;
use mpyw\MyOtherPackage\Bar;
$loader = require __DIR__ . '/vendor.phar';
$loader->addPsr4('mpyw\\MyOtherPackage\\', '~/my-other-package/src');
$foo = new Foo();
$bar = new Bar();
```

### Error: Permission denied

It's possible you run into the error "Permission denied":

```ShellSession
example@localhost:~/my-new-package$ comphar -v
Error: Permission denied: Disable "phar.readonly" and "phar.require_hash"
```

In this case, you need to adjust your configuration at the "php.ini":

```
[Phar]
; http://php.net/phar.readonly
phar.readonly = Off

; http://php.net/phar.require-hash
phar.require_hash = Off
```