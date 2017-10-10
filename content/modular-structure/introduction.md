# Introduction

- [About Modular world](#about-modular-world)
- [Quick Example](#quick-example)
- [Laravel-Modules Package](#laravel-modules-package)
    * [Installation and setup](#installation-and-setup)
    * [Customizing](#installation-and-setup)
        * [Register ModuleServiceProvider](#register-ModuleServiceProvider)
        * [Overwrite default stubs](#overwrite-default-stubs)

## About Modular World 

Just imagine having a medium sized application where everything is in the `app/` folder, worse, every model is in the root of the app folder! At some point you will spend a lot of time looking for things because everything is bunched together.

This is what being modular is trying to resolve. You split of the business logic into different parts, which belongs together. If you're into Domain Driven Design, you can consider a module an aggregate.

Every module has its own `routes/controllers/models/views/business logic/etc.` Meaning every module contains a group of classes that all are related to each other in some way.

## Quick Example

Consider an out patient module. To keep it simple.

``` markdown
app/
bootstrap/
.
.
MobileDoctors  
    ├── Patient  
        ├── config  
        |   └── config.php  
        ├── console  
        ├── database  
        │   ├── factories  
        │   ├── migrations  
        │   └── seeders  
        │       └── PatientDatabaseSeeder.php  
        ├── resources  
        │   ├── assets  
        │   ├── lang  
        │   └── views  
        │       ├── layouts  
        │       │   └── master.blade.php  
        │       └── index.blade.php  
        ├── routes  
        │   ├── api.php  
        │   ├── channels.php  
        │   ├── console.php  
        │   └── web.php  
        ├── src  
        │   ├── Emails  
        │   ├── Events  
        │   ├── Http  
        │   │   ├── Controllers  
        │   │   │   └── PatientController.php  
        │   │   ├── Middleware  
        │   │   └── Requests  
        │   ├── Jobs  
        │   ├── Listeners  
        │   ├── Models  
        │   │   ├── Enums  
        │   │   ├── Presenters  
        │   │   ├── Scopes  
        │   │   ├── Traits  
        │   │   └── Transformers  
        │   ├── Notifications  
        │   ├── Policies  
        │   ├── Providers  
        │   │   └── PatientServiceProvider.php  
        │   ├── Repositories  
        │   └── Rules  
        ├── tests  
        ├── composer.json  
        ├── module.json  
        └── start.php
```

## Laravel-Modules Package

This package give us the ability to have custom namespaces for views, config, and languages. Handling migrations/seeds per module. Assets management per module. Helper convenience methods. And so much more.

### Installation and setup

To install through `Composer`, by run the following command:

``` bash
composer require nwidart/laravel-modules
```

The package will automatically register a service provider and alias.

Optionally, publish the package's configuration file by running:

``` bash
php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"
```

`Autoloading`
By default the module classes are not loaded automatically. You can autoload your modules using `psr-4`. For example :

``` json
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/"
    }
  }
}
```

**Tip: don't forget to run `composer dump-autoload` afterwards**

### Customizing

In order to make modules follow the same app structure adopted with the latest version of Laravel, ensuring that modules feel like a natural part of our application and make it looks more readable. We had to make some changes and overriding a little of original package files the way won't affect or make any conflicts with package support & upgrades.

### Register `ModuleServiceProvider`

Instead of letting each module to register its views, config, and languages. Handling migrations/seeds. Decided to move that logic into one provider to handle all. So  I created a `ModuleServiceProvider` in `App` namespace with this content in boot method:

``` php
public function boot()
{
    $modules = $this->app['modules']->enabled();
        
    foreach ($modules as $module)
    {
        $this->registerAutoload($module);
        $this->registerRoutes($module);
        $this->registerConfig($module);
        $this->registerMigrations($module);
        $this->registerViews($module);
        $this->registerTranslations($module);
        $this->registerFactories($module);
    }
}
```

`registerAutoload` to register module `autoloaders` for enabling classes to be automatically loaded if they are currently not defined.

``` php
protected function registerAutoload(Module $module)
{
    $loader = new ClassLoader();

    $loader->addPsr4($this->getNameSpace($module->getComposerAttr('name')), $module->getPath().'/src');
    $loader->register();
    $loader->setUseIncludePath(true);
}
```

### Overwrite default stubs

- Empty `json.stub` providers array
- Remove `autoloads` section on `composer.json`