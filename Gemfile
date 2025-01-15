# frozen_string_literal: true

source "https://rubygems.org"

ruby RUBY_VERSION

gem "bootsnap", "~> 1.3"
gem "decidim", "0.29.0"
# gem "decidim-conferences", "0.29.0"
gem "decidim-design", "0.29.0"
# gem "decidim-initiatives", "0.29.0"
# gem "decidim-templates", "0.29.0"

# Setup cron jobs
gem "whenever", require: false

gem "puma", ">= 6.3.1"

gem "wicked_pdf", "~> 2.1"

gem "dotenv-rails" # Loads environment variables from .env files

gem "bcrypt_pbkdf", "~> 1.0" # Required for SSH key authentication (Net::SSH)
gem "ed25519", "~> 1.2" # ED25519 key support (Net::SSH)

group :development, :test do
  gem "brakeman", "~> 6.1"
  gem "byebug", "~> 11.0", platform: :mri
  gem "capistrano", "~> 3.19" # Core Capistrano
  gem "capistrano-bundler", "~> 2.0" # Manages Bundler
  gem "capistrano-rails", "~> 1.6" # Rails tasks (assets, migrations)
  gem "capistrano-ssh-doctor", "~> 1.0.0" # Debug SSH issues with Capistrano
  gem "decidim-dev", "0.29.0"
  gem "factory_bot_rails"
  gem "net-imap", "~> 0.2.3"
  gem "net-pop", "~> 0.1.1"
  gem "net-smtp", "~> 0.3.1"
end

group :development do
  gem "letter_opener_web", "~> 2.0"
  gem "listen", "~> 3.1"
  gem "spring"
  gem "web-console", "~> 4.2"
end

group :test do
  gem "capybara"
  gem "coveralls", require: false
  gem "database_cleaner-active_record"
  gem "rspec-rails", "~> 6.0.0"
  gem "simplecov", require: false
  gem "simplecov-lcov", require: false
end
