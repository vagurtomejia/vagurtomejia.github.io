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


##Ressources
[MDN documentation](https://developer.mozilla.org/fr/docs/Web/JavaScript)
[JQuery cheat sheet](https://oscarotero.com/jquery/)
[Javascript cheat Sheet](http://wps.aw.com/wps/media/objects/2234/2287950/javascript_refererence.pdf)
[OOJS lecture](https://developer.mozilla.org/fr/docs/Web/JavaScript)


