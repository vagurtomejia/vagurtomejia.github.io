#Javascript

## Object Literals

### Syntax
```js
var mySpecialObject = {

        someProperty: "Foo",
        someOtherProperty: "Bar",
        8: "WTF?",
        mySpecialFunction: function () {
            return "I'm a special function";
        }
};
```
### Interacting with an Object Literal

```js
mySpecialObject.someProperty
mySpecialObject.someOtherProperty
mySpecialObject.mySpecialFunction()
```
or 
 
```js
mySpecialObject[8]
mySpecialObject["someProperty"]
mySpecialObject["someOtherProperty"]
```

I can also add properties and functions to an Object literal on the fly.

```js
mySpecialObject.someCrazyNewThing = 5;
mySpecialObject.someCrazyNewFunction = function () {
    return "I'm new";
}
```

##OOJS
### Constructor Functions

If I want to be able to create different instances of an object, I can use a constructor function to accomplish this. 
    
```js
var Person = function(firstName, lastName, ssn) {
    //private vars & methods
    var ssn = ssn;
    var that = this;
    
    function getLastFour() {
        // i can use 'that' here to refer to 'this' Person properly
        // this inside of a private method like this gets scoped globally
        // I do not need 'this.' to access private vars & methods of this object.
        console.log(that);
        return ssn.substring((ssn.length - 4), ssn.length);

    }
    
    // public vars and functions use 'this'
    this.firstName = firstName;
    this.lastName = lastName;

    this.maskedSSN = function() {
        return "XXX-XX-" + getLastFour();
    }
}
```

#### Accessing properties & functions of an object created with a constructor function

```js 
kevin = new Person("Kevin", "Solorio", "123456789");
kevin.firstName
kevin.lastName
```

Because I scoped ssn, that and getLastFour() to be local to the Object, I cannot access them once I have an instance unless I scope it through a public method like getSSN()

```js
kevin.ssn
kevin.that
getLastFour()
```

Don't forget the new keyword when creating new object with constructor functions.  It will have unintended effects.  What you will see is that all the properties of the Person object joe are placed on the window object.

```js
// forgetting the new keyword will produce undesirable effects
joe = Person("Joe", "Smith", "123456789");
window.firstName
```

### Prototypes

Prototypes allow me to add methods to the object.  The reason this is preferred over doing it in the contstructor function itself, is because it adds all the methods to the protoype for the object to use rather than defining it every time with the constructor function.  Helps with memory management.

```js
Person.prototype.fullName = function(){
        return this.firstName + " " + this.lastName;
}

Person.prototype.initials = function() {
        return this.firstName[0] + this.lastName[0];
}

kevin.fullName();
kevin.initials();
```

Note:  I cannot access private variables or methods if I define a method with prototype as shown below.  It does not have visibility to the private things.
```js
Person.prototype.maskedSSN = function() {
    return "XXX-XX-" + this.ssn.substring((this.ssn.length - 4), this.ssn.length);
}

kevin.maskedSSN();
```
    
### Object.create() - object creation alternative

I can define a set of functions as a hash and pass it to the Object.create().  This will add all the functions to prototype.

```js
var behavior = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }, 
    initials: function() {
        return this.firstName[0] + this.lastName[0];
    }
}

var person = Object.create(behavior);
person.firstName = "Bob";
person.lastName = "Smith";
person.fullName();
```

##Tests
###Jasmine
To run the tests, we open the `SpecRunner.html` file in the browser.
[Jasmine doc](http://jasmine.github.io/2.4/introduction.html)

##DBC Challenges
- [Garden and Flowers](https://github.com/chi-red-pandas-2016/oojs-garden-challenge/tree/solo_vagurtomejia)
###Flower
```js
var Flower = function(name, color) {
  this.name = name;
  this.color = color;
}
```

###Garden
```js
var garden = {
  name: "Kula Botanical Garden",
  location: "Makawao",
  flowers: [],
  addFlower: function(flower) {
    this.flowers.push(flower);
  },
  plant: function(newFlowers) {
    this.flowers.push.apply(this.flowers, newFlowers);
    //other option:
    //this.flowers = this.flowers.concat(newFlowers);
  },
  remove: function(flower) {
    flowerIndex = this.flowers.indexOf(flower);
    this.flowers.splice(flowerIndex,1);
  },
  flowersByColor: function(color) {

    return this.flowers.filter(function(flower) {
      return flower.color === color;
    });

  },

  flowersByName: function(name) {

    return this.flowers.filter(function(index) {
      return index.name === name;
    });
  }

}
```

##Fixing issues
###Var scope: local to functions
Variables are function scope if we use 'var':

###Var scope: global

Variables are gobal scope if we do not use 'var'
```js
function a() {
  for(i=0; i < 10; i++) {
    b();
  }
}

function b() {
  for(i=0; i < 100; i++) {
    console.log(i);
  }
}
```

##DBC challenges
- [Orange Tree](https://github.com/chi-red-pandas-2016/oojs-orange-tree-challenge/tree/solo_vagurtomejia)
- [Station and bikes](https://github.com/chi-red-pandas-2016/oojs-bikes-and-stations-challenge/tree/pair-DanielJPark%2Ckellymp/src)
###OrangeTree
```js
var OrangeTree = function() {
  this.age = 0;
  this.height = 0;
  this.oranges = [];

  this.isMature = function() {
    if (this.age < 7)
      return false;
    else
      return true;
  }

  this.isDead = function() {
    return this.age >= 100;
  }

  this.hasOranges = function() {
    return this.oranges.length > 0;
  }

  this.passGrowingSeason = function() {

    if (this.isDead())
      return;

    this.age += 1;
    if (this.height < 25)
       this.height += 2.5;

     if (this.isMature()) {
      this.oranges = [];
      var numberOfOranges = Math.floor((Math.random() * 300) + 100);
      for (i = 0; i < numberOfOranges; i++) {
          this.oranges.push(new Orange());
      }
    }
  }

  this.pickOrange = function() {
    return this.oranges.pop();
  }

};
```

###Orange
```js
var Orange = function() {
  this.diameter = this.calculateDiameter();
}

Orange.prototype.minOrangeDiameter = 2.5;
Orange.prototype.maxOrangeDiameter = 3.2;

Orange.prototype.calculateDiameter = function() {
  return (Math.random() * (this.maxOrangeDiameter - this.minOrangeDiameter)) + this.minOrangeDiameter;
}
```

##Ressources
- [MDN documentation](https://developer.mozilla.org/fr/docs/Web/JavaScript)
- [MDN Javscript reference](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference)
- [Arrays MDN doc](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Array)
- [Javascript cheat Sheet](http://wps.aw.com/wps/media/objects/2234/2287950/javascript_refererence.pdf)
- [Objects and classes](https://github.com/chi-red-pandas-2016/javascript-from-ruby-challenge/blob/master/07-objects-and-classes/01-what-are-objects-and-classes.md)
- [OOJS lecture](https://gist.github.com/alycit/e6f5f20ced9b42a64f5a)
- [JQuery cheat sheet](https://oscarotero.com/jquery/)


