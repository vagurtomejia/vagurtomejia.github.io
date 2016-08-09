#jQuery
##Include the jQuery library and our custom javascript code
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Home</title>
  </head>
  <body>
    <div class="homepage-wrapper">
      <h2>Welcome to jQuery Travels - Traversing the DOM since 2006</h2>
      <p>Fly to New York today for as little as <span>$299.99</span></p>
    </div>
    <script src="jquery.min.js"></script>
    <script src="application.js"></script>
  </body>
</html>
```

## Call after the DOM is finished
jQuery(document).ready(function(){
  $("span").text("$100");
});

## Selectors
### id selector
$( "#id" )
[id jQuery doc](http://api.jquery.com/id-selector/)
### class selector
$( ".class" )
[class jQuery doc](http://api.jquery.com/class-selector/)

```html
    <div id="tours-wrapper">
      <h1>Guided Tours</h1>
      <ul id="tours">
        <correct><li class="america">
          <h2>New York, New York</h2>
          <span class="details">$1,899 for 7 nights</span>
          <ul class="vote"><correct><li><a href="#">↑</a></li></correct><correct><li><a href="#">↓</a></li></correct></ul>
        </li></correct>
        <correct><li class="europe">
          <h2>Paris, France</h2>
          <span class="details">$2,499 for 7 nights</span>
          <ul class="vote"><correct><li><a href="#">↑</a></li></correct><correct><li><a href="#">↓</a></li></correct></ul>
        </li></correct>
        <correct><li class="europe sale">
          <h2>Madrid, Spain</h2>
          <span class="details">$1,577 for 5 nights</span>
          <ul class="vote"><correct><li><a href="#">↑</a></li></correct><correct><li><a href="#">↓</a></li></correct></ul>
        </li></correct>
        <correct><li class="asia">
          <h2>Tokyo, Japan</h2>
          <span class="details">$1,999 for 5 nights</span>
          <ul class="vote"><correct><li><a href="#">↑</a></li></correct><correct><li><a href="#">↓</a></li></correct></ul>
        </li></correct>
      </ul>

      <ul class="sorting">
        <li><a href="#">America</a></li>
        <li><a href="#">Europe</a></li>
        <li><a href="#">Asia</a></li>
      </ul>
    </div>
```

###descendant selectors
select all of the li elements within the #tours list using a descendant selector.
> $("#tours  li")

###direct descendant : child slector
Let's use a direct child selector to only select the li items that are direct children of #tours.
$("#tours > li")

###multiple different items
select all tours that are from Asia and all tours that are On Sale. You'll need to use multiple selectors for this, with the classes for .asia and .sale.

$("#tours > .asia, #tours > .sale")

###pseudo-selectors
####:first
Use the :first pseudo selector to select the first tour in the list.
>$("#tours li:first")

####:even
Find the direct children li elements, and then use the :even pseudo selector to select every other li element.

> $("#tours > li:even")

##Traversing
###find()
To select all vacations from America we can use $("#vacations .america"). This or traversal with the find method:
>$("#vacations").find(".america")

###first()
Using traversal or filtering, select the first vacation li element from the list.
>$("#vacations").find("li").first()

###last()
Find the last li within #vacations using traversal 
>$("#vacations").find("li").last()

###prev()
Use traversal with the prev() method to select the vacation that is right before the last one.
>$("#vacations li").last().prev()

###parent()
Using traversal, select all tours that have a .featured class on their title by getting the parent() of featured titles.
>$(".featured").parent()

###children()
Select all the tours: $("#tours > li"). You immediately realize it can be done better, refactor this to use traversal with children().
>$("#tours").children()

##Manipulating the DOM

###Creating a DOM node
When the page loads, we'll show a message to the traveler letting them know how to book this trip. To start out, let's create a <span> node with our phone number, 1-555-jquery-air and set it to the message variable.
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');

###Adding to the DOM
####before()
Let's add the phone number immediately before() the "Book Now" button. You can check out the HTML of the rendered page by clicking on the HTML tab below.
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $(".book").before(message)

####append()
append() our <span> to the bottom of the .usa element. Let's change the code to add it there instead.
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $(".usa").append(message)

###Removing from the DOM
####remove()
Remove that "Book Now" button until we can implement it.
var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
$('.usa').append(message);
$('.book').remove()

##Acting on interaction
###Click interaction
Let's start by wrapping all of our previous code in a click handler for any <button> elements using the on() method.

$( "button" ).click(function() {
  var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
  $('.usa').append(message);
  $('button').remove();
});

or

$('button').on('click', function() {
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $('.usa').append(message);
    $('button').remove();
  });
####Remove only the clicked button
$(this).remove();

####Relative Traversing
* Add the message after() the button we click on.
var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
$(this).after(message);


* The <button> is inside a <div> tag. We don't want the message to go inside the <div> tag though, we want it to go at the bottom of the <li> element. Instead of using after(), let's change our code to find the closest() .tour element and append() the message to the bottom of it.
```html
    <li class="usa tour">
      <h2>New York, New York</h2>
      <span class="details">$1,899 for 7 nights</span>
      <div>
        <button class="book">Book Now</button>
      </div>
    </li>
```

$(this).closest('.tour').append(message);

###All the code
$(document).ready(function() {
  $('button').on('click', function() {
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $(this).closest('.tour').append(message);
    $(this).remove();
  });
});

## Fetching Data From the DOM I 
Let's add a bit more incentive to get people to book these tours by offering a discount if they book today. Create a discount variable, and then assign the discount that is stored in the data() attribute on the .tour element.
We want to show this discount to the user in the message we show after the "Book Now" button is clicked.

    var discount = $(this).closest('.tour').data('discount');
    var message = $('<span>Call 1-555-jquery-air for a $'+discount+' discount.</span>');

##Better handlers
There is a small problem with the way our on() handler is being used. Currently, it is targeting all of the <button> elements on the screen. Refactor the on() handler to only target <button> elements within a .tour by using the selector argument of the on() method.
    $(document).ready(function() {
      $('.tour').on('click', 'button', function() {
        ...

##Filters
Let's add some result filtering options to our page. We want to be able to click on a filter and highlight the corresponding tours with a .highlight class..(on-sale or .featured filters).

    $(document).ready(function() {
      $('#filters').on('click', '.on-sale', function() {
        $('.tour').removeClass('highlight');
        $('.tour').filter('.on-sale').addClass('highlight');
      });

      $('#filters').on('click', '.featured', function() {
        $('.tour').removeClass('highlight');
        $('.tour').filter('.featured').addClass('highlight');
      });
    });

##Ressources
- [jQuery Cheat Sheet](https://oscarotero.com/jquery/)
- [jQuery Event Basics][]
- [Handling Events][] (e.g., a form `"submit"` event)
- [event.preventDefault()][] (e.g., to prevent the default form submission behavior)
- [.appendTo()][] / [.append()][]
- [.val()][]

[.append()]: http://api.jquery.com/append/
[.appendTo()]: http://api.jquery.com/appendTo/
[.val()]: http://api.jquery.com/val/
[event.preventDefault()]: http://api.jquery.com/event.preventDefault/
[Handling Events]: http://learn.jquery.com/events/handling-events/
[jQuery Event Basics]: http://learn.jquery.com/events/event-basics/

#AJAX
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

#Javascript
[Cheat Sheet](http://wps.aw.com/wps/media/objects/2234/2287950/javascript_refererence.pdf)
