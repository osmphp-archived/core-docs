# Applications

In this section, you'll learn everything about applications. As you have already learned in the [Main Concepts](main-concepts.html) section, an application is something you can run in the command line or on the Web.  

{{ toc }}

## Application Classes

Out of the box application classes make up an inheritance hierarchy:

    Osm\Core\App            # base class for all application classes
        My\App              # your production app
            My\Samples\App  # a sample app that you run in unit tests

Applications don't tell what modules they include. Vice versa, modules define what application they are for. However, the application class inheritance affects module registration. If some module registers itself with the base class `Osm\Core\App`, it's automatically registered with its derived classes, `My\App` and `My\Samples\App`.    

For example, the `My\FirstModule\Module` registers itself with `My\App`, and due to inheritance, it also loaded as a part of `My\Samples\App`. The `My\Tests\test_01_first_module::test_that_module_is_loaded()` demonstrates that.

## Compiling Application

In context of the Osm Core library, compilation is a process of bundling of application modules together into a codebase that operates as a monolith. Basically, compilation does two things: 

1. As you will learn in the [Dynamic Traits](dynamic-traits.html) section, modules can add custom properties and methods, or override existing ones, in the classes of other modules, and, in order to make it possible, some code have to be generated.

2. Searching for modules and reflecting through class hierarchies takes time, and in order to eliminate this overhead from the application footprint, all this information is collected during compilation phase, and serialized into a file. 

Compile the application **after any code change** using the following command-line alias:

    osmc My\App

Alternatively, you can compile the application in PHP code, using the `Apps` class:

    Apps::$project_path = dirname(__DIR__);
    Apps::compile(\My\App::class);

## Running Application 

### In The Command Line

1. Create an entry point file, or use a sample one, `bin/test_show_modules.php`. Either way, the entry point file should invoke `Apps::run()` method and provide the application logic in the callback:

        <?php
        
        declare(strict_types=1);
        
        use My\App;
        use Osm\Runtime\Apps;
        
        require 'vendor/autoload.php';
        
        Apps::$project_path = dirname(__DIR__);
        Apps::run(Apps::create(App::class), function (App $app) {
            // logic goes here
        });

2. Run it from the command line:

        php bin/test_show_modules.php

### From Code

Run the application by calling `Apps::run()` method:

    Apps::run(Apps::create(App::class), function (App $app) {
        // logic goes here
    });

### From Another Application

Uou can run an application from the already running application as follows:

Run the application by calling `Apps::run()` method:

    Apps::run(Apps::create(App1::class), function (App1 $app) {
        // logic of App1

        // run the App2
        Apps::run(Apps::create(App2::class), function (App2 $app) {
            // logic of App2
        });
    });

## Application Initialization

Before running the application, `Apps::run()` initializes it:

1. The generated code (remember modules that add custom code to the classes of other modules?) is loaded.
2. The application object containing all the information about modules and PHP classes is un-serialized. 
3. The global `$osm_app` variable is initialized with a reference to the application object. 

## Global `osm_app` Variable

Yes, you have read it correctly. The Osm Core library maintains a global variable, `$osm_app`. While normally using global variables use an eyebrow, this decision was made after weighting the alternatives.

`$osm_app` serves as a service container. Compared to the main alternative - dependency injection - the code written using service container pattern is a lot easier to read and maintain. 

In order to prevent occasional variable assignment, this variable could have been hidden behind a function `get_app()`. Actually, it was in the initial library design, but while optimizing performance of one read-world application, we have found that using variable directly reduces the application footprint by 100ms compared to using the `get_app()` function, and the `get_app()` function was removed. 

