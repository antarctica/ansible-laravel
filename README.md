# Laravel (`laravel`)

**Part of the BAS Ansible Role Collection (BARC)**

Configures the Laravel framework

## Overview

This role serves more than one purpose, controlled by including task files from this role in a playbook.

**Note:** Including this role in the `roles` key of a playbook will call `tasks/main` automatically.

### Including `tasks/main` configures `bootstrap/start.php`

* Populates the Laravel environments array using `laravel_bootstrap_environments`, this sets the Laravel environment based on the domain the app is accessed from.
* If enabled and an app user is used an alias for "php artisan" is added to the app users bash_aliases, this is enabled by default.

### Including `tasks/environment` creates a Laravel environment file

* Creates a Laravel [environment file](http://laravel.com/docs/4.2/configuration#protecting-sensitive-configuration) which contains key/value's that Laravel will expose as environment variables. These can be read in other config files (i.e. `app/config`) as needed.
	* These files are ideal for protecting passwords or exposing ansible variables (e.g. database location). This task configures one environment at a time, typically it will be called multiple times (production / development).
    * A `laravel_env_user` variable **must** be declared when using this task file. Its `name` property **must** match an environment set in `bootstrap/start.php`.
    * Any values set in `laravel_env_defaults` will be automatically applied to all environment files. As all variables are namespaced there is no danger of clobbering variables, however you must ensure `app/config` files reference the correct environment variables.

## Author

[British Antarctic Survey](http://www.antarctica.ac.uk) - Web & Applications Team

Contact: [basweb@bas.ac.uk](mailto:basweb@bas.ac.uk).

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Branches

This project uses three permanent branches with the *Git Flow* branching model managing the interaction between branches.

* **Develop:** unstable, potentially non-working but most current version of roles. Bug fixes and features interact with this branch only.
* **Master:** stable, tested, working version of role with full documentation. Releases and hot fixes mainly interact with this branch. This branch should when consuming roles internally.
* **Public:** equivalent to the *master* branch, but available externally. Some configuration details may be altered or features removed to make available for public release.

## Testing

Manual testing is performed for all roles to ensure roles achieve their aims and this forms a prerequisite task for merging changes into the *master* and *public* branches.
Wherever possible testing is as complete as possible meaning tasks such as downloading dependencies are performed as part of each test.

## Issues

Please log issues to the [BAS Web and Applications Team](https://jira.ceh.ac.uk/browse/BASWEB) project in Jira, within the *Project - Ansible Roles* component.

If outside of NERC please get in touch to report any issues.

## Contributions

We have no formal contribution policy, if you spot any bugs or potential improvements please submit a pull request or get in touch.

These roles are used for internal projects which may dictate whether any contributions can be included.

## License

[Open Government Licence V2](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/)

## Requirements

### BAS Ansible Role Collection (BARC)

* `composer`

### Other

#### Laravel

Laravel is required to use this role, Composer is used to create a Laravel application.

**Note:** You only need to install Laravel once, if you are not creating a new project this will have already been done (i.e. if Laravel is already mentioned in `composer.json` Laravel has already been installed).

To create a new project `foo` in a `projects` directory:

	$ cd projects
    $ composer create-project laravel/laravel foo --prefer-dist

Add `/composer.lock` and `/vendor/` to the project `.gitignore`.

Include these steps in your project README (usually in a 'getting started' section or similar):

**Note:** Run these within the project VM, not your local machine

    $ composer install
    $ php artisan key:generate

## Variables

* `laravel_app_user_username`
    * The username of the app user, used for day to day tasks, if enabled
    * This variable must be a valid unix username
    * Default: "app"
* `laravel_app_user_add_artisan_bash_aliases`
    * Whether or not to add an alias for 'php artisan' to the app users bash_aliases file, if used/enabled
    * Default: "true"
* `laravel_app_root`
    * Path to the directory holding the laravel app (i.e. containing composer.lock)
    * This variable **must** point to a directory, it **must not** include a trailing `/`.
    * Default: "/app"
* `laravel_config_bootstrap_environments`
    * Array of Laravel environment - hostname mappings
    * With each item consisting of following key/values
        * `name` A project supported Laravel environment
        * `host` Valid host or domain name
    * Default: []  (empty array)
* `laravel_env_defaults`
	* Default environment name and options for all environment files
	* Default: (array)
		* name = "production"
		* options = (array)
			* name = "debug"
			* type = "raw"
			* value = "false"
* `laravel_env_user`
    * Options for a specific environment file
    * This variable **must** be declared if a task relies on it as no default is provided and a fatal error will occur.
    * The `name` property **must** match an environment set in `bootstrap/start.php`.
    * Default: (none - not defined)

The format of `laravel_env_defaults` and `laravel_env_user` is as follows:

    laravel_env_<something>
      name: <environment name>
      options:
        - key: <option key>
        - type: <option value type>
        - value: <option value>

* `laravel_env_<something>`
	* Either `laravel_env_defaults` or `laravel_env_user`
* `name: <environment name>`
    * Name of Laravel environment (as defined by `laravel_config_bootstrap_environments`)
    * E.g. `development`
* `options`
    * An array of options, with each option consisting of the following key/values:
        * `key: <option key>`
	        * Name of environment variable (i.e. for `$ENV['sparkle']` name = "sparkle")
        * `type: <option value type`
	        * Data type for `value`
	        * Use `string` for string values, use `raw` for anything else
        * `value: <option value>` value for option
	        * The value for the environment variable

## Changelog


### 0.6.0 - December 2014

* An alias for 'php artisan' is registered for the app user, if used

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
