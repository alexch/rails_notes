# Adding RSpec
Create your app like this:

    rails new sample_app --skip-test-unit

Open `Gemfile` and add:

    group :development do
      gem 'rspec-rails'
    end
    
    group :test do
      gem 'rspec'
      gem 'capybara'
    end

Then, run:

    bundle install
    
And finally:

    rails generate rspec:install

# Query Exercise

Create a new file in your Rails directory tree: `spec/models/user_queries_spec.rb`. Then paste the following content in:

    require 'spec_helper'

    describe User do

      it 'can retrieve a user by email address'

      it 'retrieves all users sorted by name'

      it 'retrieves all users containing the partial name "Smith"'

      it 'retrieves all users containing the partial name "Smith" sorted by email'

    end

# Validation Exercise

* valid?
* changed?
* errors

Create a new file in your Rails directory tree: `spec/models/user_validations_spec.rb`. Then paste the following content in:

    require 'spec_helper'

    describe User do

      it 'is invalid unless name is present'

      it 'the error message says "must be present"'

      it 'when changing an attribute, changed? flag is true'

      it 'it is only valid with both name and email given'

    end

# CRUD Exercise

Create a new file in your Rails directory tree: `spec/models/user_spec.rb`. Then paste the following content in:

    require 'spec_helper'
    describe User do

      it 'can create a user and retrieve it back by ID' do
        u = User.create(:name => "Joe", :email => "joe@example.com")
        User.find(u.id).should == u
      end

      it 'can instantiate a user with new, then save it, and find it back by ID'

      it 'can update an existing user name with "attributes=" and save, then verify it is been written correctly'

      it 'can update an existing user name with "update_attribute" and save, then verify it is been written correctly'

      it 'can count the number of users'

      it 'can destroy a user, then verify it is gone'

    end

# Onto a Real (Real) App

Rails Tutorial Chapter 3 (page 78ff)

     rails new sample_app -T

-T causes the rails generator to skip the default test unit directories. We'll be using RSpec instead

# Be a good citizen

## ...and put something useful into the README file.

Live Long and Prosper!

# Commit

It's a good time to commit now.

If you're using RubyMine, open your `.gitignore` file and add a line with `.idea/` to it. This excludes RubyMine configuration files from being tracked.

    git add .
    git commit -m "First commit, RSpec installed"


# Test-First Teaching in Rails

## Add the `rails_tft` gem to your `Gemfile`

Add to your `Gemfile`

    gem "tft_rails"

Then run

    bundle install

# TFT Generators

    rails g  # see all generators

## Add tests (i.e. student assignments) to the project

    rails g chapter07:begin

## View generated instructions file

## Then add code to make them pass

    rake spec   # should be all green

## Skip ahead/stuck?

    rails g chapter07:solutions

# Many new concepts

* Not all covered by lecture
* Check generated instructions file
* Tips <http://ruby.railstutorial.org/ruby-on-rails-tutorial-book>
* Rails reference: <http://railsapi.com/>

# Why starting at Chapter 07?

* RailsTutorial udpated for Devise gem
* Industry-standard authentication solution
* Gives us more time to get to the good bits
* Don't build your auth system by hand

(unless you're a security expert)

# Chapter 7

# Exercises: user_spec

* Check for method present
* How does ActiveRecord know that there is a name method?
* No such method is defined anywhere???

# Exercises

* pages_controller_spec
* user_spec
* users_controller_spec (show action plus view)

# Chapter 08, 09

* Sign up and sign in

# Chapter 10 Exercises

Note: A new rake task `rake db:populate` was added to create dummy data for development

* Add an `index` action to `UsersController`, to view all users
  * Add pagination to the `index` view (Chapter 10.3.3, 10.3.4)
* Admin Privileges (Chapter 10.4)
  * Admin flag
  * Only admins can destroy users
  * Only admins see destroy link

(Add tests to check that the delete links in Listing 10.38 appear for admins but not for normal users.)
(Modify the db:populate task such that at least one user has admin privileges)

Note: Devise provides a controller method authentication_user!

Also available: generate dummy data

    rake db:populate

# TFT Gem update

    bundle update tft_rails

# Chapter 11.1

* Micropost model
* Association to User model

# Chapter 11.2

* MicropostsContoller

# Chapter 11.3

* Proto feed of user's posts on the home page

