#CRUD
## What is it?
Behaviors for interacting with our data in our database
Create, Read, Update, Delete data.




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

##ERB template files
layout.erb - What is not going to change between all the pages of the site is inside this file, it's the 'overall layout', IT IS ALWAYS THERE

index.erb -_ it's the body that changes between all the pages, IT CHANGES

index.erb get rendered inside the 'yield' field present into layout.erb

## Params
### Retrieving params for a instance creation:
- In the view:
<form action="/dogs" method="post">
  <label for="name">Name:</label>
  <input type="text" name="dog[name]" value="" />
  <br>

  <label for="age">Age:</label>
  <input type="text" name="dog[age]" value="" />
  <br>

  <label for="age">Weight:</label>
  <input type="text" name="dog[weigth]" value="" />
  <br>

  <input type="submit" value="Add dog!">

</form>

- In the controller:

post '/dogs' do
  Dog.create(params[:dog])
  redirect '/dogs'
end

### Inspect params
raise params.inspect


## Faking a PUT request
GET edit, then PUT
- Viewer side:
```html
<a href="/dogs/<%= @dog.id %>/edit">Edit this dog</a>
```
- Controler side: 
get '/dogs/:id/edit' do
  @dog = Dog.find(params[:id])
  erb :edit
end

then,

- Viewer side:
<form action="/dogs/<%= @dog.id %>" method="post">
    <input type="hidden" name="_method" value="put">

  <label for="name">Name:</label>
  <input type="text" name="dog[name]" value="<%= @dog.name %>" />
  <br>

  <label for="age">Age:</label>
  <input type="text" name="dog[age]" value="<%= @dog.age %>" />
  <br>

  <label for="age">Weight:</label>
  <input type="text" name="dog[weigth]" value="<%= @dog.weigth %>" />
  <br>

  <input type="submit" value="Update dog!">

</form>

- Controller side:

put '/dogs' do
  Dog.create(params[:dog])
  redirect '/dogs'
end


Controler side:
put '/dogs/:id' do
  dog = Dog.find(params[:id])
  dog.update(params[:dog])

  redirect "/dogs/#{@dog.id}"
end

## Faking a DELETE request
```html
<form action="/dogs/<%= dog.id %>" method="post">
  <input type="hidden" name="_method" value="delete">
  <input type="submit" value="Kill the dog!">
</form>
```
Controler side:
delete '/dogs/:id' do
  @dog = Dog.find(params[:id])
  @dog.destroy

  redirect '/dogs'
end

## Partial views
<%= erb :_form_fields %>
[Partials Cheat-sheet](https://www.learnhowtoprogram.com/lessons/partials-in-sinatra#cheat-sheet)


#REST
## What is it?
- stands for:
REpresentational
State
Transfer
- It's not a formal protocol, but rather an "architectural style"
- A **convention** for mapping CRUD actions to HTTP requests
- It's stateless

## How do you implement these RESTful routes in your application?
- Follow the conventions to make your code more readable, your routes easier to understand and follow!

- [Test your REST](www.restular.com)
## More about REST
[REST](https://gist.github.com/case-eee/72715407554996828e0c#file-rest-md)


![Imgur](http://i.imgur.com/0Zu9rHR.png)


#Interesting ressources
[HTTP protocol](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177)







