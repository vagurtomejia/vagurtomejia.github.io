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

##Complete example with post request
    $(document).ready(function() {
      // This is called after the document has loaded in its entirety
      // This guarantees that any elements we bind to will exist on the page
      // when we try to bind to them

      // See: http://docs.jquery.com/Tutorials:Introducing_$(document).ready()
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
##public/js/application.js
    $(document).ready(function() {
      $('form').submit(function(e) {
        
        e.preventDefault();//otherwise javascript will make the post and the browser will ALSO make the post
        $.post("/rolls"); jQuery helper function that will send an AJAX post request
        //we can also use put, get, .. or:
        $.ajax("/rolls", {method: "put"}) 
        //to handle responses we use rollbacks:
        $.post("/rolls", function(response) {
            //debugger;
            res = $(response);//this parses the string (where I have all my html) into a DOM
            var die = $(response).find("#die");
            $("#die").html(die.html());//set the new html
            //or finally:
            $("#die").html(response);
          });
      });

    });

##app/controllers/index.rb
In the post '/rolls' route:

if request.xhr?
  %()//get the image that we display in the index inside the string: <img src="">...
else
  erb :index
end

##Ajax livecode example

[Ellie breakout on Ajax](https://github.com/chi-red-pandas-2016/sinatra-skeleton-mvc-challenge/compare/example...chi-red-pandas-2016:ellie-ajax-jquery-livecode)