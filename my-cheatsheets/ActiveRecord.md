
#Active Record
Active Record is an Object-Relational Mapping system, or ORM.

## Methods
Dog.all returns an array of all the dog objects (rom 0 to many)
Dog.where("name like '%Jada%'") returns an array (from 0 to many)
Dog.find(1) 1 or 0 elements
Dod.find_by_id(1) 1 or nil

jada.update_attributes( {weight: 33} )
tenley.destroy

[Active Record Cheat sheet](https://gist.github.com/jessieay/3131622#file-ActiveRecord Cheat Sheet v1)
[Finder methods - API](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-find_by)
[Query methods - API](http://edgeapi.rubyonrails.org/classes/ActiveRecord/QueryMethods.html)
[Finding data](https://www.safaribooksonline.com/library/view/learning-rails/9780596154943/ch04s04.html)
[Finding - Querying](http://culttt.com/2015/12/02/querying-with-active-record-in-ruby-on-rails/)
[Calculations](http://api.rubyonrails.org/classes/ActiveRecord/Calculations.html)

## Associations
###Jockey-entry-race
![](https://github.com/chi-red-pandas-2016/active-record-associations-drill-at-the-races-challenge/blob/master/races_schema.png)
```ruby
class Jockey < ActiveRecord::Base
  has_many :entries
  has_many :entered_races, through: :entries, source: :race
  has_many :ridden_horses, through: :entries, source: :horse
end
```

```ruby
class Entry < ActiveRecord::Base
  belongs_to :jockey
  belongs_to :horse
  belongs_to :race
end
```

```ruby
class Race < ActiveRecord::Base
  has_many :entries
  has_many :jockeys, through: :entries
  has_many :horses, through: :entries
end
```

###Hotel-room-booking
![](https://github.com/chi-red-pandas-2016/active-record-associations-drill-hotels-challenge/blob/master/hotels_schema.png)
```ruby
class Booking < ActiveRecord::Base
  belongs_to :guest, class_name: "User"
  belongs_to :room
  has_one :hotel, through: :room
end
```

```ruby
class Hotel < ActiveRecord::Base
  has_many :rooms
  has_many :bookings, through: :rooms
  has_many :booked_guests, through: :bookings, source: :guest
end
```

```ruby
class Room < ActiveRecord::Base
  has_many :bookings
  belongs_to :hotel
  #has_many :users, through: :bookings
end
```

```ruby
class User < ActiveRecord::Base
  has_many :bookings, foreign_key: "guest_id"
  has_many :booked_rooms, through: :bookings, source: :room
  has_many :booked_hotels, through: :booked_rooms, source: :hotel
end
```

##Validations, errors and callbacks
### Validations
[Validations and callbacks on Rails doc](http://guides.rubyonrails.org/v3.2.13/active_record_validations_callbacks.html)
[Custom methods for validations](http://guides.rubyonrails.org/active_record_validations.html#custom-methods)

### Callbacks
[Callbacks on Rails doc](http://guides.rubyonrails.org/active_record_callbacks.html)

### Errors
http://guides.rubyonrails.org/active_record_validations.html#working-with-validation-errors


## Use the Rake Console
```
$ bundle exec rake console
```

  To sample what we can do from within the console, run ...

  -  `ActiveRecord::Base.connection.tables`

      This returns an array containing the names of the tables in our database.

  -  `ActiveRecord::Base.connection.columns(:dogs)`

      This returns an array of objects representing the columns in the `dogs` table—one object for each column.  The objects themselves happen to be instances of the `ActiveRecord::ConnectionAdapters::Column` class.

  -  `Dog`

      This will return the `Dog` class itself, and we will see in parentheses a list of attribute names and types that are associated with instances of `Dog`.  Active Record derives these attributes from the columns in the `dogs` table in our database.

  - `Dog.all`

      `Dog::all` is a class method that returns all of the records in the `dogs` table as instances of the `Dog` class.  The individual instances are returned within a collection, an `ActiveRecord::Relation` object that acts much like an array.  In the console output, we can see the SQL statement that was executed: `SELECT "dogs".* FROM "dogs"`.

  - `exit`

     This will exit the console, just like IRB.  Alternatively, use *control + d*.



# Sessions and user authentication
get '/dogs' do
 @dogs = Dog.all

 session[:dog_views] = 0 unless session[:dog_views] #Sinatra keeps track of the number of times I had view my dog
 session[:dog_views] += 1

 erb :index
end

## View
[Video](https://talks.devbootcamp.com/sessions-and-user-authentication) at 29:54

## Model
```ruby
class User < ActiveRecord::Base

  def  password
    @password ||= BCrypt::Password.new(encrypted_password)
  end

  def password=(new_password)
    @password = BCrypt::Password.create(new_password)
    self.encrypted_password = @password
  end
  
  def authenticate(password)
    self.password == password
  end
end
```

## Controllers
### login.rb
```ruby
get '/login' do
  erb :login
end

post '/login' do
  user = User.find_by(username: params[:username])
  if user.authenticate(params[:password]) #return true or false, whatever or not the password equals to the original
    session[:user_id] = user.id
    redirect '/dogs' #restricted area
  else
    erb :login
  end
end

get '/logout' do
  session.delete(:user_id)
  redirect '/login'
end
```

### dogs.rb
```ruby
get '/dogs' do
  redirect '/login' unless session[:user_id]

  @dogs = Dog.all

  session[:dog_views] = 0 unless session[:dog_views]
  session[:dog_views] += 1

  erb :index
end
```

##helper viewer that can be useful for debugging/inspcting:
Session.inspect give us all the data/info in the session as a hash
```ruby
get '/session-viewer' do
  session.inspect
end
```
Accessible by localhost:9393/session-viewer

#Random

## Useful links
[Encrypt passwords with bcrypt](https://github.com/codahale/bcrypt-ruby)

tasks controller:
tasks/new

## Useful Sublime shortcuts
Command + D => selects all the occureneces of an expression one by one
Command + T => find a file

be rake db:reset to drop, recreate and re-migrate the database


post '/tasks' do
title = params[:title]
desc = params[:description]
completed = false

task = Task.new({ title: title, description: desc, completed: completed})

if task.save
  redirect '/tasks/#{task.id}'
else
  @errors = @task.errors
  erb: 'task/new'
end


Errors
======
redirect "/tasks/#{task.id}?message="


@current_user ||= x
equals to
@current_user || @current_user = x
that means
@current_user ||= some_expression means use @current_user if it's non-nil or true; otherwise, assign some_expression to @current_user and then use @current_user.





