# Laravel (`laravel`)

**Part of the BAS Ansible Role Collection (BARC)**

Configures the Laravel framework

## Overview

This role serves more than one purpose, controlled by including task files from this role in a playbook. 

**Note:** Including this role in the `roles` key of a playbook will call `tasks/main` automatically.

### Including `tasks/main` configures `bootstrap/start.php`

* Populates the Laravel environments array using `laravel_bootstrap_environments`, this sets the Laravel environment based on the domain the app is accessed from. 

### Including `tasks/environment` creates a Laravel environment file

* Creates a Laravel [environment file](http://laravel.com/docs/4.2/configuration#protecting-sensitive-configuration) which contains key/value's that Laravel will expose as environment variables. These can be read in other config files (i.e. `app/config`) as needed.
	* These files are ideal for protecting passwords or exposing ansible variables (e.g. database location). This task configures one environment at a time, typically it will be called multiple times (production / development).
	* Each environment file is named using `laravel_env_defaults.name` then `laravel_env_user.name`, which should match an environment set in `bootstrap/start.php`.
	* Key/value's for each file are taken first from `laravel_env_defaults.options` then `laravel_env_user.options`.

## Author

[British Antarctic Survey](http://www.antarctica.ac.uk) - Web & Applications Team

Contact: [basweb@bas.ac.uk](mailto:basweb@bas.ac.uk).

## Availability

This role is designed for internal use but if useful can be shared publicly.

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
    * Default: []  (empty)

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
