# Add a Database

## Configure
You can add a database to your app by simply adding a data component to your `boxfile.yml`:

<div class="meta" data-class="snippet" data-optional-components="mysql,postgres" ></div>

In the above snippet `db` is the unique identifier of this component. It can be anything you choose as long as it is unique.

Nanobox generates the following environment variables based off that name:

* `DATA_DB_HOST` : auto-generated unique host ip
* `DATA_DB_USER` : user to connect with
* `DATA_DB_PASS` : unique password

For databases that require a name, the name will always be `gonano`.

**HEADS UP**: Your database will be provisioned the next time you `nanobox run`.

## Connect

### Include the DB Driver Extension
Add the required extension to the list of extensions in your `boxfile.yml`.

```yaml
run.config:
  engine.config:
    extensions:
      - pdo_mysql
```

### Configure Phalcon

Phalcon stores its database connection credentials in a `app/config/config.php` file for database configuration. Update this file to use the appropriate adapter and auto-generated environment variables.

<div class="meta" data-class="configFile" data-run="app/config/config.php"></div>

```php
<?php
defined('BASE_PATH') || define('BASE_PATH', getenv('BASE_PATH') ?: realpath(dirname(__FILE__) . '/../..'));
defined('APP_PATH') || define('APP_PATH', BASE_PATH . '/app');

return new \Phalcon\Config([
    'database' => [
        'adapter'     => 'Mysql',
        'host'        => $_ENV['DATA_DB_HOST'],
        'username'    => $_ENV['DATA_DB_USER'],
        'password'    => $_ENV['DATA_DB_PASS'],
        'dbname'      => 'gonano',
        'charset'     => 'utf8',
    ],
    'application' => [
        'appDir'         => APP_PATH . '/',
        'controllersDir' => APP_PATH . '/controllers/',
        'modelsDir'      => APP_PATH . '/models/',
        'migrationsDir'  => APP_PATH . '/migrations/',
        'viewsDir'       => APP_PATH . '/views/',
        'pluginsDir'     => APP_PATH . '/plugins/',
        'libraryDir'     => APP_PATH . '/library/',
        'cacheDir'       => BASE_PATH . '/cache/',

        // This allows the baseUri to be understand project paths that are not in the root directory
        // of the webpspace.  This will break if the public/index.php entry point is moved or
        // possibly if the web server rewrite rules are changed. This can also be set to a static path.
        'baseUri'        => preg_replace('/public([\/\\\\])index.php$/', '', $_SERVER["PHP_SELF"]),
    ]
]);

```

## Test

#### From an external client
You can connect directly to your database from an <a href="https://docs.nanobox.io/data-management/managing-local-data/" target="\_blank">external client</a>.

#### From Phalcon
You can also test your connection by simply trying to run a migration.

```bash
nanobox run phalcon run-migration
```

## Now what?
Whats next? Think about what else your app might need and hopefully the topics below will help you get started with the next steps of your development!

* [Frontend Javascript](/php/phalcon/frontend-javascript)
* [Local Environment Variables](/php/phalcon/local-evars)
* [Back to Phalcon overview](/php/phalcon)
