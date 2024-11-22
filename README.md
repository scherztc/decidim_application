# decidim_application

Free Open-Source participatory democracy, citizen participation and open government for cities and organizations

This is the open-source repository for decidim_application, based on [Decidim](https://github.com/decidim/decidim).

## Setting up the application

You will need to do some steps before having the app working properly once you have deployed it:

Note: If you run into bundler problems with openssl, reinstall ruby with
* rbenv: CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1) --with-opt-dir=$(brew --prefix openssl@1.1)" rbenv install 3.2.2 && rbenv rehash
* rvm: rvm reinstall 3.2.2 --with-openssl-dir=`brew --prefix openssl@1.1` --with-opt-dir=`brew --prefix openssl@1.1`

Note: If you run into bundler problems with pg, run the following:
* bundle config build.pg --with-pg-config=/usr/local/Cellar/libpq/17.1/bin/pg_config

1. brew install libpq postgresql 
1. brew services restart postgresql@14
1. createuser -s postgres
1. nvm use 22.1.0 (must be > 18.x)
1. yarn install
1. DISABLE_SPRING=1 bin/rails assets:precompile
1. DISABLE_SPRING=1 bin/rails db:create
1. DISABLE_SPRING=1 bin/rails db:migrate
1. DISABLE_SPRING=1 bin/rails db:seed
1. Create a System Admin user: `bin/rails decidim_system:create_admin`
1. bin/rails decidim_system:create_admin
1. Start the app with `bin/dev`
1. Visit `http://localhost:3000/system` and log in with your system admin credentials
1. Create a new organization. Check the locales you want to use for that organization, and select a default locale.
1. Set the correct default host for the organization, otherwise the app will not work properly. Note that you need to include any subdomain you might be using.
1. Fill the rest of the form and submit it.

You are good to go!

## Getting the application decidim decidim_application to work on MacBook Pro.

Here are some of the problem areas for using decidim in your Mac environment.

1.  ERROR : charlock_holmes and icu4c
   1. Could not find decidim-dev-0.28.4, charlock_holmes-0.7.7, web-push-3.0.0 in locally installed gems
      1. brew install icu4c@74 
      1. gem install charlock_holmes -v 0.7.7 -- --with-icu-dir=$(brew --prefix icu4c@74).
      1. Make sure you do this in the folder that you are running the decidim decidim_application
      1. You might have to wash the libraries by uninstalling completely first.
   1. Links
      1. https://stackoverflow.com/questions/23883857/cant-bundler-install
      1. https://stackoverflow.com/questions/78059927/cant-bundle-install-with-charlock-holmes
1.  ERROR : Carrierwave Initializer NoMethod Found. 
      1. gem install carrierwave
   1. Links
      1. https://github.com/carrierwaveuploader/carrierwave

### Configuring the database and running migrations

1.  Make sure you have postgreSQL@14 installed correctly by running psql
    1. brew services restart postgresql@14
    1. psql -U postgres
    1. If you have to make conf changes : /usr/local/var/postgresql@14/postgresql.conf

1.  Our Macbooks do not like something about running multithreaded Spring commands
    1. DISABLE_SPRING=1 bin/rails db:create
    1. DISABLE_SPRING=1 bin/rails db:migrate
    1. DISABLE_SPRING=1 bin/rails assets:precompile
    1. DISABLE_SPRING=1 bin/rails db:seed

## Links

1. https://decidim.org/features/

