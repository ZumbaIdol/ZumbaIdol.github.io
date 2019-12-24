---
layout: post
title:      "This Rails Project, Though..."
date:       2019-12-24 18:20:06 +0000
permalink:  this_rails_project_though
---


This project was challenging, indeed!  I try not to be too hard on myself, knowing that I've only been learning about Rails for a few months and have successfully built projects with it, Sinatra and Ruby within a 6 month period.  I had no experience with any of the aforementioned languages and frameworks, so I should be feeling pretty good about things, and I do.  There just remains this *stubborn* part of me that remains *perfectionist*, always pushing me to be better, to "get it all at once".  Yeah, I know, not feasible...that's why there's such a thing as Google, helpful classmates, helpful instructors, helpful AAQ technichians, Stack Overflow and the Ruby/Rails documentation to get us unstuck, right?  I can happily say that I was able to get past many roadblocks in this project myself by taking the time to carefully read error messages and interpret what the message was trying to tell me about my code.  That being said, there were yet some rather simple errors that I missed, but I'm learning that such is the life of a programmer.  Programmer...that's a term I'm still becoming a accustomed to when referring to myself.

I want to briefly write about the scope aspect of this project, which was something new and not covered in the labs leading up to the project (as if there weren't already enough concepts that I didn't have full command of!)  **Scope** is a custom query defined in your Rails models with a scope method:

```
class Movie < ApplicationRecord
  has_many :movie_logs
  has_many :users, through: :movie_logs
  validates :title, :category, :rating, presence: true

  scope :most_comments, -> {(
    select("movies.id, movies.title, count(movies.id) as movie_count")
    .joins(:movie_logs)
    .group("movies.id")
    .order("movie_count DESC")
    .limit(1)
    )}
end
```

My scope method above queries the movie with the most comments about it in my Movie Library app.  Most_comments is the name that I call the scope in my code and "->" is called a lambda, which implements the query.  The result of calling a scope is an ActiveRecord::Relation object.  Mhy scope selects the most commented on movie by it's id and title by counting all of the movie comments in the database (aliased ad movie_count).  It grabs this info from my join table, called movie_logs in my project, which is where the comments reside.  This scope orders the movie_count ins descending order (DESC) and grabs the movie with the most comments on it (limit(1)).  The above scope is in my Movie model, so I had to incoporate code into my movies_controller:

```
def index
    @most_popular = Movie.most_comments.first
    @user = current_user
    @movies = @user.movies
  end
```

The action in my movies_controller#index assigns the variable @most_popular to the movie with the most comments (Movie.most_comments.first).  This information will display on the currently logged in user's page along with the movies this user has commented on (@user = current_user and @movies = @user.movies, respectively).  I also display the most commented on movie on the all comments index page:

```
<h1>All Movie Comments:</h1>
<h3>Movie with most comments: <strong><%= @most_popular.title %></strong></h3><br><br>

<ul>
  <% @movie_logs.each do |movie_log| %>
    <li>
      <%= movie_log.user.name %> saw <%=movie_log.movie.title %> and commented <strong><%= movie_log.comments %></strong>

      <%= link_to "View Comment", movie_log_path(movie_log) %>

      <%= link_to "Edit Comment", edit_movie_log_path(movie_log) if movie_log.user == current_user %>

      <%= link_to "Delete", movie_log_path(movie_log), data: { confirm: "Are you sure you want to delete this comment?" }, method: :delete, class: 'btn btn-mini btn-danger'  if movie_log.user == current_user %> 
    </li>
  <% end %>
</ul>
```

I used a little header and bold styling to make it stand out a little.   As with many other aspects of coding, the scope method can get more complex if you chain and combine scopes.  It was enough for me (and the project requirements) to get this simple example to work, (which didn't when I first attempted to implement if, of course, but you know how that stuff goes...)
