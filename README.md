# The Easiest way to use Coding Standard

[![Downloads total](https://img.shields.io/packagist/dt/symplify/easy-coding-standard.svg?style=flat-square)](https://packagist.org/packages/symplify/easy-coding-standard/stats)

<div class="text-align: center">
<img src="https://avatars.githubusercontent.com/u/123805080?s=200&v=4" style="width: 15em; height: 15em">
</div>

<br>

## Killer Features

- Install without conflicts on **any PHP 7.2+** project
- Blazing fast parallel run out of the box
- Use [PHP_CodeSniffer or PHP-CS-Fixer](https://tomasvotruba.com/blog/2017/05/03/combine-power-of-php-code-sniffer-and-php-cs-fixer-in-3-lines/) - anything you like

- Use **prepared sets** to save time - PSR-12, arrays, use statements, spaces and more

```php
$ecsConfig->sets([SetList::PSR_12]);
```

- Use **php-cs-fixer sets** from [like you're used to](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/blob/master/doc/ruleSets/index.rst)

```php
$ecsConfig->dynamicSets(['@Symfony']);
```

- Skipping files for specific checkers

<br>

Are you already using another tool? We got you covered:

* See how to migrate from [PHP_CodeSniffer](https://tomasvotruba.com/blog/2018/06/04/how-to-migrate-from-php-code-sniffer-to-easy-coding-standard)
or [PHP CS Fixer](https://tomasvotruba.com/blog/2018/06/07/how-to-migrate-from-php-cs-fixer-to-easy-coding-standard)

<br>

## Install

```bash
composer require symplify/easy-coding-standard --dev
```

<br>

## Usage

### 1. First Run

To start using ECS, just run it:

```bash
vendor/bin/ecs
```

ECS will create `ecs.php` config with directories in your project and first rule to kick off.

<br>

### 2. Run Again

```bash
vendor/bin/ecs
```

This will run ECS in *dry run* - it will show you the diff before they get applied. To fix code style, run this command:

```bash
vendor/bin/ecs --fix
```

<br>

## Configuration

Configuration can be extended with many options. Here is list of them with example values and little description what are they for:

```php
use PhpCsFixer\Fixer\ArrayNotation\ArraySyntaxFixer;
use Symplify\EasyCodingStandard\Config\ECSConfig;

return static function (ECSConfig $ecsConfig): void {
    $ecsConfig->paths([__DIR__ . '/src', __DIR__ . '/tests']);

    $ecsConfig->skip([
        // skip whole rule
        ArraySyntaxFixer::class,

        // skip directory by absolute
        __DIR__ . '/src/Migrations',

        // skip directories by mask
        __DIR__ . '/src/*/Legacy',

        // skip single rule in particular paths
        LineLenghtFixer::class => [
            __DIR__ . '/src/ValueObject/File.php',
        ],
    ]);

    // file extensions to scan [default: [php]]
    $ecsConfig->fileExtensions(['php', 'phpt']);

    // configure cache paths & namespace - useful for Gitlab CI caching, where getcwd() produces always different path
    // [default: sys_get_temp_dir() . '/_changed_files_detector_tests']
    $ecsConfig->cacheDirectory('.ecs_cache');

    // [default: \Nette\Utils\Strings::webalize(getcwd())']
    $ecsConfig->cacheNamespace('my_project_namespace');

    // indent and tabs/spaces [default: spaces]
    $ecsConfig->indentation('tab');

    // end of line [default: PHP_EOL]; other options: "\n"
    $ecsConfig->lineEnding("\r\n");
};
```

<br>

## FAQ

### How do I clear cache?

```bash
vendor/bin/ecs check src --clear-cache
```

### How to load custom Config?

```bash
vendor/bin/ecs check src --config another-config.php
```

<br>

## Acknowledgment

The parallel run is heavily inspired by [phpstan/phpstan-src](https://github.com/phpstan/phpstan-src).
