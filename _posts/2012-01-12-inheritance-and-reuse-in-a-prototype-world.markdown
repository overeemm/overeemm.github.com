---
layout: posts
title: Inheritance and reuse in a prototype world
categories: [javascript]
---
One of the things that I struggled with writing JavaScript, is the [prototype-based programming][1]. Coming from a Java and C# background, it is hard to let go of classes, interfaces and _normal_ inheritance. In a Class-based object-oriented language the classes define how objects behave. An object is an instance of a class. Classes can inherit from other classes and interfaces. In JavaScript (and other prototype-based languages like [IO][2]) there are no classes. There are objects, constructors and prototypes.

The prototype way in JavaScript is simply shown in the following example.
Let us first define a constructor:

    #js
    function Vehicle() {
      this.speed = 10;
    }
    
Now you shouldn't think to much of this constructor. It is a normal function in JavaScript, but by convention we let it start with a capital letter. The important thing is that you use it in combination with the __new__ keyword. 

The next step is to define functions on the prototype object of this constructor:

    #js
    Vehicle.prototype.setMaxSpeed = function (speed) {
      this.speed = speed;
    };
    
    Vehicle.prototype.maxSpeed = function () {
      return this.speed;
    };
    
When we construct a new object, we can use the constructor to create an instance:

    #js
    var myVehicle = new Vehicle();
    var isVehicle = myVehicle instanceof Vehicle; 
    // isVehicle is true
    
The constructor makes sure that the new object is based on the prototype. This makes all defined functions available in the prototype chain. There are two advantages of this approach: we can inherit from this object, and we can use the __instanceof__ operator.

    #js
    function Car() {
    }
    Car.prototype = new Vehicle();
    
    var myCar = new Car();
    var isVehicle = myCar instanceof Vehicle;

Now we have an instance that is more specific then our Vehicle object, but can be used as a Vehicle none the less. As one might notice, the inheritance is achieved by setting the prototype of a constructor. But it is not set to another constructor, but to the instance of an object. The inheritance chain is thus build on instances and not on classes.

There are other approaches possible to share code between objects. One of them is object augmentation. Because we can alter the JavaScript objects at any moment, we can augment objects with functionality.

    #js
    function makeVehicle(o) {
        var speed = 10;
        o.maxSpeed = function() {
           return speed;
        };
        o.setMaxSpeed = function(s) {
           speed = s;
        };
        return o;
    }
    
    var myVehicle = makeVehicle({});
    
    function makeCar(o){
        o = makeVehicle(o);
        // add extra functionality
        return o;
    }
    
We cannot use __instanceof__ anymore, but we can easily create objects with the same functionality. Of course there are some (subtle) difference with the prototype approach. If we decide that vehicles need more functions later on, we cannot alter all instances by changing the prototype. As a positive gain, we can use a [Mixin][7] pattern to get more flexibility.

The prototype approach can also be used without constructors and the __new__ keyword. [ECMAScript 5th Edition][3] added a new function, Object.create. This function creates a new object using the given object as prototype.

Douglas Crockford has some nice articles ([this][4] and [this][5]) about inheritance and reuse in JavaScript. Because objects in JavaScript are soft and can be changed at any moment, he hardly sees use for the Classical Inheritance approach.
Peter Michaux also [writes][8] about the augmentation approach, seeing the Decorator Pattern in it.

Another discussion raised by looking at the [SOLID principles][6] is the usage of inheritance. Most of the time, inheritance is used for method overloading and polymorphic methods. The Liskov Substitution Principle states that 

>Subtypes must be substitutable for their base types.

But JavaScript is not statically typed. This gives us the freedom to do more dynamic typing. Instead of looking at inheritance, we can look at behavior. By checking if certain functions are present we can be much more flexible in the range of objects that we can act on.

    #js
    function move(vehicle){
        if(!vehicle instanceof Vehicle)
            throw Error('not a vehicle');
        // ...
    }
    
    function moveDynamic(vehicle){
        if(!vehicle.move)
            throw Error('move function not present');
        // ...
    }

The __moveDynamic__ function shows that we can move any object that has a move function. The move function only works on instances of our constructor function. The last fits the dynamic model and gives more flexibility.

[9]: http://stackoverflow.com/questions/2271166/when-should-you-use-prototype-during-object-augmentation-in-javascript]
[1]: http://en.wikipedia.org/wiki/Prototype-based_programming
[2]: http://iolanguage.com/
[3]: https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Object/create
[4]: http://javascript.crockford.com/prototypal.html
[5]: http://www.crockford.com/javascript/inheritance.html
[6]: http://freshbrewedcode.com/derekgreer/2011/12/31/solid-javascript-the-liskov-substitution-principle/
[7]: http://en.wikipedia.org/wiki/Mixin
[8]: http://michaux.ca/articles/javascript-oop-encapsulation-durable-objects-parasitic-inheritance-and-the-decorator-pattern
