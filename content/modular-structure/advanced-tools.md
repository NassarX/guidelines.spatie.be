# Advanced Tools

- [Artisan Commands](#artisan-commands)
    * [Utility Commands](#utility-commands)
    * [Generator Commands](#generator-commands)
- [Facade Methods](#facade-methods)
- [Module Methods](#module-methods)
- [Registering module console commands](#registering-module-console-commands)
- [Registering module events](#registering-module-events)

**`You can use the following commands with the --help suffix to find its arguments and options.`**

## Utility commands

### module:make

Generate a new module.

```bash
php artisan module:make Blog
```

### module:make

Generate multiple modules at once.

```bash
php artisan module:make Blog User Auth
```

### module:use

Use a given module. This allows you to not specify the module name on other commands requiring the module name as an argument.

```bash
php artisan module:use Blog
```

### module:unuse

This unsets the specified module that was set with the `module:use` command.

```bash
php artisan module:unuse
```

### module:list

List all available modules.

```bash
php artisan module:list
```

### module:migrate

Migrate the given module, or without a module an argument, migrate all modules.

```bash
php artisan module:migrate Blog
```

### module:migrate-rollback

Rollback the given module, or without an argument, rollback all modules.

```bash
php artisan module:migrate-rollback Blog
```

### module:migrate-refresh

Refresh the migration for the given module, or without a specified module refresh all modules migrations.

```bash
php artisan module:migrate-refresh Blog
```

### module:migrate-reset Blog

Reset the migration for the given module, or without a specified module reset all modules migrations.

```bash
php artisan module:migrate-reset Blog
```

### module:seed

Seed the given module, or without an argument, seed all modules

```bash
php artisan module:seed Blog
```

### module:publish-migration

Publish the migration files for the given module, or without an argument publish all modules migrations.

```bash
php artisan module:publish-migration Blog
```

### module:publish-config

Publish the given module configuration files, or without an argument publish all modules configuration files.

```bash
php artisan module:publish-config Blog
```

### module:publish-translation

Publish the translation files for the given module, or without a specified module publish all modules migrations.

```bash
php artisan module:publish-translation Blog
```

### module:enable

Enable the given module.

```bash
php artisan module:enable Blog
```

### module:disable

Disable the given module.

```bash
php artisan module:disable Blog
```

### module:update

Update the given module.

```bash
php artisan module:update Blog
```

## Generator commands

### module:make-command

Generate the given console command for the specified module.

```bash
php artisan module:make-command CreatePostCommand Blog
```

### module:make-migration

Generate a migration for specified module.

```bash
php artisan module:make-migration create_posts_table Blog
```

### module:make-seed

Generate the given seed name for the specified module.

```bash
php artisan module:make-seed seed_fake_blog_posts Blog
```

### module:make-controller

Generate a controller for the specified module.

```bash
php artisan module:make-controller PostsController Blog
```

### module:make-model

Generate the given model for the specified module.

```bash
php artisan module:make-model Post Blog
```

Optional options:

- `--fillable=field1,field2`: set the fillable fields on the generated model
- `--migration`, `-m`: create the migration file for the given model

### module:make-provider

Generate the given service provider name for the specified module.

```bash
php artisan module:make-provider BlogServiceProvider Blog
```

### module:make-middleware

Generate the given middleware name for the specified module.

```bash
php artisan module:make-middleware CanReadPostsMiddleware Blog
```

### module:make-mail

Generate the given mail class for the specified module.

```bash
php artisan module:make-mail SendWeeklyPostsEmail Blog
```

### module:make-notification

Generate the given notification class name for the specified module.

```bash
php artisan module:make-notification NotifyAdminOfNewComment Blog
```

### module:make-listener

Generate the given listener for the specified module. Optionally you can specify which event class it should listen to. It also accepts a `--queued` flag allowed queued event listeners.

```bash
php artisan module:make-listener NotifyUsersOfANewPost Blog
php artisan module:make-listener NotifyUsersOfANewPost Blog --event=PostWasCreated
php artisan module:make-listener NotifyUsersOfANewPost Blog --event=PostWasCreated --queued
```

### module:make-request

Generate the given request for the specified module.

```bash
php artisan module:make-request CreatePostRequest Blog
```

### module:make-event

Generate the given event for the specified module.

```bash
php artisan module:make-event BlogPostWasUpdated Blog
```

### module:make-job

Generate the given job for the specified module.

```bash
php artisan module:make-job JobName Blog

php artisan module:make-job JobName Blog --sync # A synchronous job class
```

### module:route-provider

Generate the given route service provider for the specified module.

```bash
php artisan module:route-provider Blog
```

### module:make-factory

Generate the given database factory for the specified module.

```bash
php artisan module:make-factory FactoryName Blog
```

### module:make-policy

Generate the given policy class for the specified module.

The `Policies` is not generated by default when creating a new module. Change the value of `paths.generator.policies` in `modules.php` to your desired location.

```bash
php artisan module:make-policy PolicyName Blog
```

### module:make-rule

Generate the given validation rule class for the specified module.

The `Rules` folder is not generated by default when creating a new module. Change the value of `paths.generator.rules` in `modules.php` to your desired location.

```bash
php artisan module:make-rule ValidationRule Blog
```

### module:make-resource

Generate the given resource class for the specified module. It can have an optional `--collection` argument to generate a resource collection.

The `Transformers` folder is not generated by default when creating a new module. Change the value of `paths.generator.resource` in `modules.php` to your desired location.

```bash
php artisan module:make-resource PostResource Blog
php artisan module:make-resource PostResource Blog --collection
```

### module:make-test

Generate the given test class for the specified module.

```bash
php artisan module:make-test EloquentPostRepositoryTest Blog
```

## Facade Methods

Get all modules.

```php
Module::all();
```

Get all cached modules.

```php
Module::getCached()
```

Get ordered modules. The modules will be ordered by the `priority` key in `module.json` file.

```php
Module::getOrdered();
```

Get scanned modules.

```php
Module::scan();
```

Find a specific module.

```php
Module::find('name');
// OR
Module::get('name');
```

Find a module, if there is one, return the `Module` instance, otherwise throw `Nwidart\Modules\Exeptions\ModuleNotFoundException`.

```php
Module::findOrFail('module-name');
```

Get scanned paths.

```php
Module::getScanPaths();
```

Get all modules as a collection instance.

```php
Module::toCollection();
```

Get modules by the status. 1 for active and 0 for inactive.

```php
Module::getByStatus(1);
```

Check the specified module. If it exists, will return `true`, otherwise `false`.

```php
Module::has('blog');
```

Get all enabled modules.

```php
Module::enabled();
```

Get all disabled modules.

```php
Module::disabled();
```

Get count of all modules.

```php
Module::count();
```

Get module path.

```php
Module::getPath();
```

Register the modules.

```php
Module::register();
```

Boot all available modules.

```php
Module::boot();
```

Get all enabled modules as collection instance.

```php
Module::collections();
```

Get module path from the specified module.

```php
Module::getModulePath('name');
```

Get assets path from the specified module.

```php
Module::assetPath('name');
```

Get config value from this package.

```php
Module::config('composer.vendor');
```

Get used storage path.

```php
Module::getUsedStoragePath();
```

Get used module for cli session.

```php
Module::getUsedNow();
// OR
Module::getUsed();
```

Set used module for cli session.

```php
Module::setUsed('name');
```

Get modules's assets path.

```php
Module::getAssetsPath();
```

Get asset url from specific module.

```php
Module::asset('blog:img/logo.img');
```

Install the specified module by given module name.

```php
Module::install('nwidart/hello');
```

Update dependencies for the specified module.

```php
Module::update('hello');
```

Add a macro to the module repository.

```php
Module::macro('hello', function() {
    echo "I'm a macro";
});
```

Call a macro from the module repository.

```php
Module::hello();
```

Get all required modules of a module

```php
Module::getRequirements('module name');
```

## Module Methods

Get an entity from a specific module.

```php
$module = Module::find('blog');
```

Get module name.

```
$module->getName();
```

Get module name in lowercase.

```
$module->getLowerName();
```

Get module name in studlycase.

```
$module->getStudlyName();
```

Get module path.

```
$module->getPath();
```

Get extra path.

```
$module->getExtraPath('Assets');
```

Disable the specified module.

```
$module->disable();
```

Enable the specified module.

```
$module->enable();
```

Delete the specified module.

```
$module->delete();
```

Get an array of module requirements. Note: these should be aliases of the module.

```
$module->getRequires();
```

## Registering module console commands

Your module may contain console commands. You can generate these commands manually, or with the following helper:

```bash
php artisan module:make-command CreatePostCommand Blog
```

This will create a `CreatePostCommand` inside the Blog module. By default this will be `Modules/Blog/Console/CreatePostCommand`.

Please refer to the [laravel documentation on artisan commands](https://laravel.com/docs/5.5/artisan) to learn all about them.

### Registering the command

You can register the command with the laravel method called `commands` that is available inside a service provider class.

``` php
$this->commands([
    \Modules\Blog\Console\CreatePostCommand::class,
]);
```

You can now access your command via `php artisan` in the console.

## Registering module events

Your module may contain events and event listeners. You can create these classes manually, or with the following helpers:

``` bash
php artisan module:make-event BlogPostWasUpdated Blog
php artisan module:make-listener NotifyAdminOfNewPost Blog
```

Once those are create you need to register them in laravel. This can be done in 2 ways:

- Manually calling `$this->app['events']->listen(BlogPostWasUpdated::class, NotifyAdminOfNewPost::class);` in your module service provider
- Or by creating a event service provider for your module which will contain all its events, similar to the `EventServiceProvider` under the app/ namespace.

### Creating an EventServiceProvider

Once you have multiple events, you might find it easier to have all events and their listeners in a dedicated service provider. This is what the EventServiceProvider is for.

Create a new class called for instance `EventServiceProvider` in the `Modules/Blog/Providers` folder (Blog being an example name).

This class needs to look like this:

``` php
<?php

namespace Modules\Blog\Providers;

use Illuminate\Foundation\Support\Providers\EventServiceProvider as ServiceProvider;

class EventServiceProvider extends ServiceProvider
{
    protected $listen = [
        BlogPostWasUpdated::class => [
            NotifyAdminOfNewPost::class,
        ],
    ];
}

```
**Don't forget to load this service provider, for instance by adding it in the module.json file of your module.**