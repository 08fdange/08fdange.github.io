---
layout: post
title:      "My Last Portfolio Project"
date:       2020-11-30 01:49:32 +0000
permalink:  my_last_portfolio_project
---


**Introduction**

For our last project we were assigned to creating a single page application using React and Redux for the frontend while utilizing a Rails API for the backend. While still brainstorming ideas for my project, I had to help a friend make a post on Craigslist. While on there, I noticed how dated and boring the interface was yet, the site is still a very popular option for posting random things online for sale. So, my idea was to create a web application similar to Craiglist but a more intuitive and friendly user interface. I decided to name it "PiratePort" with the theme in mind that "one man's trash is another man's treasure". 

**Planning**

To plan my project I used my iPad my to draw out my model relationships I would have on the backend, as well as drawing a rough draft what the interface would be like for the end user. I then decided on what I wanted my routes to look like among other things. Then I got to work..

**Backend**

For the Rails API backend I created migrations for a User model and Item model. A User has many Items and an Item belongs to a User. I had to decide between token-based authentication and session-based. I chose to go with JWT authentication because it is what most modern web applications use for multiple reasons including scalability and mobile device authentication. It did prove to be a bit more challenging, you can check out my other blog post about my implementation of it [here.](https://08fdange.github.io/jwt_user_authentication_in_rails_api) After setting up the user authentication, I then set up the avatar picture uploads for User model via Cloudinary using the 'cloudinary' gem. Afterwards, I implemented a similar solution for the picture uploads for the items. Here's some of the other gems I used in on my Rails API: 

* 'jwt' - for user authentication
* 'simple-command' - for building and use Service Objects in Ruby
* 'foreman' - used for starting both Rails and npm servers
* 'fast_jsonapi' - used for creating serializers for models, an Active Model Serializer

**Frontend**

I started my React/Redux frontend by getting a working user sign up/login interface. After I got that working, I started creating the rest of components needed for a user that would be logged in. For the main container components, there is a HomeContainer, ProfileContainer and ItemsContainer. As far as other components well, there are a lot. I tried focusing on making as many components as I could reusable. I used Material UI elements in many of components for styling. If there was more than one component needed to bring a feature to life I would organize them in folders according to those said features. For example, the form for creating a new item has three components, a container for the item form called NewItemForm, a NewItemDetails component which is responsible for the actual user input and a NewItemConfirm component which displays all of the information the user entered for them to be able to confirm everything is correct before submitting it. All three of these components were put in my NewItemForm folder which was located in my components folder. The whole frontend was organized in this fashion. 

**Conclusion**

Overall, the project was really quite enjoyable to work on and I'd describe React as a fun framework to work with. As far as my goal of making the application like a more user friendly and better looking version of craiglist, I would say I succeeded. It is easy to sign up as user and quicker and easier to post an item for sale than on craigslist. I can't wait to apply some of what I've learned working on this project on my next one! 


![](https://res.cloudinary.com/dbtndpluf/image/upload/c_scale,w_800/v1606700716/Screen_Shot_2020-11-29_at_8.40.50_PM_a0ptb1.png)

