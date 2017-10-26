## Rails Authentication with JSON Web Token

This is a fork from part of a tutorial article about authentication with JWT tokens in rails
You can view the tutorial [here](http://tutorials.pluralsight.com/ruby-ruby-on-rails/token-based-authentication-with-ruby-on-rails-5-api)

## Docker Setup

1. I've added docker files, so you should be able to get going using: 

$ sudo docker-compose up --build

2. Don't forget to run your migrations:

$  sudo docker-compose exec --user "$(id -u):$(id -g)" website rails db:migrate

## Test

1. Create a user from the rails console:

$ sudo docker-compose exec --user "$(id -u):$(id -g)" website rails c

2. Use CURL to authenticate:

$ curl -H "Content-Type: application/json" -X POST -d '{"email":"example@mail.com","password":"123123123"}' http://localhost:3000/authenticate

Your token will be returned: 

{"auth_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE0NjA2NTgxODZ9.xsSwcPC22IR71OBv6bU_OGCSyfE89DvEzWfDU0iybMA"}

4. Request a resource:

$ curl http://localhost:3000/items

You should get an error:

{"error":"Not Authorized"}

5. Request resource with your auth token:

$ curl -H "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE0NjA2NTgxODZ9.xsSwcPC22IR71OBv6bU_OGCSyfE89DvEzWfDU0iybMA" http://localhost:3000/items 
[]


With the token prepended, an empty array ([]) is returned. This is normal -- after you add any items, you will see them returned in the request.

Awesome! Everything works.


## Original Setup

* Install RVM https://rvm.io/rvm/install

* Use RVM to install and use Ruby version 2.3.0

```
rvm install 2.3.0
rvm use 2.3.0
```

* Install Gems

```
bundle install
```

* Migrate into database

```
rails db:migrate
```

* Run the server

```
rails s
```

* Open a new Terminal.app tab and create a new user using Rails Console

```
rails console

User.create(:email => 'example@mail.com', :password => '123', :password_confirmation => '123')
```

* Open another Terminal.app tab and use cURL to perform a JWT HTTP POST request to back-end

```
curl -H "Content-Type: application/json" -X POST -d '{"email":"example@mail.com","password":"123"}' http://localhost:3000/authenticate
```
