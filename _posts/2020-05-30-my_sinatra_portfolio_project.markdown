---
layout: post
title:      "My Sinatra Portfolio Project"
date:       2020-05-30 04:48:50 +0000
permalink:  my_sinatra_portfolio_project
---

For this project, I decided I wanted to do something different. I wanted to make something that I would potentially use, something that aligned with my interests. I ended up with "Workout Logger", a site you could use to enter in your workouts and add all the exercises that were included in that workout. 

I started out by creating my models, which include a User, a Workout, and an Exercise model. A user has many workouts. A workout belongs to a user, and has many and belongs to many exercises. An exercise has many and belongs to many workouts.  At first, a User had an email, username and password attributes. The Workout model has a title and user id attributes. The Exercise model had a name to start. I later added a bio attribute to the User model and description and exercise type attributes to the Exercise model, via additional migrations. 

On the home page there is an option to sign up or log in. Once you are logged in, the homepage displays all workouts that have been entered by all users and there is hyperlinks for the workouts and for the users who posted those workouts. There is a menu bar on the top of page with "Home", "Your Workouts", "New Workout", "Exercises", "New Exercises" and "Profile" options. Clicking on the "Your Workouts" option brings you to a page that shows all of your workouts. You can then click on each individual workout which will bring you to a "show" page for that workout, or you can click on a particular exercise which will bring you to the "show" page for that exercise. The "New Workout" option allows you to enter a new workout, if you hadn't guessed that already.. You are able to choose from exercises that have already been entered in the database and create new exercises as you make your new workout. The next option on the menu bar is the "Exercises" option. This brings you to page with all exercises that have already been entered into the database. There is a hyperlink for each one, which brings you to the "show" page for that exercise. "New Exercises" option on menu, allows you to enter up to five exercises simultaneously, including description and exercise type attributes. Finally, you have the "Profile" option which brings you to your profile page. You can add a bio for yourself on this page.

As a casual user of "Workout Logger" there has to be restrictions as far as what you can edit or delete right? A casual user can edit or delete their workout and they can add or edit their own bio. They can edit exercise attributes but they can't delete exercises, because other users may be using those exercises on their workouts as well. Only the admin can delete the exercises. 

I must say, I had a lot of fun working on this project. It's still pretty basic as far as functionality but I may add even more functionality in the future when I have time. The most challenging parts were keeping the code nice and clean, like implementing iteration to create objects instead of repeating code in certain controllers to accomplish the same thing. I really enjoyed making an CRUD MVC application, that said, I am looking forward to learning new things in whatever is next in the curriculum!


