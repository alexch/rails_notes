!SLIDE subsection

# Ruby-on-Rails

!SLIDE

## MVC

![MVC](images/mvc.png)

# Command-Line Summary

## Creating a Rails project

    rails new my_app
    cd my_app

(Error? Try `gem install rails`)

## Versioning with Git

    git init
    git status
    git add .
    git status
    git commit -m "Initial commit"

## Deploying to Heroku

    heroku create
    git push heroku master

(Error? Try `gem install heroku` and make sure your *ssh key* is correct.)

# Onto a Real (Demo) App

## We're following Michael Hartl's Rails Tutorial

<http://ruby.railstutorial.org/>

# Create App

    rails new demo_app
    cd demo_app
    echo "rvm use 1.9.3@rails3" > .rvmrc
    git init
    
    
# Create a Scaffold for User

A scaffold is a vertical slice of application, including a database migration, a model, as well as controller, and views for basic CRUD operations.

    rails generate scaffold User name:string email:string

# A note on Rails conventions

* Tables
    * Lower case and plurals, e.g. `users`
    * They represent many users

* Models
    * CamelCase and singular, e.g. `User`
    * Models are Ruby Classes
    * An instance of a model represents a single database record

# Migrating

    rake db:migrate

* A *migration* is a little script that changes the database schema
* When you run `rake db:migrate`, Rails applies *all* the migrations that have *not yet been applied* to the current database
* This allows you to change the development schema ahead of changing the production schema
  * when you deploy to production, run `rake db:migrate` and get *all the new migrations*

# A look at the model

    rails console

    > ActiveRecord::Base.logger = Logger.new(STDOUT)
    > User.create!(:name => 'Joe', :email => 'joe@example.com')
    > User.find(1)

# Restful Routes

    map.resources :users (config/routes.rb)

    rake routes

    users_path, users_url "named route methods"
    GET		/users			-> "index" action
    POST	/users			-> "create" action

    new_user_path, new_user_url
    GET		/users/new		-> "new" action

    edit_user_path(id), edit_user_url(id)
    GET		/users/:id/edit	-> "edit" action with ID

    user_path(id), user_url(id)
    GET		/users/:id		-> "show" action with ID
    PUT 	/users/:id		-> "update" action with ID
    DELETE  /users/:id		-> "destroy" action with ID


!SLIDE

![MVC detailed](images/mvc_detailed.png)

Source: (c) 2011, Michael Hartl, railstutorial.com

# Generated controllers

* Take a look at `app/controllers/users_controller`
* Note that each restful route correspond to a method (an "action")
* There are 7 of them:
    * `new`, `edit` -- serve forms to create a new or edit existing records
    * `index`, `show` -- list all or a single record
    * `create`, `update` -- process a form post for creating or updating a record
    * `delete` -- removing a record

# Problems with Scaffold

* No data validations
* No authentication or access protection
* Both HTML and XML responses -- often not warranted
* Tests are boilerplate, and not very robust
* No real understanding

# Models

# Rails Console

    rails c[onsole]

    ActiveRecord::Base.logger = Logger.new(STDOUT)

    u = User.new
    u = User.new(:name => "Joe Smith", :email => "joe@example.com")
    u.save
    u.save!
    u = User.create(:name => "Joe Smith", :email => "joe@example.com")
    u = User.create!(:name => "Joe Smith", :email => "joe@example.com")

    u = User.find(1)
    u = User.where(:email => "joe@example.com").all

# Attribute Methods

Each column in the database auto-magically turns into a method on the model object, both setters and getters.

    @@@ruby
    u = User.new
    u.name = "Joe"
    u.name  # => "Joe"

Behind the veil of magic: `method_missing`

# Instantiating vs. Creating

## In-memory operations
* `User.new({...})`
* `u.attributes = {...}`
* `u.name = ...`

`save` commits changes of an in-memory object to the database.

## Direct-to-DB operations

* `User.create({...})`
* `u.update_attribute(key, val)`
* `u.update_attributes({...})`

# Validations

* Secure data integrity at the model level
* Prevents bad data from entering the database
* Un-burdens controller or UI from messy validation code

!SLIDE

    @@@ruby
    class User < ActiveRecord::Base
        validates :name, :email,
                  :presence => true,
                  :length   => {:maximum => 256}

        ...
    end

# Active Record API

* find
* create
* count
* valid?
* changed?
* errors
* attributes, attributes=
* update_attribute(s)
* destroy

Reference: <http://railsapi.com/>

# Queries

## Compose queries with

* where
* order
* limit
* include
* join

## Then execute them with

* first
* last
* all

Reference: <http://guides.rubyonrails.org/active_record_querying.html>

# SQL Injection Guards

## NEVER use STRING INTERPOLATION in SQL conditions!!!

    str = 'joe'

    User.where("name = #{str}")   # This is UNSAFE!!! DON'T DO IT!

    User.where(["name = ?", str])  # This is safe

    # Alternatively, with hash keys substitution
    User.where(["name = :str AND email = :email", :str => str, :email => 'joe@example.com'])

# Add RSpec
Open `Gemfile` and add:

    group :development do
      gem 'rspec-rails', '~> 2.6.0'
    end
    group :test do
      gem 'rspec', '~> 2.6.0'
    end

Then, run:

    bundle install

Bundler is a tool to manage a project's gem dependencies. This command will check and, if necessary, install all the gems and specific versions according to the `Gemfile` manifest.

