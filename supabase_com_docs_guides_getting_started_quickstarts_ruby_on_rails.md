Getting Started

# Use Supabase with Ruby on Rails

## Learn how to create a Rails project and connect it to your Supabase Postgres database.

* * *

1

### Create a Rails Project

Make sure your Ruby and Rails versions are up to date, then use `rails new` to scaffold a new Rails project. Use the `-d=postgresql` flag to set it up for Postgres.

Go to the [Rails docs](https://guides.rubyonrails.org/getting_started.html) for more details.

Terminal

`
rails new blog -d=postgresql
`

2

### Set up the Postgres connection details

Go to [database.new](https://database.new/) and create a new Supabase project. Save your database password securely.

When your project is up and running, navigate to the [database settings](https://supabase.com/dashboard/project/_/settings/database) to find the URI connection string. Make sure **Use connection pooling** is checked and **Session mode** is selected. Then copy the URI. Replace the password placeholder with your saved database password.

If your network supports IPv6 connections, you can also use the direct connection string. Uncheck **Use connection pooling** and copy the new URI.

Terminal

`
export DATABASE_URL=postgres://postgres.xxxx:password@xxxx.pooler.supabase.com:5432/postgres
`

3

### Create and run a database migration

Rails includes Active Record as the ORM as well as database migration tooling which generates the SQL migration files for you.

Create an example `Article` model and generate the migration files.

Terminal

`
bin/rails generate model Article title:string body:text
bin/rails db:migrate
`

4

### Use the Model to interact with the database

You can use the included Rails console to interact with the database. For example, you can create new entries or list all entries in a Model's table.

Terminal

`
bin/rails console
`

irb

`
article = Article.new(title: "Hello Rails", body: "I am on Rails!")
article.save # Saves the entry to the database
Article.all
`

5

### Start the app

Run the development server. Go to [http://127.0.0.1:3000](http://127.0.0.1:3000/) in a browser to see your application running.

Terminal

`
bin/rails server
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings