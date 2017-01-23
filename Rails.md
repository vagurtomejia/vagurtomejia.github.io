#Rails

##see the options of rails:
rails

##creates the rake and file structures (we use -d for soecifying the type of the database - sqlite by default )
rails new -d postgresql hackernews

##generate models (unlike Sinatra, the generate models command creates the tables also)
rails g model User (unlike Sinatra this creates the model also)

##generate migrations (like rake generate:migration on Sinatra)
rails generate migration create_posts

##set the models and migrations
like we used to do in sinatra in /models and /db/migrate

##create the database
rake db:create

##run the migrations
rake db:migrate

##run the console for verifying if the models were created
rails console

##run the rails server locally (equivalent of be shotgun in sinatra)
rails s

##define the method in the controller class



##define the routes in the config/routes.rb file
root 'posts#index'
get 'posts' => 'posts#index'

##create the views:
views/posts/index.html.erb

##
rake routes


get 'post'




##useful stuff
er [TAB]
