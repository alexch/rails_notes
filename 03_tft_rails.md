
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

