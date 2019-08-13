---
layout: post
title:      "My First CLI Gem Project"
date:       2019-08-13 00:11:07 -0400
permalink:  my_first_cli_gem_project
---


Hey there!  Welcome to my first official project blog.  The purpose of this project is demonstrate the skills we've been taught since the beginning of the course and to have a portfolio of projects to showcase when it's time to start interviewing for that dream developer's job.

# About My Project
My project is a CLI gem that involves displaying a list of guitar and accessory items scraped from a website.  The user runs the program by typing `./bin/run` in terminal and following the prompts thereafter.  A nice welcome message appears along with a list of the guitar and accessory items that are "for sale."  There are 45 items in all on my "My-Music" site.  At the end of the list is a prompt giving the user choices to either enter an item number to learn more about it, the word "cat" to see the catalog (list) of items again, or "exit" to close the program with a nice thank you and goodbye message.  If erroneous input is entered by the user, the program alerts the user as such and re-prompts the user to enter the appropriate input.  When acceptable input has been entered, the program displays the item name, price, and a brief description of that item if one exists.  The user is then prompted again to make choices or close the program and the process will continue until "exit" is entered.  The program involves object orientation, scraping, object relationships, and iteration.

# What I Learned
A ton!  As deceivingly simple as this project seems on the surface, it's really loaded with many programming concepts that interrelate and flow together in order to produce the desired outcome.  One concept I will point out, though I confess that I don't fully grasp how it works, is called a proc.  Here's an example from my project:

```
 def self.scrape_guitar_list
    guitars = self.get_page.css("div .span-11 a.bp-title")[0..44].map(&:text).map(&:strip)
    links = self.get_page.css("div .span-11 a.bp-title").map{|item| item.attr("href")}
    make_guitars(guitars, links)
  end
```

The guitars variable has procs at the suffix of the line: .map(&:text) and then .map(&:strip).  These work like calling the .text and .strip methods on elements, however, I was getting errors when attempting to just use those two methods without the procs.  *"If you don’t know what a Proc is, you can consider it to be just like a lambda or a closure. It’s a piece of code that can be moved around and executed (by calling call() on it, for instance)."*  

# What Ruby Does When It Sees &
Whenever Ruby sees a & sign as a parameter, it wants that parameter to be a proc.  If this isn't the case, Ruby calls to_proc on this object to convert it.  In my code example above, the guitars variable is set to a .css selector that scrapes all of the guitar and accessory names that I want.  The .map method returns an array of these names and then the procs perform the .text and .strip methods so that I only get the content returned that I actually want.  I don't really know how it works, I just know that it does and it got rid of the errors that I was getting without employing this rather useful tool.  I improved my error handling skills, although I still have much to learn.  I wanted to trash my computer so many times during this project and for a while, wasn't seeing any light at the end of the tunnel.  Thanks to tenacity and some very much needed help from my cohort lead, I COMPLETED THIS SON-OF-A-BISCUIT!!!  Maybe I'll even build another one...but for now, I'm going to celebrate my success, um, tomorrow because I'm sleepy as heck!  Night, night...

