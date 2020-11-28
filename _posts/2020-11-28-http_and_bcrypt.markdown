---
layout: post
title:      "HTTP and bcrypt"
date:       2020-11-28 18:19:50 -0500
permalink:  http_and_bcrypt
---


HTTP is a stateless protocol, meaning that it treats each request independently and thus does not hold any information from any previous requests. Because of this, it cannot remember a person's identity, preferences, or requests from one page to the next. Because of this, cookies and sessions were created. 

A 'cookie' is a hash that is stored in the users web browser and sent back to the server with any request. A 'session' on the other hand is an object that stores data based on a user's interaction with a website on the websites server. When a user sends an HTTP request to a website, the application will generate and store data about the user which is kept in the session hash, this data is also placed in a cookie which is sent back to the user to be stored in the browser. When the user returns to the website the cookie data is compared with the session data which allows the application to determine what information to load onto the page, giving each user a personalized experience. This is how websites are able to persist the data of each individual user. 

Because HTTP is a stateless protocol, we use two different type of cookies to interact with websites, session cookies which allows a user to navigate a website without having to re-authenticate whenever a new page loads on the website, and persistent cookies that allow websites to recall all the user information on any future visit.

To use sessions, a programmer making a Sinatra app can add the following code to their application controller in the configure block:

```
  configure do
    ...
    enable :sessions
    set :session_secret, "secret"
  end
```

(please note, session_secret should never be set by hand and instead use a secure random number generator to create it)

The first line, enable :sessions, turns the session on while the next line is the encryption key that is used to create a session_id.

To keep track of users throughout a session, the programmer will set up a session hash to store the users id, for example:

```
    get '/index' do 
      @session = session
    end
```

This allow the /index route to be processed by a GET request and because sessions are enabled, every controller action will have access to the session hash which will look something like this:

```
    @session = {
      "session_id"=>  
        "dd32f512ee239ad74aa6f10c8cad37ce28d6c6922eff252ed641b1017130fe22", 
      "csrf"=> "040e9777d4dfae03bb1e6498f2a75482", 
      "tracking"=>{ 
        "HTTP_USER_AGENT"=> "e193e9e937caa9a19ca483f046281aae77d2216b", 
        "HTTP_ACCEPT_LANGUAGE"=> "66eae971492938c2dcc2fb1ddc8d7ec3196037da"
      }
    }
```

Moving on, in my Sinatra app, I used the bcrypt gem to secure user data. Bcrypt is a hashing algorithm that takes sensitive data and creates a 'digit fingerprint,' or hash, out of it. This process is not reversible so there is no way to get the data back from the hash. Because hackers can penetrate the databases used to store these hashes of sensitive data, then just run lists of possible passwords through the same algorithm to look up the passwords by their hash, salts were introduced. A salt is a small chunk of random data that is added to the password before it is hashed. This salt is then stored with the hash in the database and used to check valid passwords. By adding a salt, an attacker then has to have a huge database for each unique salt, which requires more storage space than is likely feasible for attackers. The Ruby gem bcrypt was designed to automatically handle the process of hashing and salting  passwords.

In my Sinatra final project, I used bcrypt to add security to sensitive user information. To use bcrypt, I added a 'password_digest' column to the users table to store the salted password hashes. The app also needs to have 'has_secure_password' added to the user model. This will allow bcrypt to handle user creations and automatically convert given passwords into salted hashes which are stored in the password digest. 

Bcrypt also provides various methods to allow the app to authorize users. I used the bcrypt 'authenticate' method to validate users on login as seen from my apps login POST request:
```
  post '/login' do
    user = User.find_by(:username => params[:username])
    if user && user.authenticate(params[:password])
        session[:user_id] = user.id
        redirect to "/dashboard"
    else
        flash[:error] = "Sorry, password/username does not match"
        redirect to '/login'
    end
  end
```

This method checks the authentication of the user and if valid stores their identity in the session cookie. The POST request first tries to find the user based on the username, if the user exists then bcrypt's built in 'authenticate' method authenticates them using the password provided in the form. The 'authenticate' method works by returning a false value if the password does not match, otherwise it returns the user instance and the next line of code will store their id in the session cookie before redirecting them to the user dashboard. 

This blog gave a technical overview on why my app required sessions to persist data due to HTTP being a stateless protocol and went into the details of how I used bcrypt to secure user passwords and authenticate the user on login. 
