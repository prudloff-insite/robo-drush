# Robo Drush Extension

Extension to execute Drush commands in [Robo](https://github.com/Codegyre/Robo).

Runs Drush commands in stack. You can define global options for all commands (like Drupal root and uri).

The option -y assumed by default but can be overridden on calls to `exec()` by passing `false` as the second parameter.

## Table of contents

- [Differences with upstream](#differences-with-upstream)
- [Installation](#installation)
- [Testing](#testing)
- [Usage](#usage)
- [Examples](#examples)

## Differences with upstream

This package was forked from [boedah/robo-drush](https://github.com/boedah/robo-drush). It adds the following changes:

- Adds support for PHP 8.1.
- Adds support for Drush 10, 11.
- Adds support for Robo 3, 4.
- Drops support for PHP 5.5, 5.6 and 7.0.

## Installation

For new projects (and Robo >= 1.0.0-RC1), just do:

    composer require --dev boedah/robo-drush

For older versions of Robo, use:

- `~1.0`: Robo <= 0.4.5
- `~2.1`: Robo >= 0.5.2

## Testing

`composer test`

## Usage

Use the trait (according to your used version) in your RoboFile:

```php
class RoboFile extends \Robo\Tasks
{
    // if you use robo-drush ~2.1 for Robo >=0.5.2, or robo-drush >3 for Robo >=1.0.0-RC1, or robo-drush form Robo >=2.0.0
    use \Boedah\Robo\Task\Drush\loadTasks;

    // if you use ~1.0 for Robo ~0.4
    use \Boedah\Robo\Task\Drush;
    
    //...
}
```

## Examples

### Site update

This executes pending database updates and reverts all features (from code to database):

```php
$this->taskDrushStack()
    ->drupalRootDirectory('/var/www/html/some-site')
    ->uri('sub.example.com')
    ->maintenanceOn()
    ->updateDb()
    ->revertAllFeatures()
    ->maintenanceOff()
    ->run();
```

### Site install

```php
$this->taskDrushStack()
  ->siteName('Site Name')
  ->siteMail('site-mail@example.com')
  ->locale('de')
  ->accountMail('mail@example.com')
  ->accountName('admin')
  ->accountPass('pw')
  ->dbPrefix('drupal_')
  ->sqliteDbUrl('sites/default/.ht.sqlite')
  ->disableUpdateStatusModule()
  ->siteInstall('minimal')
  ->run();
```
