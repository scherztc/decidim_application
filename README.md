# Decidim Application Setup Guide

Free Open-Source participatory democracy, citizen participation, and open government for cities and organizations.

This is the open-source repository for `decidim_application`, based on [Decidim](https://github.com/decidim/decidim).

---

## Prerequisites

- **Homebrew** installed on your Mac.
- **Ruby** and **Rails** set up for Decidim.
- **Node.js** version 18.x or higher (we recommend using Node.js 22.x).
- **Decidim** project cloned on your machine.

---

## Setting Up the Application on macOS

Follow these steps to set up Decidim on your MacBook. These instructions are tailored for macOS users with Homebrew and include troubleshooting tips for common issues.

### 1. Install Dependencies

#### 1.1 Install PostgreSQL and libpq

```bash
brew install postgresql@14 libpq
```
#### 1.2 Start PostgreSQL Service
```bash
brew services start postgresql@14
```
#### 1.3 Verify PostgreSQL is Running
```bash
brew services list
```
You should see a list of services with postgresql@14 marked as started. It should look something like:
```sql
Name              Status  User       Plist
postgresql@14     started your_user  /Users/your_user/Library/LaunchAgents/homebrew.mxcl.postgresql@14.plist
```
If you see an error status next to postgresql@14:

- Check the logs for more details:
```bash
less /usr/local/var/log/postgresql@14.log
```
- Ensure the PostgreSQL data directory is initialized (see Step 2).
---
### 2. Initialize PostgreSQL (If Not Already Initialized)
If this is your first time installing PostgreSQL, you need to initialize the database directory:
```bash
initdb /usr/local/var/postgresql@14
```
Note: If you receive a "permission denied" error or "cannot be run as root", make sure you're not running the command with sudo and that you have the correct permissions:
```bash
mkdir -p /usr/local/var/postgresql@14
sudo chown -R $(whoami) /usr/local/var/postgresql@14
initdb /usr/local/var/postgresql@14
```
Restart the PostgreSQL service:
```bash
brew services restart postgresql@14
```
---
### 3. Set Up PostgreSQL Users and Database
#### 3.1 Access PostgreSQL
Try logging into PostgreSQL as the default superuser:
```bash
psql -U postgres
```
- If prompted for a password and you haven't set one, just press Enter.
- If you cannot log in or receive an authentication error, you may need to create the postgres superuser.
#### 3.1.1 Create the postgres Superuser (If Necessary)
If you cannot log in, you can start PostgreSQL in single-user mode to create the postgres user:

1. Stop the PostgreSQL service:
```bash
brew services stop postgresql@14
```
2. Start PostgreSQL in single-user mode:
```bash
postgres --single -D /usr/local/var/postgresql@14 postgres
```
3. At the prompt, create the postgres user with a password (replace password with your desired password):
```sql
CREATE USER postgres WITH SUPERUSER PASSWORD 'password';
```
4. Exit single-user mode:
   Press `Control + D`.
5. Restart PostgreSQL:
```bash
brew services start postgresql@14
```
6. Try logging in again:
```bash
psql -U postgres
```
- When prompted for a password, enter the password you set.
---
### 4. Install Ruby Dependencies
#### 4.1 Fix OpenSSL Issues (If Needed)
If you run into bundler problems with OpenSSL, reinstall Ruby with the correct OpenSSL configuration:

- For rbenv:
```bash
CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1) --with-opt-dir=$(brew --prefix openssl@1.1)" rbenv install 3.2.2 && rbenv rehash
```
- For rvm:
```bash
rvm reinstall 3.2.2 --with-openssl-dir=$(brew --prefix openssl@1.1) --with-opt-dir=$(brew --prefix openssl@1.1)
```
#### 4.2 Configure Bundler for pg Gem (If Needed)
If you run into bundler problems with the pg gem, run:
```bash
bundle config build.pg --with-pg-config=$(brew --prefix libpq)/bin/pg_config
```
Ensure that libpq is linked:
```bash
brew link --force libpq
```
---
### Install Project Dependencies
#### 5.1 Install Node.js
Ensure you have Node.js version 18.x or higher. We recommend using Node.js 22.x. 
You can use nvm to manage Node.js versions:
```bash
nvm install 22.1.0
nvm use 22.1.0
```
#### 5.2 Install Gems
In your project directory, run:
```bash
bundle install
```
#### 5.3 Install Yarn Packages
```bash
yarn install
```
---
### 6. Configure Environment Variables
In your Decidim project directory, make sure you have a .env.development file with the following content:
```bash
DATABASE_HOST=localhost
DATABASE_USERNAME=decidim_app
DATABASE_PASSWORD=thepassword
```
Ensure your config/database.yml is set up to use these environment variables.
---
### 7. Troubleshooting Common Issues
#### 7.1 Error: charlock_holmes and icu4c
If you encounter an error like:
```arduino
Could not find charlock_holmes-0.7.7 in locally installed gems
```
Solution:

1. Install the specific version of icu4c:
```bash
brew install icu4c@74
```
2. Install the charlock_holmes gem with the correct icu4c path:
```bash
gem install charlock_holmes -v '0.7.7' -- --with-icu-dir=$(brew --prefix icu4c@74)
```
3. Ensure you are in your project directory when running the commands.
4. You might need to uninstall existing versions before reinstalling.
#### Helpful Links:
- [Stack Overflow: Can't bundle install with charlock_holmes](https://stackoverflow.com/questions/78059927/cant-bundle-install-with-charlock-holmes)
#### 7.2 Error: CarrierWave Initializer NoMethodError
If you encounter an error related to CarrierWave:

Solution:
Install the carrierwave gem:
```bash
gem install carrierwave
```
#### Helpful Links:
- [CarrierWave GitHub Repository](https://github.com/carrierwaveuploader/carrierwave)
---
### 8. Configure the Database and Run Migrations
Due to issues with multithreaded Spring commands on MacBooks, prepend `DISABLE_SPRING=1` to each command.

Run the following commands in your terminal, inside the root directory of your Decidim project:
```bash
DISABLE_SPRING=1 bin/rails db:create
DISABLE_SPRING=1 bin/rails db:migrate
DISABLE_SPRING=1 bin/rails assets:precompile
DISABLE_SPRING=1 bin/rails db:seed
```
---
### 9. Create a System Admin User
```bash
bin/rails decidim_system:create_admin
```
Follow the prompts to create your system admin user.
---
### 10. Start the Application
You can now start your application:
```bash
rails server
```
Note: You do not need to prepend DISABLE_SPRING=1 when starting the server.
---
### Additional Links
- [Decidim Features](https://decidim.org/features/)
