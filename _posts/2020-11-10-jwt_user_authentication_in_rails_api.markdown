---
layout: post
title:      "JWT User Authentication in Rails API"
date:       2020-11-10 20:41:08 -0500
permalink:  jwt_user_authentication_in_rails_api
---


I've decided to provide my own guide for setting up a basic token-based user authentication system for your Rails API backend. I'd like to make it as simple and straight forward as possible for those that would like to add authentication to their app without spending too much time fiddling around with it.

**Create your new Rails API-only application**

This is as simple as running the following in your terminal/command prompt: 

```
rails new your-app-name --api
```

Don't forget your api flag!

**Add required gems**

We'll be using "jwt", "bcrypt", and "simple_command" gems. The "bcrypt" gem should already be present in your gemfile, you just need to make sure you uncomment it. The other two need to be added manually.

```
#Gemfile.rb

# Use Active Model has_secure_password
gem 'bcrypt', '~> 3.1.7'
# JWT Token generation
gem 'jwt'
# SimpleCommand to build Service Objects
gem 'simple_command'
# Foreman utility for managing multiple processes
gem 'foreman'
```

**Create your User model**

Next, we'll need to create a User model: 

```
rails g model User email username password_digest
```

Now we need to run the migrations:

```
rails db:migrate
```

Go into your User.rb model file and add "has_secure_password" :

```
#app/models/User.rb

class User < ApplicationRecord
    has_secure_password
end

```

**Encode/Decode JWT Tokens with JWT Singleton Class**

We will need to create a singleton class, which is a special type of class that only creates one instance of class to a single object. Create a file in your "lib" folder called "json_web_token.rb":

```
#lib/json_web_token.rb

class JsonWebToken
    class << self
        def encode(payload, exp = 24.hours.from_now)
            payload[:exp] = exp.to_i
            JWT.encode(payload, Rails.application.secrets.secret_key_base)
        end
    
    def decode(token)
        body = JWT.decode(token, Rails.application.secrets.secret_key_base)[0]
        HashWithIndifferentAccess.new body
    rescue
        nil
    end
   end
end
```

This provides with the encode and decode methods. Encode authenticates the user and creates a JWT token, which is really just encoded string of letters and numbers. Decode does what think it might, decodes the token that is received in each request. 

**Authentication of the User**

This is where will be utilizing the "simple_command" gem, to create services that which will act as middleman between our controller and model, which we leave both the model and controller clean as could be. 

```
#app/commands/authenticate_user.rb

class AuthenticateUser
    prepend SimpleCommand

    def initialize(email, password)
        @email = email
        @password = password
    end

    def call
        JsonWebToken.encode(user_id: user.id) if user
    end

    private

    attr_accessor :email, :password

    def user
        user = User.find_by(email: email)
        return user if user && user.authenticate(password)

        errors.add :user_authentication, 'invalid credentials'
        nil
    end
end
```

This command takes in the user's email address and password and looks for an instance of User model. If found, it authenticates the password, takes the id of that instance of user and plugs it into the JsonWebToken's encode method. We end up with a generated Json Web Token.

**Checking JWT Validity **

We will now be setting up another command to check if the token sent through the authorization header is valid by decoding it. 

```
#app/commands/authorize_api_request.api

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
      else
        errors.add(:token, 'Missing token')
      end
      nil
    end
  end
```

In this command (who's use is made possible by the "simple_command" gem, once again) we extract the token from the authorization header, decode the token to get the user's id, and then check if the user exists in the database based on that id.

**Controller Logic**

With our authentication system set up we now need to put it to use in the controllers. We start by creating an AuthenticationController, which handles the user's login through the use of our AuthenticateUser command.

```
#app/controllers/authentication_controller.rb

class AuthenticationController < ApplicationController

    def authenticate
        command = AuthenticateUser.call(params[:email], params[:password])

        if command.success?
            render json: { auth_token: command.result, user: {
                email: params[:email], 
                username: User.find_by(email: params[:email]).username
                }}

        else
            render json: { error: command.errors }, status: :unauthorized
        end
    end
    
end
```

In this controller, the AuthenticateUser command is being called with the user's email and password. If successful, a token as well as user information will be sent back in the request. If not, an "unauthorized" error will be sent back. 

We will also add some logic in the ApplicationController: 

```
#app/controllers/application_controller.rb

class ApplicationController < ActionController::API
    attr_reader :current_user

    private

    def authenticate_request
        @current_user = AuthorizeApiRequest.call(request.headers).result
        render json: { error: 'Not Authorized' }, status: 401 unless @current_user
    end
end

```

**Setting up a route for user authentication**

Our last step in this guide will be setting up a route to authenticate users: 

```
#config/routes.rb

post 'users/auth', to: 'authentication#authenticate'
```

**Conclusion**

After this basic JWT authentication set up you will need to decide what logic you'd like to put in the UserController based on what you will what a logged in or logged out user to have access to. This goes for any other models you may decide to put into your application as well. You may also want to add some validations to your User model. Anyways, I hope this helped! 


