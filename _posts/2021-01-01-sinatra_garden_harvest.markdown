---
layout: post
title:      "Sinatra Garden Harvest"
date:       2021-01-01 23:01:25 +0000
permalink:  sinatra_garden_harvest
---

## About the Application
I’ve built a Sinatra web application allowing a user to sign up, login, and log out, as well as track crops harvested in their garden. A user can create, read, update, and delete (CRUD) their own crops and read the crops of other users. Sinatra is a web framework for building rack-based web applications. It provides the bare essentials for constructing a web application as the lightweight alternative to Rails. Learning Sinatra has helped me to grasp the basics of web applications and establishing the fundamental components of how they function together. Garden Harvest was built with the Models Views Controllers (MVC) Framework for the web app stack, depicted in the triangle below. Starting with the database, I worked up the triangle to configure the application. By installing the Corneal gem I was able to begin creating Garden Harvest with the essential functional components already established. Corneal is a Ruby gem providing the necessary directories and files for a bare bones Sinatra application. See [https://github.com/thebrianemory/corneal](https://github.com/thebrianemory/corneal) for more Corneal information. Garden Harvest was developed using RESTful conventions for performing CRUD actions over the internet. Follow along this content with the code in [https://github.com/meranly8/sinatra-garden-harvest](https://github.com/meranly8/sinatra-garden-harvest).

![web_app_stack](https://i.imgur.com/veLdAbql.png)

## Database

Garden Harvest has a sqlite3 database with two tables to track users and their crops. A user has many crops and a crop belongs to a user. Users have columns of username, email, and password_digest with the established email and password being the login credentials.  The columns set up for a crop are name, quantity, year, season, user_id, created_at, and updated_at. These tables were created via migrations facilitated by rake tasks.

## Models, Views, and Controllers
### Models
In the User model, username and email are required and must be unique through ActiveRecord validations. The User model also utilizes the has_secure_password macro to salt and hash a given password to make it secure and provide built-in validations for the user’s password. An association with the crops table is established by the has_many :crops macro. In the Crop model, name, quantity, year, and season are all required fields and quantity must be at least 1 through ActiveRecord validations. These validations allow for #errors to be called on a crop object to display any error messages. 

### Views
Garden Harvest views are broken down into crops and users, and then have a layout and welcome page. View pages are embedded ruby (ERB) files allowing the combination of HTML and Ruby to be in the file. Ruby code is placed in either scripting (<%  %>) or substitution (<%=  %>) tags based on if it will be displayed in the browser or not. Only ‘get’ routes in the controllers have corresponding views to be rendered. Users have views to sign up, login, and show their personal homepage. Crops have views to index all crops, show a specific user’s crops, create a new crop, and edit an existing crop, if authorized to do so. The layout page holds the logic for what is displayed in the navigation bar and the footer, based on who and if a user is logged in, on each view. 

### Controllers
Garden Harvest has a main ApplicationController, CropsController, and UsersController for separation of concerns. The ApplicationController holds the configure do block, which includes enabling sessions to hold onto a user for the duration of their session in the web app. The only route in this controller is for the main URL. This route uses a helper method to determine if the user is logged in and if they are, the user is taken to their own show page and if not the welcome page is rendered to sign up or login. The ApplicationController is also where helper methods live so that they can be accessed by the entire application. #logged_in? determines if the user on the site is logged in or not, #current_user returns the object of the current user utilizing memoization to reduce calls to the database, #authorized_for? determines if a user is authorized to make changes on a crop based on the crop’s user_id, and #redirect_if_not_logged_in redirects a user to the main URL route if they are not logged in. 

The UsersController has four main functions pertaining to the user, sign up, login, logout, and showing their personal homepage. The ‘get’ routes all have corresponding views and helper methods are utilized throughout the controller. The CropsController contains full CRUD functionality for creating, reading, updating, and deleting crops. Helper methods are utilized throughout the controller, as well as a private method #set_crop established to find and capture the relevant crop from the database. This private method can only be used in the CropsController class.

### Challenges
I was going through the Sinatra project video tutorials while building my application and ran into an unexpected error. It was late at night and I implemented features discussed in the video, without testing each component, and then went to sleep. When I returned to continue working on the project the next day, I fired up shotgun and received an error I had not seen before. 

![error_message](https://i.imgur.com/Yy2J9i7l.png)

After spending time googling the error I wasn’t grasping anything I thought was specific enough to help me. With my anxiety growing, I knew it had to be something small and silly, which didn’t help. To escape my error paralysis, my resolution was to start over. I know, not a very good debugging strategy, but at the time I wasn’t too far along and thought if I re-built my app testing line by line-by-line I would definitely find the problem. Sure enough, I went to enable sessions in the configure do block of the new ApplicationController and found the error.

```
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    enable :session_secret, "discover_weekly"
  end
```
 
Instead of setting the session secret, I was enabling it (along with my stress). This taught me many things, most importantly to not go to bed without testing. Error anxiety is no joke. As I code more, I’m becoming more comfortable with errors, but they aren’t what I want to see. Going forward I’ve been taking things slower and instantly testing anything implemented.

### Future Growth
I enjoyed developing The Garden Harvest web application. There are two main improvement ideas I have for Garden Harvest. The first would be to configure a gardens table in the database to join users and crops through their gardens. The gardens table would track a garden’s location, size, etc. The crops table would change from holding the user_id to holding the garden_id and the gardens table would hold the user_id. My second upgrade would be to implement a ‘Donate’ button on the crop’s show page. This would trigger a notification to a local food bank or organization and logistics would be coordinated for getting the donated crops to people in need. 

