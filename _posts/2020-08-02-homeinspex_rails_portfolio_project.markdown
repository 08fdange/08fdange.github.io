---
layout: post
title:      "HomeInspex Rails Portfolio Project"
date:       2020-08-02 20:46:01 -0400
permalink:  homeinspex_rails_portfolio_project
---


For my Rails Portfolio Project I decided to make an application that could be used by clients and inspectors for home inspections. I started out by figuring out what models I would need and their relationships by making this little web graph of sorts on my iPad. 

https://imgur.com/vExxPt8

Once I had that figured out I looked into what gems I would use for my project. I decided to use the devise gem for authentication. I had to customize a few things to get devise to work for both user models (Client and Inspector) without any cross model visits. Here's what I did: 

1. I generated both models with "rails g devise <model>" in my command prompt.
2. I assigned both models routes using "devise_for" in the routes.rb file.
3. I exposed the devise controllers so I could customize registration and sessions controllers for each model.
4. I specified registration and sessions controllers in the "devise_for" routes for each model.
5. I created a file in the "controllers/concerns" called "accessible.rb". In the file is a module that defines a method called check_user. It checks whether you are a current_client or current_inspector and if you are, it redirects to the home page with a notice. The concern uses a before_action to make it work automatically after it is included in the proper controllers.
6. I then included the Accessible concern the registration and sessions controllers for each user model and used "skip_before_action" for the "destroy" action in the sessions controllers and "new/create" actions in the registration controllers. This is done to prevent the redirects from happening before those actions can occur.

After figuring a work around for the cross model visits, I added omniauth for the Client model, made a migration for the Inspection model with all the attributes of an inspection, defined the model relationships in each model.rb file, and created all the rest of routes needed. I then worked on the views, forms, added bootstrap for styling, worked on the user interface, added validations, worked on displaying errors, etc. It was a really fun project and I think it ended up looking fairly aesthetic. Here's a picture of the homepage:

https://imgur.com/2EDhLTU


Really looking forward to future challenges! 

