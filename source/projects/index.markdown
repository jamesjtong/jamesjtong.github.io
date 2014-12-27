---
layout: page
title: "projects"
date: 2014-12-27 11:45
comments: true
sharing: true
footer: true
---
List of projects

GameTable = A Real Time table-top platform that lets you play your favorite board games with friends
+ Integrated Sync Library and Pusher to post real time responses to player moves and chat messages across all users
+Combined simplicity with security for usage of the app, using Ruby's SecureRandom class and Action Mailer
+Used ruby metaprogramming and used seed_dump gem to create and reset token positions 
+Implemented background jobs using Whenever for automatically deleting games from our database
+Styled application using plain Twitter bootstrap and custom images
+Deployed via Nginx and Digital Ocean

http://gametable.co

jobLogger = AngularJS clientside app that lets you create cover letters, thank you letters, practice interviewing, etc. (in progress)
+no jQuery, everything done the angular way
+custom styles, filters, directives
+jasmine and e2e tested
+integrates zeroclipboard so users don't have to manually copy and paste; they can just click a button
+deployed via Heroku

http://joblogger.herokuapp.com

NBASTALK = A Ruby on Rails app that lets you view your favorite NBA player's recent social media and statistics
+ Built a dynamic social media/sports statistics dashboard for NBA athletes
+ Utilized Nokogiri and Mechanize gems to scrape espn for core basketball statistics
+ Integrated jquery ui to add an autocompleting search bar so users can search for their favorite
athlete
+ Used JavaScript to interact with the APIs of Twitter, Facebook and Youtube
+ Wrote various complex regex expressions to parse data to custom format
+ Setup and deployed to Digital Ocean server using NGINX, Passenger, and Capistrano 

http://nbastalk.com

