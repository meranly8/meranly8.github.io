---
layout: post
title:      "Moody Foodie"
date:       2021-02-28 21:00:56 +0000
permalink:  moody_foodie
---


## About
I have been keeping a food journal for a couple years now and, along with what I consume throughout the day, I have also been tracking how I’m feeling at the end of the day. I don’t know whether they really correlate, but just thought it would be interesting data to have. My idea for this Rails project was to create the structure of my food journal with four models of User, Entry, EntryProvision, and Provision. 

![MoodyFoodieSchemaImgur](https://i.imgur.com/SUjT0An.png)

For the first week of the project I had anxiety over how to relate all of the items. As I moved through development I was also, in parallel, building out another project for wine tracking with User, Wine, and Winery models. This was an easier structure for me to visualize the relationships, but I still wanted to try to make the Moody Foodie happen. At least 4 times I felt like giving up and just moving forward with the Wine, but as I debugged and went back through lecture videos things started clicking for what could be applied to the Moody Foodie. I feel like I struggled with this project more than the Sinatra and CLI projects. My original model setup included tables for additional items to track with yes/no (i.e. exercise, drinking water, headaches, etc). This quickly became a stretch goal. On instagram I’ve seen bullet journal charts of people’s daily moods for a month and that kind of visualization is also a stretch goal for this app.

## User Experience
When users begin with the Moody Foodie, they can create an account or log into the app using their Google account. From the home page they can view their existing entries or create a new one. Entries hold the date and mood for the day and in the entry, entry provisions can be added, edited, and deleted. The entry as a whole can also be deleted and users can only create one entry per day. Provisions live in their own table and do not belong to a user. They can not be updated or deleted once created. I went with the word ‘provision’ because I wanted it to encompass food and drink. The word also reminds me of when they unload the provisions on Below Deck. Users have the ability to view their list of entries, the entry details with its list of entry provisions, the calorie breakdown and sum for the entry, list of provisions, and provision details with who has consumed it.

## Development
To begin developing the Moody Foodie, I did ‘rails new rails-moody-foodie’, generated the models, migrated the ActiveRecord tables and established the relationships between the models listed in the diagram above. When using the handy rails generators for this app, I stuck with use the model and controller generators so I didn’t get too many extra files I would later delete. Before starting on any CRUD functionality, I first wanted to get the signup, login, logout cycle established. From there I could get the current_user helper method working and be able to associate new entries to the current_user. Moody Foodie has its own authentication system, along with utilizing OmniAuth with Google to also log users in. From there I started to build out my controller action views. 

When looking at refactoring logic out of my views, I was struggling with how to get all the display code I wanted returned on one line. It took me a while to realize partials are able to hold larger displays to be displayed conditionally. This helped a lot with the navigation links for when a user is not logged in and when they are. It also helped with conditionally displaying edit and delete fields for entries that belong to the current_user. With this app I didn’t have as much time as projects in the past to develop styling for the site. The only styling I added was for errors to be displayed in red. 

## Expanding Moody Foodie
My mood at the end of finishing the requirements for Moody Foodie was proud. It has been in my ideas list for a while and to be able to see it reflected in the browser is a cool feeling. As I keep thinking about it, the stretch goal list grows:

1. Daily mood color visualization
2. Additional yes/no tracking items (exercise, drinking water, headaches, etc.)
3. Allow users to create their own collection of moods to select from
