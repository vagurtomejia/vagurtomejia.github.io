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
```javascript
jQuery(document).ready(function(){
  $("span").text("$100");
});
```
OR
```javascript
Shorthand for document ready:
$(function(){
.....
```

## Selectors
### id selector
```javascript
$( "#id" )
```
[id jQuery doc](http://api.jquery.com/id-selector/)
### class selector
```javascript
$( ".class" )
```
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
```javascript
$("#tours  li")
```

###direct descendant : child slector
Let's use a direct child selector to only select the li items that are direct children of #tours.
```javascript
$("#tours > li")
```

###multiple different items
select all tours that are from Asia and all tours that are On Sale. You'll need to use multiple selectors for this, with the classes for .asia and .sale.
```javascript
$("#tours > .asia, #tours > .sale")
```

###pseudo-selectors
####:first
Use the :first pseudo selector to select the first tour in the list.
```javascript
$("#tours li:first")
```

####:even
Find the direct children li elements, and then use the :even pseudo selector to select every other li element.
```javascript
$("#tours > li:even")
```

##Traversing
###find()
To select all vacations from America we can use $("#vacations .america"). This or traversal with the find method:
```javascript
$("#vacations").find(".america")
```

###first()
Using traversal or filtering, select the first vacation li element from the list.
```javascript
$("#vacations").find("li").first()
```

###last()
Find the last li within #vacations using traversal 
```javascript
$("#vacations").find("li").last()
```

###prev()
Use traversal with the prev() method to select the vacation that is right before the last one.
```javascript
$("#vacations li").last().prev()
```

###parent()
Using traversal, select all tours that have a .featured class on their title by getting the parent() of featured titles.
```javascript
$(".featured").parent()
```

###children()
Select all the tours: $("#tours > li"). You immediately realize it can be done better, refactor this to use traversal with children().
```javascript
$("#tours").children()
```

##Manipulating the DOM

###Creating a DOM node
When the page loads, we'll show a message to the traveler letting them know how to book this trip. To start out, let's create a <span> node with our phone number, 1-555-jquery-air and set it to the message variable.
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');

###Adding to the DOM
####before()
Let's add the phone number immediately before() the "Book Now" button. You can check out the HTML of the rendered page by clicking on the HTML tab below.
```javascript
var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
$(".book").before(message)
```

####append()
append() our <span> to the bottom of the .usa element. Let's change the code to add it there instead.
```javascript
var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
$(".usa").append(message)
```

###Removing from the DOM
####remove()
Remove that "Book Now" button until we can implement it.
```javascript
var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
$('.usa').append(message);
$('.book').remove()
```

##Useful JQuery methods
###attr(), prop(), is() 
[attr documentation](http://api.jquery.com/attr/)
[prop documentation](http://api.jquery.com/prop/)
[is documentation](http://api.jquery.com/is/)

```javascript
$( "input" )
  .change(function() {
    var $input = $( this );
    $( "p" ).html( ".attr( 'checked' ): <b>" + $input.attr( "checked" ) + "</b><br>" +
      ".prop( 'checked' ): <b>" + $input.prop( "checked" ) + "</b><br>" +
      ".is( ':checked' ): <b>" + $input.is( ":checked" ) + "</b>" );
  })
  .change();
```

###val() - returns string or number or array
[val documentation](http://api.jquery.com/val/)
The .val() method is primarily used to get the values of form elements such as input, select and textarea.
When called on an empty collection, it returns undefined.


###css(propertyName) - returns string
[css documentation](http://api.jquery.com/css/)
####Get the value of a computed style property for the first element in the set of matched elements or set one or more CSS properties for every matched element.
example of getting multiple property values:
```javascript
  var styleProps = $( this ).css([
    "width", "height", "color", "background-color"
  ]);
```

####To set a specified CSS property, use the following syntax:
  css("propertyname","value");
example of setting a value:
```javascript
  $(selector).css(property, value);
```

###serialize() - returns string
The .serialize() method creates a text string in standard URL-encoded notation. It can act on a jQuery object that has selected individual form controls, such as ```html <input>, <textarea>, and <select>```: 
```javascript
$( "input, textarea, select" ).serialize();
```

ex:

```html
    <form id="style_editor" name="style_editor">
      <input name="selector" placeholder="css selector" />
      <input name="property" placeholder="property" />
      <input name="value" placeholder="value" />
      <input type="submit" value="Style it!" />
    </form>
```

```javascript
    $( "form" ).on( "submit", function( event ) {
      event.preventDefault();
      console.log( $( this ).serialize() );
      //=>selector=a&property=s&value=d
    });
```
[serialize documentation](https://api.jquery.com/serialize/)

##Acting on interaction (events handlers)
###Events and events handlers
* [Basics of jQuery Events][]
* [Event Handling][]
* [event.preventDefault() method][]

[Basics of jQuery Events]: http://learn.jquery.com/events/event-basics/
[Event Handling]: http://learn.jquery.com/events/handling-events/
[event.preventDefault() method]: http://api.jquery.com/event.preventDefault/

If you bind to the `submit` event, you should consider using the
[event.preventDefault() method][] provided by jQuery.

###Click interaction
Let's start by wrapping all of our previous code in a click handler for any `<button>` elements using the on() method.

```javascript
$( "button" ).click(function() {
  var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
  $('.usa').append(message);
  $('button').remove();
});
```
or
```javascript
$('button').on('click', function() {
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $('.usa').append(message);
    $('button').remove();
  });
```

####Remove only the clicked button
```javascript
$(this).remove();
```

####Relative Traversing
* Add the message after() the button we click on.
```javascript
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $(this).after(message);
```


* The `<button>` is inside a `<div>` tag. We don't want the message to go inside the `<div>` tag though, we want it to go at the bottom of the `<li>` element. Instead of using after(), let's change our code to find the closest() .tour element and append() the message to the bottom of it.
```html
<li class="usa tour">
  <h2>New York, New York</h2>
  <span class="details">$1,899 for 7 nights</span>
  <div>
    <button class="book">Book Now</button>
  </div>
</li>
```

```javascript
$(this).closest('.tour').append(message);
```

###All the code
```javascript
$(document).ready(function() {
  $('button').on('click', function() {
    var message = $('<span>Call 1-555-jquery-air to book this tour</span>');
    $(this).closest('.tour').append(message);
    $(this).remove();
  });
});
```

## Fetching Data From the DOM I 
Let's add a bit more incentive to get people to book these tours by offering a discount if they book today. Create a discount variable, and then assign the discount that is stored in the data() attribute on the .tour element.
We want to show this discount to the user in the message we show after the "Book Now" button is clicked.

```javascript
var discount = $(this).closest('.tour').data('discount');
var message = $('<span>Call 1-555-jquery-air for a $'+discount+' discount.</span>');
```

##Better handlers
There is a small problem with the way our on() handler is being used. Currently, it is targeting all of the `<button>` elements on the screen. Refactor the on() handler to only target `<button>` elements within a .tour by using the selector argument of the on() method.
```javascript
    $(document).ready(function() {
      $('.tour').on('click', 'button', function() {
        ...
```

##Filters
Let's add some result filtering options to our page. We want to be able to click on a filter and highlight the corresponding tours with a .highlight class..(on-sale or .featured filters).

```javascript
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
```

##Ressources
- [jQuery Cheat Sheet](https://oscarotero.com/jquery/)
- [jQuery Event Basics][]
- [Handling Events][] (e.g., a form `"submit"` event)
- [event.preventDefault()][] (e.g., to prevent the default form submission behavior)
- [.appendTo()][] / [.append()][]
- [.val()][]
- [CSS Mozilla documentation](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- [JQuery cheat sheet](https://oscarotero.com/jquery/)
[Javascript cheat Sheet](http://wps.aw.com/wps/media/objects/2234/2287950/javascript_refererence.pdf)

[.append()]: http://api.jquery.com/append/
[.appendTo()]: http://api.jquery.com/appendTo/
[.val()]: http://api.jquery.com/val/
[event.preventDefault()]: http://api.jquery.com/event.preventDefault/
[Handling Events]: http://learn.jquery.com/events/handling-events/
[jQuery Event Basics]: http://learn.jquery.com/events/event-basics/
