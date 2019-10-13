---
layout: post
title:      "My 1st Sinatra App"
date:       2019-10-12 22:32:02 -0400
permalink:  my_1st_sinatra_app
---


This project was somewhat fun for me despite the pressure of needing to complete a project successfully in order to pass.  It was suggested that this project entail something that we cared about; for me, the answer was MOVIES!  I've accumulated quite a collection of movies over the years, and I'm especially enamored with superhero movies.  I decided that I would attempt a movie tracker app where users could sign up, login, create, update and delete movies in their collections.  I would like to take a moment to thank the creator of the Corneal Gem, which definitely made the project easier to facilitate.  There was one small issue, however, with some of the folder names and even the app name having some peculiar spellings with upper case letters and dashes being in some strange places within the words.  I had to edit those but that was simple enough to do.

After meeting the project requirments of creating a MVC (Model View Controller) app [https://learn.co/tracks/full-stack-web-development-v6/sinatra/mvc-and-forms/intro-to-mvc ](http://) with CRUD (Create, Read, Update, Delete) capabilites.  The app had to utilize ActiveRecord [https://learn.co/tracks/full-stack-web-development-v6/sinatra/activerecord/activerecord-setup-in-sinatra](http://) among other requirements.  For a bonus challenge, we could add error messaging if users tried to manipulate data from another user.  One of the tools Sinatra utilizes for this task is called Rack-Flash [https://github.com/nakajima/rack-flash](http://).  In order to incorporate it in your app, there is certain code that must be inserted in key places in order for it to work:

```
config.ru
require './config/environment'

if ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending. Run `rake db:migrate` to resolve the issue.'
end

use Rack::MethodOverride
use UsersController
use MoviesController
use SessionsController
use Rack::Flash
run ApplicationController
```

```
gemfile
source 'http://rubygems.org'

gem 'sinatra'
gem 'activerecord', '~> 4.2', '>= 4.2.6', :require => 'active_record'
gem 'sinatra-activerecord', :require => 'sinatra/activerecord'
gem 'rake'
gem 'require_all'
gem 'sqlite3', '~> 1.3.6'
gem 'thin'
gem 'shotgun'
gem 'pry'
gem 'bcrypt'
gem 'tux'
gem 'rack-flash3'

group :test do
  gem 'rspec'
  gem 'capybara'
  gem 'rack-test'
  gem 'database_cleaner', git: 'https://github.com/bmabey/database_cleaner.git'
end
```

```
environment.rb
ENV['SINATRA_ENV'] ||= "development"

require 'bundler/setup'
require 'rack-flash'
Bundler.require(:default, ENV['SINATRA_ENV'])

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)

require './app/controllers/application_controller'
require_all 'app'
```

I had the wrong version installed at first and it wouldn't run, nor would it allow my app to load when running "shotgun" 
[https://learn.co/tracks/online-software-engineering-structured/sinatra/sinatra-basics/using-the-shotgun-development-server](http://).  Uninstalling the incorrect version was a little of a task because it kept showing up in the gemfile after supposedly being removed.  I removed everything flash related and re-ran "bundle install" and later reinserted the flash related code after installing the correct version as indicated in my code snippets above, rack-flash3.  Here's what happens if a user tries to delete data from another user:

[https://imgur.com/a/5K7nZT9](http://)  The code behind this message comes from here: 

```
delete '/movies/:id' do
      movie_user = Movie.find_by_id(params[:id]).user
      if movie_user.id == current_user.id
          Movie.destroy(params[:id])
          redirect :'/movies'
      else
          flash[:err] = "You aren't authorized to delete the selected movie."
          redirect :'/movies'
      end
  end
```
	
	
One last interesting encounter I had while creating this app was tring to add a background image to replace the generic background created by the Corneal Gem.  For some reason, the default main.css file that comes with the gem wouldn't allow me to upload the image (I discovered this was a problem for others as well, including a fellow classmate).  The solution for this was to simply create another .css stylesheet and then "copy-pasta" the code from the original stylesheet to the newly created one.  The image I added is seen more clearly on the bottom image located here:   [https://imgur.com/a/5K7nZT9](http://).  Happy coding!

