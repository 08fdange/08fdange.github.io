---
layout: post
title:      "Javascript/Rails API Project"
date:       2020-09-27 21:21:44 -0400
permalink:  javascript_rails_api_project
---



## My Project Idea - BurgerBites

For this project I decided I wanted to make a burger rating application. It is a single page application (per requirements) where users can register, log in and leave a rating on local burger joints based on whatever location they enter. It utilizes the Yelp API in order to populate the application with burger places. Originally, I wanted to create the application to pull your location automatically through the browser and display a map based on that location of all the burger places. Unfortunately, I didn't have enough time to create an application that complex so I simplified it a bit by adding a search/query bar where you can enter an address or zip code to search for the burger spots and a list, instead of a map, to display them. 

## The Backend Set Up

**The Rails API backend has three tables/models**:

* User table with email and password_digest columns
* Rating table with stars, place and user_id columns
* Review table with content, place and user_id columns

**Model relationships are as follows**: 

* A User has many ratings and reviews
* A Rating belongs to a User
* A Review belongs to a User

For the User model, I had to set up an authentication which included me using a few different gems to handle the user registration/login. 

**Gems I used**:

* "bcrypt"
* "jwt"
* "simple_command"

I set up register and login actions in the user controller as well as "auth" folder located in the backend "app" folder which contained two files "authenticate_user.rb" and "authorize_api_request.api". 

```
#app/auth/authenticate_user.rb

class AuthenticateUser
    prepend SimpleCommand
    attr_accessor :email, :password
  
    #this is where parameters are taken when the command is called
    def initialize(email, password)
      @email = email
      @password = password
    end
    
    #this is where the result gets returned
    def call
      JsonWebToken.encode(user_id: user.id) if user
    end
  
    private
  
    def user
      user = User.find_by_email(email)
      return user if user && user.authenticate(password)
  
      errors.add :user_authentication, 'Invalid credentials'
      nil
    end
  end
```

```
#app/auth/authorize_api_request.rb

class AuthorizeApiRequest
    prepend SimpleCommand
  
    def initialize(headers = {})
      @headers = headers
    end
  
    def call
      user
    end
  
    private
  
    attr_reader :headers
  
    def user
      @user ||= User.find(decoded_auth_token[:user_id]) if decoded_auth_token
      @user || errors.add(:token, 'Invalid token') && nil
    end
  
    def decoded_auth_token
      @decoded_auth_token ||= JsonWebToken.decode(http_auth_header)
    end
  
    def http_auth_header
      if headers['Authorization'].present?
        return headers['Authorization'].split(' ').last
      else errors.add(:token, 'Missing token')
      end
      nil
    end
  end
```

An "authenticate_request" function was added in the ApplicationController to be accessed in all other controllers. 

```
#controllers/application_controller.rb

    def authenticate_request
      @current_user = AuthorizeApiRequest.call(request.headers).result
      render json: { error: 'Not Authorized' }, status: 401 unless @current_user
    end
```

Overall, the backend wasn't too difficult to set up properly, and took very little troubleshooting. 

## Setting Up the Front End

For the assignment, we were told to organize our Javascript in Object Oriented classes. These were the js files I created in the "src" file: 

* "index.js" - Responsible for handling most of whats happening on the application, including many event listeners
* "fetch.js" - Responsible for handling get/post fetch requests
* "place.js" - Handles creation of place js model after receiving information for the model attributes from the Yelp API
* "user.js" - Handles creation of user model in js, sets currentUser throughout the front end
* "rating.js" - Handles creation of rating js model. These models get their attributes from the back end. 

The front end was challenging and took a lot of debugging to get everything to display properly. Registration/Login system was probably less ideal based on the requirements of the project and probably could have implemented better with use of a front end framework so that took a decent chunk of my time to get working properly. I also have some styling with bootstrap 4 and stars display for the ratings of each place using icons from Font Awesome. A little basic math in a static function located in the Rating js model to determine how many stars should be displayed based on a percentage (stars were displayed through CSS). Due to time constraints as well as preference, I chose to keep a rather clean interface for the application, although I would like to spend a little more time fine tuning the styling of the application.

### Conclusion

Overall, I had a fun time building this project. It definitely put some challenges in front of me that I hadn't faced previously in the curriculum and I learned a lot working on it. I'd love to spend some additional time working on the project and making it even better than it is now, and eventually implenting my original idea of the map interface instead of the list. 






