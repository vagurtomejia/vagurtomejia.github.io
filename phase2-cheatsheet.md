
#Active Records
Active Record is an Object-Relational Mapping system, or ORM.

When bundler is installed, it provides a command-line utility. We can use this utility to install any necessary gems that are missing from our system (see Figure 3). Because the code base provides a Gemfile.lock file, Bundler will use this file to determine exactly which version of each gem to install. We'll begin most of our challenges going forward with this step of ensuring that all necessary gems are installed.
```
$ bundle install
```

## Associations
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

### Rakefile
List all the possibilities into the Rakefile
```
$ bundle exec rake -T
```
OR 
rake -T
*Figure 1*.  Listing available Rake tasks.

### Release 1:  Create the Database
```
$ bundle exec rake db:create
```
### Release 2:  Check Database Version
```
$ bundle exec rake db:version
```
*Figure 5*.  Executing the Rake task for checking the database version.

### Release 3:  Run the Test Suite
```
$ bundle exec rake spec
```
*Figure 6*.  Executing the Rake task for running the test suite.

### Release 4:  Run the Migrations
```
$ bundle exec rake db:migrate
```
*Figure 7*.  Executing the Rake task for running the database migrations.

### Release 7:  Populate the Database with Seed Data
```
$ bundle exec rake db:seed
```
*Figure 8*.  Executing the Rake task to seed our database.

### Release 8:  Open and Use the Console
```
$ bundle exec rake console
```
*Figure 9*.  Executing the Rake task to open the console.

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

### Release 9:  Rollback the Database
```
$ bundle exec rake db:rollback
```
*Figure 10*.  Executing the Rake task to undo the last migration.

### Release 10:  Drop the Database
```
$ bundle exec rake db:drop
```
*Figure 11*.  Executing the Rake task to drop the database.

# Rakefile
Rake is a Make-like program implemented in Ruby. Tasks and dependencies are specified in standard Ruby syntax.


[More on Rakefiles](https://github.com/ruby/rake)

# Gemfile
What is a Gemfile?
A Gemfile is a file we create which is used for describing gem dependencies for Ruby programs. A gem is a collection of Ruby code that we can extract into a “collection” which we can call later.

Your Gemfile should always be in the root of your project directory, this is where Bundler expects it to be and it is the standard place for any package manager style files to live.

It is useful to note that your Gemfile is evaluated as Ruby code. When it is evaluated by Bundler the context it is in allows us access to certain methods that we will use to explain our gem requirements.

A gemfile is writtend as follows:

```ruby
source :rubygems

gem 'activerecord'
gem 'sqlite3'
gem 'faker'
gem 'rspec'
```

[More on Gemfiles](http://tosbourn.com/what-is-the-gemfile/)

#Sinatra
## Definition
<a href="http://www.sinatrarb.com" target="_blank">Sinatra</a> is a small
Domain-Specific language (or DSL; a "language" that is used for a specific
purpose) for creating web applications quickly.

## Beginning
If you haven't already, install the sinatra gem:

```bash
$ gem install sinatra -v '~> 1.4.6'
```

> :flashlight: *What's with that `~>` syntax? We're telling `gem` to install sinatra 
> version greater than or equal to `1.4.6`, but limiting which versions it will 
> install to patch releases greater than `1.4.6`. 

Create a new ruby file (let's call it `sinatra.rb`, but it could really be
called anything):

```bash
$ touch sinatra.rb
```

and open it in your editor. At the top of this file, we'll need to include the
sinatra library.

```ruby
require 'sinatra'
```

## Rendering

### What's get rendered?

Given a route, it's the last line of the block that Sinatra tries to render to the browser.

### ERB
* ERB is a templating language supported by Sinatra
* erb is a method that is most commonly called with a symbol (:about in this case). It executes ruby code contained in special tags.

* Sinatra's erb method returns a string, which means we can run it at the end of a route block and expect the method's output to render to the browser.

##Priorities
The Sinatra objects (into app.rb) are prioritized top down!! Be careful with that!

##Controller-Views scope
An instance variable set in a route's block will be available to the view that has been rendered by that block. That's a pretty complicated sentence

## URL Parameters

```ruby
get '/greetings/:name' do
  params[:name]
end
```

In
<a href="http://localhost:9393/greetings/eve" target="_blank">http://localhost:9393/greetings/eve</a>.
The browser will respond with the parameter sent in. In that case "eve" will be displayed into the browser.

Notice that we used `params` in the route body. `params` is a hash that Sinatra
provides to allow access to data that has been sent from the browser. Also
notice that the `:name` portion of the route definition (`get '/greetings/:name'`)
is the same as the `:name` key of the `params` hash.

Since `params` is a hash, we can use standard ruby code such as string
interpolation to manipulate or display information that has been sent in. Let's
modify the previous route to be a little more friendly.

```ruby
get '/greetings/:name' do
  "Hey #{params[:name]}!"
end
```

You can also add multiple parameters to be replaced in a route:

```ruby
get '/cities/:city/greetings/:name' do
  "Hey #{params[:name]}! Welcome to the #{params[:city]} greeting page!"
end
```

Sinatra calls these "named parameters," since it uses the text after the symbol
as that parameter's key in the `params` hash. When accessing a route, you can
replace the named parameters with text.

These will all match the route we've defined above:

<a href="http://localhost:9393/cities/Chicago/greetings/Michael" target="_blank">http://localhost:9393/cities/Chicago/greetings/Michael</a><br>
<a href="http://localhost:9393/cities/Los%20Angeles/greetings/Anne" target="_blank">http://localhost:9393/cities/Los%20Angeles/greetings/Anne</a><br>
<a href="http://localhost:9393/cities/Pluto/greetings/mickey" target="_blank">http://localhost:9393/cities/Pluto/greetings/mickey</a><br>

## POST method

A convenient way for users to send information to our site is by using an HTML
form. Create a view file called `views/greetings.erb` and add a form:

```html
<h1><%= @greeting %>, friend!</h1>

<form action="/custom_greetings" method="post">
  <input type="text" name="greeting">
  <input type="submit">
</form>
```

> :flashlight: Notice that this view is displaying something on the page using
> ERB tags as well as displaying the form.

Add a new route to your controller file that will render the view you just
created:

```ruby
get '/greetings' do
  erb :greetings
end
```

When you visit your <a href="http://localhost:9393/greetings" target="_blank">greetings page</a>,
you should see an input with a submit button as well as a pretty ugly greeting.

![snitch7](snitch1-7.png)

In order for Sinatra to respond to the route our form is POSTing data to, we
need to add a route that matches it. Add the following route to `sinatra.rb`:

```ruby
post '/custom_greetings' do
  @greeting = params[:greeting]
  erb :greetings
end
```

For now, we're just going to render the same view again, `greeting.erb`.

![snitch9](snitch1-9.png)








layout.erb - What is not going to change between all the pages of the site is inside this file, it's the 'overall layout', IT IS ALWAYS THERE

index.erb -_ it's the body that changes between all the pages, IT CHANGES

index.erb get rendered inside the 'yield' field present into layout.erb