The `~>` syntax for specifying versions mean that bundler will tolerate minor release revisions (e.g. `"~> 1.0.0"` will also accept version 1.0.1, but not 1.1.0)

# Run RSpec's installer

    rails g rspec:install


# CRUD Exercise

## Class Methods

* new
* find
* create
* count

## Instance Methods

* attributes, attributes= (operates in-memory only)
* update_attribute(s) (saves right away)
* destroy

## Exercise

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

# Destroyed Objects

##... are frozen

    @@@ruby
    u = User.create(:name=>'Joe', :email=>'joe@example.com')
    u.destroy

    u.name  # => "Joe"
    u.id    # => ID

    # but:
    User.find(u.id)  # => Kaboom!

    u.frozen?  #  => true


Frozen objects can't be modified

# Query Exercise

* ActiveRelations Query language

## Compose queries with

* where
  * where(:name => 'Joe')
  * where(["email LIKE ?", '%example.com'])
* order
  * order('name ASC')
* limit
* include
* join

## Then execute them with

* first
* last
* all
* count


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

!SLIDE subsection

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

* RailsTutorial adpated for Devise gem
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

# Named routes

The generator created entries in `config/routes` like so:

    get "pages/contact"

If you switch this to...

    match '/about', :to => 'pages#about'

...you'll get so-called "named routes" with methods:

    about_path  # relative url (without hostname)
    about_url   # absolute url (with hostname and protocol)

You can verify this by running

    rake routes

# Chapter 08, 09

* Sign up and sign in

# Test Data

## Define the factory

    Factory.define(:user) do |f|
      f.name "Joe"
      f.email "joe@example.com"
    end

## to produce users

    Factory(:user)  # =>  User record with name=="Joe", ...

&nbsp;
* Factory data is for testing
* Permits quick creation of a few records

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

# Protecting attributes

## `attr_accessible`

* Whitelist of attributes that are mass-assignable, i.e.

    User.create!(:name => "Joe", :email => "joe@example.com")

&nbsp;
* Don't make privileged data mass-assignable!

# Form posts


    @@@ruby

    @@@ruby
    class UsersController < ApplicationController

       def create
         @user = User.create(params[:user])
         ...
       end
    ...
    end

# The `params` hash

* Form posts
  * Attributes with `name[...]` will turn into has keys
* Url components, as defined by routes
  * `match "/about/:page" => ...
  * produces a `:page` entry in `params`

# Migrations

* start with a generator
  * `rails g migration MODEL field:type field:type`
  * e.g. `rails g migration User User name:string age:integer`
* you must add extra indexes on your own
  * or `rails g migration add_some_indexes` makes an empty migration which you can fill in 
* Migrations are stored in the `db/migrate` directory

# Access Control

## View

* Don't show a privileged link to non-privileged users

## Controller

* Check for privilege on destructive operation
* Protect against form spoofing

# Microposts

    rails generate model Micropost content:string user_id:integer

    rake db:migrate


# Validations for Micropost

    @@@ruby
    class Micopost < ActiveRecord::Base
        validates :content,
                  :presence => true,
                  :length   => {:maximum => 140}
        ...
    end

<http://guides.rubyonrails.org/active_record_validations_callbacks.html#validation-helpers>

# Associations

    @@@ruby
    class User < ActiveRecord::Base
      has_many :microposts
    end

    class Micropost < ActiveRecord::Base
      belongs_to :user

      validates :content, :length => { :maximum => 140 }
    end

!SLIDE

![Micropost User Association](images/micropost_user_association.png)

# A closer look

    rails console

    > u = User.find(1)
    > u.microposts.create(:content => "Hello World")

# Deploy

    # create .gitignore file
    git init
    git add .
    git commit -m 'initial commit'

    heroku create
    git push heroku master
    heroku rake db:migrate

# Scopes

## Named queries

    @@@ruby
    class ShippingAddress < ActiveRecord::Base

      has_many :orders

      scope :us, where(:country => "US")
      scope :in_state, lambda {|st| where(:state => st) }

      scope :on_file, lambda { includes(:orders).order('orders.created_at DESC').where('orders.created_at > ?', 12.months.ago) }
    end

# Many-to-many relationships

    @@@ruby
    class Product
      has_many :order_line_items
    end

    class OrderLineItem
      belongs_to :product
      belongs_to :user
    end

    class User
      has_many :order_line_items
      has_many :products, :through => :order_line_items
    end

# TFT Gem update

    bundle update tft_rails

# Chapter 11.1

* Micropost model
* Association to User model

# Chapter 11.2

* MicropostsContoller

# Chapter 11.3

* Proto feed of user's posts on the home page

# Testing for instance variables in a controller spec

    # spec fils:

    it "has a variable x" do
      get :action
      assigns[:x].should_not be_nil  # accesses @x in controller
    end

    # Controller:

    def action
      @x = ...
    end

# Time helpers

    1.day.ago
    2.weeks.since

    time_ago_in_words(Time.now)
    time_ago_in_words(1.day.ago)

# Rendering by object collection

In views:

    # show.html.erb

    <% @users.each do |u| %>
      <%= u.name %>
      ...
    <% end %>

# Move it to a partial by the same name

    # show.html.erb
    <%= render @users %>

    # _user.html.erb  (singular!)
    <% user.name %>

.

* Automatic loop

# Finders

    User.find(4)   # raises exception if not found

    User.find_by_id(4)  # return nil

    # also:

    User.find_by_name("Joe")
