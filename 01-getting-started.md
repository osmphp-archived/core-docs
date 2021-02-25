# Getting Started

In this section, you'll learn how to create a project, and see the first tangible results.

{{ toc }}

## Prerequisites

The Osm Core library requires:

* PHP 8 with `mbstring` extension
* Composer

Install them if necessary.

Also, configure [command-line aliases in your operating system](tips-and-tricks.html#command-line-aliases).

## Creating A Project

Run in the command line:

    composer create-project osmphp/template hello

This command creates `hello/` directory with a typical directory structure inside it, and it also installs Osm Core library and other relevant packages into the `vendor/` subdirectory.

In real projects, replace `hello` with a meaningful project name. 

## Directory Structure

Let's have a look at the most important files in the `hello/` directory:

    composer.json               # maps `src/` directory to `My\` namespace
    src/
        ModuleGroup.php         # tells "some modules are here in subdirectories"
        App.php                 # your application, `\My\App`
        FirstModule/
            Module.php          # tells "this is my first module, it belongs to the `\My\App`
    bin/
        test_show_modules.php   # an entry point file that runs the `\My\App` in 
                                # the command line and prints out all the modules
                                # loaded with it

## Concepts 

The **application** and **module** concepts are explained in detail in the [Main Concepts](main-concepts.html) section. Short version:

* Your project may define and run one or more **applications**. Simply put, an application is something you can run in the command line, something that can render a Web page and handles user actions, or both. There are the main application, sample applications, and utility applications.
* An application is made of **modules**. A module is a directory containing PHP classes that implement some specific feature. Some modules are application-specific, and others are suitable for any application.

## Compiling And Running The Application

Managing applications is explained in detail in the [Applications](applications.html) section. 

In short, before running an application, you have to compile it:

    osmc My\App

Then, in order to run it, execute its entry point. Your project already have one entry point - the `bin/test_show_modules.php` file, that outputs the list of modules loaded with the application:

    php bin/test_show_modules.php

You should see a single module in the output:

    My application modules:
    
    My\FirstModule\Module

It's because the `FirstModule` explicitly states that it's a part of the `My\App` application:

    # src/FirstModule/Module.php
    class Module extends BaseModule
    {
        public static ?string $app_class_name = \My\App::class;
    }

