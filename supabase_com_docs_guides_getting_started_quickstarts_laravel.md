Getting Started

# Use Supabase with Laravel

## Learn how to create a PHP Laravel project, connect it to your Supabase Postgres database, and configure user authentication.

* * *

1

### Create a Laravel Project

Make sure your PHP and Composer versions are up to date, then use `composer create-project` to scaffold a new Laravel project.

See the [Laravel docs](https://laravel.com/docs/10.x/installation#creating-a-laravel-project) for more details.

Terminal

`
composer create-project laravel/laravel example-app
`

2

### Install the Authentication template

Install [Laravel Breeze](https://laravel.com/docs/10.x/starter-kits#laravel-breeze), a simple implementation of all of Laravel's [authentication features](https://laravel.com/docs/10.x/authentication).

Terminal

`
composer require laravel/breeze --dev
php artisan breeze:install
`

3

### Set up the Postgres connection details

Go to [database.new](https://database.new/) and create a new Supabase project. Save your database password securely.

When your project is up and running, navigate to the [database settings](https://supabase.com/dashboard/project/_/settings/database) to find the URI connection string. Make sure **Use connection pooling** is checked and **Session mode** is selected. Then copy the URI. Replace the password placeholder with your saved database password.

If your network supports IPv6 connections, you can also use the direct connection string. Uncheck **Use connection pooling** and copy the new URI.

.env

`
DB_CONNECTION=pgsql
DB_URL=postgres://postgres.xxxx:password@xxxx.pooler.supabase.com:5432/postgres
`

4

### Change the default schema

By default Laravel uses the `public` schema. We recommend changing this as Supabase exposes the `public` schema as a [data API](https://supabase.com/docs/guides/api).

You can change the schema of your Laravel application by modifying the `search_path` variable `app/config/database.php`.

The schema you specify in `search_path` has to exist on Supabase. You can create a new schema from the [Table Editor](https://supabase.com/dashboard/project/_/editor).

app/config/database.php

`
'pgsql' => [\
    'driver' => 'pgsql',\
    'url' => env('DB_URL'),\
    'host' => env('DB_HOST', '127.0.0.1'),\
    'port' => env('DB_PORT', '5432'),\
    'database' => env('DB_DATABASE', 'laravel'),\
    'username' => env('DB_USERNAME', 'root'),\
    'password' => env('DB_PASSWORD', ''),\
    'charset' => env('DB_CHARSET', 'utf8'),\
    'prefix' => '',\
    'prefix_indexes' => true,\
    'search_path' => 'laravel',\
    'sslmode' => 'prefer',\
],
`

5

### Run the database migrations

Laravel ships with database migration files that set up the required tables for Laravel Authentication and User Management.

Note: Laravel does not use Supabase Auth but rather implements its own authentication system!

Terminal

`
php artisan migrate
`

6

### Start the app

Run the development server. Go to [http://127.0.0.1:8000](http://127.0.0.1:8000/) in a browser to see your application. You can also navigate to [http://127.0.0.1:8000/register](http://127.0.0.1:8000/register) and [http://127.0.0.1:8000/login](http://127.0.0.1:8000/login) to register and log in users.

Terminal

`
php artisan serve
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings