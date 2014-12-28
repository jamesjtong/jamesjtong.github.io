---
layout: post
title: "Easy Rails JSON APIs with rails-api and activemodel serializers"
date: 2014-12-28 12:32:01 -0500
comments: true
categories: ["rails api", "json", "activemodel serializers"]
---

JSON APIs are all the craze nowadays in web programming. Let's talk about an easy way to create a JSON API with rails-api. First, let's install the rails-api gem.

    gem install rails-api

Then let's make a new rails-api app (rails-api is basically just a lightweight version of rails with just the resources necessary and customized for creating an api, i.e. no view layer support). For this blog, I am going to create an api for the backend for one of my past apps (http://joblogger.herokuapp.com/)

    rails-api new joblogger-api && cd joblogger-api

Great, this command created what looks to be a smaller version of a normal rails app and changed directory into the app. Now let's add what we are going to need to the Gemfile:

```ruby
source 'https://rubygems.org'
gem 'rails', '4.1.0.beta1'

gem 'rails-api'

gem 'spring', :group => :development

gem 'pg'

# JSON serialization
gem "active_model_serializers", '~> 0.8.1'

group :development, :test do
  gem 'rspec-rails', '~> 3.0'
  gem 'factory_girl_rails'
  gem 'faker'
  gem 'pry'
end

```
Add your gem for whichever database you are going to be using (i am going to be using postgres and editing the database.yml, but it is fine and easier for you if you use the default sqlite gem)

The gems of note is the active_model_serializer gem (this let's me format the JSON output for various resources). My other gems are for testing using Rspec, factories, creating fake seed data (faker), and debugging (pry).

Let's run the bundle and execute the setup commands for the gems (for me there is only rspec).

    bundle install
    rails generate rspec:install

Great, now let's create a resource almost every app needs. Let's create users.

    rails g resource user name:string email:string favorite_color:string

Notice that the rails generator now knows that this is an api (thanks to this being a rails-api project) and refrains from generating routes that arent used in apis (if you look at the routes.rb file, new and edit aren't included). Also notice that a serializer is generated for the user model thanks to our activerecord serializer gem :

```ruby
#routes.rb
Rails.application.routes.draw do
  resources :users, except: [:new, :edit]
end
```

Now let's create some seed data using Faker.

```ruby
5.times do
  User.create(name: Faker::Name.name, email: Faker::Internet.email, favorite_color: Faker::Commerce.color)
end
```

Let's write some tests and create an index action on the UsersController to get a list of all the users. Let's say the functionality we want for our app is for the favorite_color to not be shown as part of the index action:

```ruby
#spec/controllers/user_controller_spec.rb
require 'rails_helper'

RSpec.describe UsersController, :type => :controller do
  describe "index" do
    before { get :index }
    it "responds successfully with an HTTP 200 status code" do
      expect(response).to be_success
      expect(response).to have_http_status(200)
    end

    it "lists the user's public attributes while hiding the user's favorite_color" do
      response_keys = JSON.parse(response.body)["users"].first.keys
      expect(response_keys).to include("id")
      expect(response_keys).to include("name")
      expect(response_keys).to include("email")
      expect(response_keys).to_not include("favorite_color")
    end
  end
end


#app/controllers/users_controller.rb
class UsersController < ApplicationController
  def index
    users = User.all
    render json: users
  end
end
```

Now let's run rspec . and we expect the second test to fail as the response will probably include favorite_color. (NOTE: you may need to run rake db:setup with the appropriate rails environments and/or rake db:migrate to setup your test and development databases if you haven't already)

    rspec .

Great as expected, one test failed. You're failure should be somewhere along the lines of:

    Failure/Error: expect(response_keys).to_not include("favorite_color")
           expected ["id", "name", "email", "favorite_color"] not to include "favorite_color"


Now, let's edit our user serializer so that favorite_color isn't shown as part of the index action (/users)

```ruby
#app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  attributes :id, :name, :email
end

```

The attributes are a whitelist of a what is shown. Now let's rspec . again and we can see all tests pass! 
You can also run a rails server and hit your local endpoint /users to confirm that this works. You should get something like this

```json
{  
   "users":[  
      {  
         "id":6,
         "name":"Devonte Hagenes",
         "email":"brendon@kihnerdman.com"
      },
      {  
         "id":7,
         "name":"Malcolm Huel",
         "email":"kimberly.wisozk@nicolasgrant.net"
      },
      {  
         "id":8,
         "name":"Wilfredo Bruen",
         "email":"ashleigh@goldner.org"
      },
      {  
         "id":9,
         "name":"Miss Arielle Leuschke",
         "email":"don_spinka@spinka.net"
      },
      {  
         "id":10,
         "name":"Quinn Hoppe",
         "email":"rita@olsonprohaska.biz"
      }
   ]
}
```

Congratulations, you have just created a simple Rails JSON API! (To view the full code, visit https://github.com/jamesjtong/joblogger-api/tree/blog-12-28-14)

HAPPY HACKING






