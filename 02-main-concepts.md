# Main Concepts

In this section, you'll learn the main concepts used in Osm Core.

Short version:

* Your project may define and run one or more **applications**. Simply put, an application is something you can run in the command line, something that can render a Web page and handles user actions, or both. There are the main application, sample applications, and utility applications.
* An application is made of **modules**. A module is a directory containing PHP classes that implement some specific feature. Some modules are application-specific, and others are suitable for any application.
* Modules are organized in groups. A **module group** is a directory with module directories inside.
* Modules, module groups and applications are all parts of **Composer packages**, either those in `vendor` directory, or the root package - the project itself.

Long version:

{{ toc }}

## Typical Project Directory

Project's directory structure has already been discussed in the [Getting Started](getting-started.html#directory-structure) section. 

Let's return to it one more time, this time with a more meaningful example. The application is an e-commerce store, and the modules implements different parts of it:

    composer.json           # maps `src/` directory to `My\` namespace
    src/
        ModuleGroup.php     # tells "some modules are here in subdirectories"
        App.php             # e-commerce application
        Base/
            Module.php      # tells "this is a module, belonging to the app `\My\App`
                            # and depending on a list of external modules"
            ...             # (optional) other classes of the module 
        Products/
            Module.php      # tells "this is a module, belonging to the app `\My\App`
                            # and depending on module `\My\Base\Module`"
            ...             # other classes of the module handling store product data 
        Cart/
            Module.php      # tells "this is a module, belonging to the app `\My\App`
                            # and depending on module `\My\Base\Module`"
            ...             # other classes of the module handling the store cart
    bin/
        my_app.php          # the entry point file that runs the `\My\App` in 
                            # the command line
    public/
        index.php           # the entry point file that runs the `My\App` on the Web

## Applications

In Osm Core, the word "application" (or "app") is slightly different from how we usually use it. A typical application - be it Facebook or Word - is a bunch of files that you can "run" as one thing. When using Osm core, the bunch of files - **the project - may contain many applications** that you can run, and these applications are quite different. 

First, there is the main application of the project that implements main functionality. For example, in an e-commerce project, the main application handles product catalog, cart, and checkout.

Second, there are sample applications that are used internally in unit tests to test small parts of the main application. For example, there is a product catalog sample application, a cart sample application, and a checkout sample application.  

Finally, there are utility applications that you use for developing and maintaining the project.

From code perspective an application is defined as a PHP class:

    namespace My;

    class App extends \Osm\Core\App {
    }

## Modules

Applications are combined from modules. It's like a puzzle. If you combine one set of modules, you'll get an e-commerce application, if you use another set of modules - you'll get a blog application.

Some modules are application-specific, while other modules are generic. For example a product catalog is specific for an e-commerce application, at the same time a module responsible for internationalization is generic and can be used in many applications.

In code, a module is a directory containing a `Module.php` file that defines a module PHP class:

    namespace My\Invoices;

    class Module extends \Osm\Core\Module {
    }

## Composer Packages

Osm Core is installed using [Composer](https://getcomposer.org/). In Composer the project defines the Composer packages it uses, and these packages are downloaded into the `vendor` directory. The project is considered to be a package, too (it's named the "root" package).

Every package has a `composer.json` file, and this file specifies in its `autoload` and `autoload-dev` sections where the PHP classes are, and what namespaces they have. For example, `osmphp/core` package stores PHP classes in to directories in `runtime` and `src` directories:

    "autoload": {
        ...
        "psr-4": {
            "Osm\\Runtime\\": "runtime/",
            "Osm\\Core\\": "src/"
        }
    },

## Module Groups

Application search for its modules in all the directories marked as "module groups" and listed in `autoload.psr-4` and `autoload-dev.psr-4` sections of every installed package, including the root package.

Mark a source directory as module group by adding a `ModuleGroup.php` file that defines a module group PHP class:

    namespace My;

    class ModuleGroup extends \Osm\Core\ModuleGroup {
    }

