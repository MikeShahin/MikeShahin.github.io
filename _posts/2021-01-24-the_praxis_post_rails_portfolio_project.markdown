---
layout: post
title:      "The Praxis Post, Rails Portfolio Project"
date:       2021-01-24 21:53:37 +0000
permalink:  the_praxis_post_rails_portfolio_project
---


For my rails portfolio project, I wanted to make a reddit like website for leftists. When I began the project I first set up my tables and models, followed by the controllers and views. After a few days of getting all of that set up I ran into a huge problem when running my rails server, it would give me an error about a manifest missing for the Javascript file linked in the application layout erb. I though, ok I can just remove that line and it shouldn't be a problem because this is a rails app and not javascript. 

I removed the line and was able to get my server running and at first everything was working fine until I tried deleting records and omni-auth logins. I ran into all sorts of problems trying to get omni-auth working and no amount of 'fixes' from google/stackoverflow were helping. I set that aside and went on to try to fix deleting records properly. I wouldn't get an error when trying to delete but it would just reload the page with the record still there, not doing anything. I went over my code again and again and could not find any fault in it. When I started googling the problem, I found out that the destry function relied on the javascript file I had removed from my app...

So off I went trying to get my rails app to accept the javascript file. I spent about the firt week of the project trying to fix these issues to no avail. Finally I realized that all the people talking about this problem were on Rails version 6 as was I so I tried to roll back to Rails 5, but that broke a bunch of other things. Frustrated I just decided to scrap it all and started a new app in Rails 5 and painstakenly copied all my old files over and viola! it finally worked, omni-auth, delete, everything else!

With that out of the way I went back to adding different features to my app. Initially it had the basic ability for users to view posts and read/add comments to posts. In the following days I added a search and sorting feature to the index (all posts) page that allows users to search for a post, or sort all the posts by newest, oldest, link posts, or self posts. Later I added a communities feature that allows users to create communities which posts would go into, kind of like subreddits. I also added a feature that allows image posts to be viewed on the posts show page. Finally I reworked the users show page to allow viewing a users posts, comments, and communities they are active in.

After adding all the features and making sure everything worked, I added bootstrap and began styling, adding a navigation bar and making the website look a whole lot nicer. Finally I went in and refactored all my code to make it neater and more DRY. I think I did a really good job on this project and am really proud of it.
