#AJAX
[Ajax documentation](http://api.jquery.com/jquery.ajax/)
##Ajax method
     $.ajax({
       method: $form.attr('method'), // Good to do for GET and POST
       url: $form.attr('action'),  
       data: postData
     })
     .done( function( responseHtml ) {
       $form[0].reset();
       $("#band-list").append( responseHtml );
     });

##Using JSON
###Controller side
####Sending JSON
```ruby
content_type :json
    { new_points: post.points, title: 'my title' }.to_json
```

###JS side
####Recieving JSON
```js
.done(function(response) {
      var points = response.new_points;
    });
```

####Sending JSON
```js
var putData = {
     seen_show: true
   };

   $.ajax({
     method: 'PUT',
     url: $form.attr('action'),  
     data: putData
   })
```

##Event delegation
(JQuery doc)[https://learn.jquery.com/events/event-delegation/]

Example of delegated event:

```js
// Attach a delegated event handler
$( "#list" ).on( "click", "a", function( event ) {
    event.preventDefault();
    console.log( $( this ).text() );
});
```
Notice how we have moved the a part from the selector to the second parameter position of the .on() method. This second, selector parameter tells the handler to listen for the specified event, and when it hears it, check to see if the triggering element for that event matches the second parameter. In this case, the triggering event is our anchor tag, which matches that parameter. Since it matches, our anonymous function will execute. We have now attached a single click event listener to our `<ul>` that will listen for clicks on its descendant anchors, instead of attaching an unknown number of directly bound events to the existing anchor tags only.

##Errors management
###server side
```ruby
  if new_post.save
    status 200 #succes
    erb :_article,
      :layout => false,
      :locals => {post: new_post}
  else
    status 422
  end
```

##Using partials
```ruby
    erb :_band_li, 
      :layout => false, 
      :locals => {band: new_band} #band is the variable used into the partial
                                  #new_band the variable used in the controller or viewer that calls the partial
```  

##Managing errors for requests in Sinatra
```ruby
  post '/posts' do
    #logic for attempting to save a post.
    if @post.save
      status 200 #succes
      erb :_post, :layout => false
    else
      status 422
    end
  end
```

##DBC Challenges
- [Cheering mascot](https://github.com/chi-red-pandas-2016/cheering-mascot-sinatra-2-asynchronous-forms-challenge/tree/solo_vagurtomejia)
- [Lucky Ajax](https://github.com/chi-red-pandas-2016/lucky-ajax-challenge/tree/solo_vagurtomejia)
- [Hacker news](https://github.com/chi-red-pandas-2016/ajaxifying-hacker-news-challenge/tree/solo_vagurtomejia)
- [Horses](https://github.com/chi-red-pandas-2016/ajax-checkpoint-challenge/tree/solo_vagurtomejia_final)

###Cheering mascot
```js
$(document).ready(function() {

  $('#cheer_caller').on('submit', function(event) {
    event.preventDefault();
    var $form = $(this);
    var method = $form.attr('method');
    var url = $form.attr('action');
    var postData = $form.serialize();

    var request = $.ajax({
      method: method,
      url: url,
      data: postData
    });

    request.done( function( msg ) {

       $(".sign-text span").text(msg);
       $form[0].reset();
       $('input[name="cheer_name"]').focus();

     });

  });
});
```

```ruby
post '/cheers' do
  if request.xhr?
    Mascot.sign_for(params[:cheer_name])
  else
    redirect "/?sign_text=#{Mascot.sign_for(params[:cheer_name])}"
  end
end
```

###Lucky Ajax
```js
$(document).ready(function () {

  $('form').on('submit', function(event) {
    event.preventDefault();
    var $form = $(this);
    var method = $form.attr('method');
    var url = $form.attr('action');
    var data = $form.serialize();

    $.ajax({
      url: url,
      type: method,
      data: data
    })
    .done(function(responseNumber) {
      var $container = $("#die-container");
      var dieDivHtml = '<div class="die"><span class="roll">'+responseNumber+'</span></div>';
      $container.html(dieDivHtml);

    })
    .fail(function() {
      console.log("error");

    });

  });

});
```

Controller side:
```ruby
post '/rolls' do
  @die = Die.new(params[:sides].to_i)

  if request.xhr?
    @die.roll.to_s
  else
    erb :index
  end
end
```

###Hacker news
javascript:

```js
$(document).ready(function() {

  //Vote for a post (event delegation)
  $('div.post-container').on('click', 'button.vote-button', function(event) {
    event.preventDefault();
    var $button = $(this);
    var $article = $button.closest('article')
    var $form = $article.find('form');

    var method = $form.attr('method');
    var action = $form.attr('action');

    $.ajax({
      url: action,
      type: method,
      //data: {param1: 'value1'},
    })
    .done(function(responsePoints) {
      console.log("success");
      //update the votes count
      var $points = $article.find('span.points');
      $points.text(responsePoints);

      //update the color of the button
      $button.css('background-color', 'red');


    })
    .fail(function() {
      console.log("error");
    });

  });


  //Delete a post (event delegation)
  $('div.post-container').on('click', 'a.delete', function(event) {
    event.preventDefault();

    var $link = $(this);
    var url = $link.attr('href');
    method = 'delete';

    $.ajax({
      url: url,
      type: method
    })
    .done(function(response) {
      console.log("success");
      if(response != 'fail'){
        //remove this article node from the DOM
        var articleSelector = 'article#' + response;
        $(articleSelector).remove();
      }
    })
    .fail(function() {
      console.log("error");
    });

  });


  //Add  a new post
  $('#posts').on('submit', function(event) {
    event.preventDefault();

    var $form = $(this);
    method = $form.attr('method');
    action = $form.attr('action');
    var data = $form.serialize();
    console.log(method);

    $.ajax({
      url: action,
      type: method,
      data: data
    })
    .done(function(responseHtml) {
      console.log("success");
      $('div.post-container').append(responseHtml);

    })
    .fail(function(jqXHR, textStatus, errorThrown) {
      console.log(textStatus, errorThrown);
    });


  });
});
```

```ruby
post '/posts/:id/vote' do
  post = Post.find(params[:id])
  post.votes.create(value: 1)
  if request.xhr?
    post.points.to_s

  else
    redirect "/posts"
  end
end

delete '/posts/:id' do
  post = Post.find(params[:id])
  destroyed_post = post.destroy;
  if(destroyed_post)
    destroyed_post.id.to_s;
  else
    "fail"
  end

end

post '/posts' do
  new_post = Post.new( title: params[:title],
               username: Faker::Internet.user_name,
               comment_count: rand(1000) )
  if new_post.save
    status 200 #succes
    erb :_article,
      :layout => false,
      :locals => {post: new_post}
  else
    status 422
  end

end
```

###Horses
javascript:

```js

$(document).ready(function() {

  //load the new horse form dynamically
  $('#new-horse-button-form').on('submit', function(event) {
    event.preventDefault();
    /* Act on the event */
    var $buttonForm = $(this);
    var action = $buttonForm.attr('action');
    var method = $buttonForm.attr('method');

    $.ajax({
      url: action,
      type: method
    })
    .done(function(responseHtml) {
      console.log("success");
      $buttonForm.closest('div').append(responseHtml);
      $buttonForm.hide();
    })
    .fail(function() {
      console.log("error");
    });

  });


//Update the list of horses after Submitting the Form form a new horse
$('#horses-div').on('submit', 'form#new-horse-form', function(event) {
  event.preventDefault();
  /* Act on the event */

  $('section.errors').remove();

  var $newHorseForm = $(this);
  var action = $newHorseForm.attr('action');
  var method = $newHorseForm.attr('method');
  var postData = $newHorseForm.serialize();

  $.ajax({
    url: action,
    type: method,
    data: postData,
  })
  .done(function(responseHtml) {
    console.log("success");
    $('#horses-div').find('ul').append(responseHtml);
    $newHorseForm.hide();
    $('#new-horse-button-form').show();

  })
  .fail(function(jqXhr, textStatus, errorThrown) {
    console.log("error");
    //console.log(jqXhr.responseText);
    $newHorseForm.before(jqXhr.responseText);
  });


});

//Dynamically Load Horse Details by clicking on the horse link
$('#horses-list').on('click', 'a', function(event) {
  event.preventDefault();
  /* Act on the event */

  var $horseLink = $(this);
  var href = $horseLink.attr('href');

  $.ajax({
    url: href,
    type: 'GET',
  })
  .done(function(responseHtml) {
    console.log("success");
    $horseLink.closest('li').append(responseHtml);
  })
  .fail(function() {
    console.log("error");
  });

});


});


```

##Ajax livecode example

[Ellie breakout on Ajax](https://github.com/chi-red-pandas-2016/sinatra-skeleton-mvc-challenge/compare/example...chi-red-pandas-2016:ellie-ajax-jquery-livecode)