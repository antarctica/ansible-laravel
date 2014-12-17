# Laravel (`laravel`) - Changelog

## 0.6.2 - December 2014

* Fixing task load order to prevent assumption of packages being installed
* Adding default app database name variable

## 0.6.1 - December 2014

* Preparing role for public release

### 0.6.0 - December 2014

* An alias for 'php artisan' is registered for the app user, if used
* If used, a cron job for the indatus/dispatcher package is created and enabled

### 0.5.4 - October 2014

* Updating dependencies
* Laravel app directory is now configurable
* Preparing for public release

### 0.5.3 - October 2014

* Fixing environment file template to support user environment variables for production environments

### 0.5.2 - October 2014

* Fixing permissions error with bootstrap/start.php

### 0.5.1 - October 2014

* Fixing permissions error with bootstrap/start.php
* Fixing incorrect dependency (was `php` should have been `composer`)

### 0.5.0 - October 2014

* Refactoring how variables for default and user options are defined. This is still not as clean as it could be and will likely change in the future.

### 0.4.1 - October 2014

* Fixing how variables are set when using the environment task file
* Fixing missing refactoring

### 0.4.0 - October 2014

* Merging `laravel-environment` into this role
* Adjusting role for inclusion in BARC

### 0.3.0 - September 2014

* Fundamentally changing how Laravel app configuration files are managed. Instead of trying to manage files using complex templates and variables which won't handle per-project changes well (or at all in the current role based approach). Having to deal with multiple environments as well (which would be equally difficult), all this seems like a fools errand.
* This role now only handles defining which environments are available within a project, setting variables within each environment is now handled by the `laravel-environment` role.

### 0.2.0 - September 2014

* Adding support for app and database configuration using templates and variables

### 0.1.0 - September 2014

* Initial version
