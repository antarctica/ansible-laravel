# Laravel (`laravel`)

**Part of the BAS Ansible Role Collection (BARC)**

Configures the Laravel framework

## Overview

This role serves more than one purpose, controlled by including task files from this role in a playbook.

**Note:** Including this role in the `roles` key of a playbook will call `tasks/main` automatically.

### Including `tasks/main` configures `bootstrap/start.php`

* Populates the Laravel environments array using `laravel_bootstrap_environments`, this sets the Laravel environment based on the domain the app is accessed from.
* If enabled and an app user is used, an alias for "php artisan" is added to the app users bash_aliases, this is enabled by default.
* If used the cron job required by the cron driver of the Indatus/dispatcher dispatcher package is added to the app users cron job, if used.

### Including `tasks/environment` creates a Laravel environment file

* Creates a Laravel [environment file](http://laravel.com/docs/4.2/configuration#protecting-sensitive-configuration) which contains key/value's that Laravel will expose as environment variables. These can be read in other config files (i.e. `app/config`) as needed.
	* These files are ideal for protecting passwords or exposing ansible variables (e.g. database location). This task configures one environment at a time, typically it will be called multiple times (production / development).
    * A `laravel_env_user` variable **must** be declared when using this task file. Its `name` property **must** match an environment set in `bootstrap/start.php`.
    * Any values set in `laravel_env_defaults` will be automatically applied to all environment files. As all variables are namespaced there is no danger of clobbering variables, however you must ensure `app/config` files reference the correct environment variables.

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `composer`

#### Other

##### Laravel

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

### Variables

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
    * This variable **must** be declared if a task relies on it as no default is provided and a fatal error will occur
    * The `name` property **must** match an environment set in `bootstrap/start.php`
    * Default: (none - not defined)
* `laravel_app_db_name`
    * Provides a neutral variable for the default database (or equivalent) regardless of which database (or equivalent) system is used
    * This variable **must** match the name of an available database (or equivalent) in whichever database (or equivalent) system is used
    * Default: "app"


#### Laravel environments

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

## Contributing

This project welcomes contributions, see `CONTRIBUTING` for our general policy.

## Developing

### Committing changes

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).

## License

Copyright 2015 NERC BAS. Licensed under the MIT license, see `LICENSE` for details.
