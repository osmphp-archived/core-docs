# Creating Command Line Shortcuts

{{ toc }}

## In Windows

1. Create a `bin` directory in `C:\Users\{your_user}`.
   
2. Add a `%USERPROFILE%\bin` directory to the `PATH` environment variable in `Control Panel -> System -> Advanced system settings -> Environment variables -> System variables`.

3. In `C:\Users\{your_user}\bin` directory, create these files with the following contents:

    `osmc.bat`:
   
        @php vendor/osmphp/core/bin/compile.php %*

   `osmh.bat`:

        @php vendor/osmphp/core/bin/hint.php %*

## In Linux

Add the following lines to the `~/.bashrc` file:

    alias osmc='php vendor/osmphp/core/bin/compile.php'
    alias osmh='php vendor/osmphp/core/bin/hint.php'