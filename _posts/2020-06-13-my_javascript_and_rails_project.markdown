---
layout: post
title:      "My JavaScript and Rails Project"
date:       2020-06-14 01:16:10 +0000
permalink:  my_javascript_and_rails_project
---


Hi there...

I'm going to briefly write about using a very hand utility called the Faker gem for your projects.  The Faker gem contains libraries called modules that can be used to generate initial data for your database.  You can view the modules that are available in the gem here: [https://www.rubydoc.info/github/stympy/faker/](http://).  I chose the DcComics module which was perfect for me because I LOVE superhero comics and movies (and there wasn't a Marvel Comics module available).  To use it, put the gem in your Rails Gemfile like so:

gem 'faker'

Then, run bundle install.  Once installed, go to your seeds.rb file and require the gem.  I also wanted to have random comics generated for the initial users I had set up, so I did the following:

require 'faker'
require 'securerandom'

I set up a group of initial users:

users_name = [
    'David',
    'Greg',
    'Hakim',
    'Aaron',
    'Theresa',
    'Deb',
    'Jared'
  ]
	
	The gem took care of creating a random collection of comics for these users with the following:
	
	user_collection = []

  users_name.each do |name|
    user_collection << User.create(name: name)
  end

  user_collection.each do |user|
    collection_size = (SecureRandom.random_number(5) + 1).floor

    (1..collection_size).each do |comic|
        title = Faker::DcComics.title
        hero = Faker::DcComics.hero
        heroine = Faker::DcComics.heroine
        villain = Faker::DcComics.villain

        DcComic.create(title: title, hero: hero, heroine: heroine, villain: villain, user_id: user.id)
      end
    end
		
The gem randomly created comics for the users with a title, hero, heroine, and villain.  There was a name option that I opted out of using for the comic in lieu of it's title.  Once I developed the code to create new users, the gem took care of generating comics for them once everything was in place for the front and backend communications to connect.

I haven't figured out how to post snips that I've saved on my PC to show you the outcome after some styling, but this gem is a great resource to help fill out your project!  Learn more about the methods that are available in this module here: [https://www.rubydoc.info/github/stympy/faker/Faker/DcComics](http://).
