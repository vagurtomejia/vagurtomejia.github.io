
#Sinatra

[Sinatrarb (Sinatra intro)](http://www.sinatrarb.com/intro.html)
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


##Bundler

When bundler is installed, it provides a command-line utility. We can use this utility to install any necessary gems that are missing from our system (see Figure 3). Because the code base provides a Gemfile.lock file, Bundler will use this file to determine exactly which version of each gem to install. We'll begin most of our challenges going forward with this step of ensuring that all necessary gems are installed.
```
$ bundle install
```

If problems during the instalation with Active suppport later version try verion 4.2.7 (into the gemfile):
```ruby
gem 'activesupport', '4.2.7'
```



## Rakefile

Rake is a Make-like program implemented in Ruby. Tasks and dependencies are specified in standard Ruby syntax.
[More on Rakefiles](https://github.com/ruby/rake)

List all the possibilities into the Rakefile:
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



## Gemfile
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

