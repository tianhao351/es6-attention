# ES6-attention

## II. Functions

### 1.Arrow functions

#### Comparison

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(function(name) { 
  return name.toUpperCase();
});
```

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```

#### Convert a function to an arrow function

- remove the `function` keyword
- remove the parentheses
- remove the opening and closing curly braces
- remove the `return` keyword
- remove the semicolon
- add an arrow ( `=>` ) between the parameter list and the function body

#### Parentheses and arrow function parameteres

```javascript
const greet = name => `Hello ${name}!`;
greet('Asser');


const sayHi = () => console.log('Hello Udacity Student!');
sayHi();

const orderIceCream = (flavor, cone) => console.log(`Here's your ${flavor} ice cream in a ${cone} cone.`);
orderIceCream('chocolate', 'waffle');
```
One confusing syntax is when an arrow function is stored in a variable.



#### Concise and block body syntax

All of the arrow functions we've been looking at have only had a single expression as the function body:

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```

This format of the function body is called the *"concise body syntax"*. The concise syntax:

- has no curly braces surroun
- ​
- ding the function body
- and automatically returns the expression.

If you need more than just a single line of code in your arrow function's body, then you can use the *"block body syntax"*.

```javascript
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map( name => {
  name = name.toUpperCase();
  return `${name} has ${name.length} characters in their name`;
});
```

Important things to keep in mind with the block syntax:

- it uses curly braces to wrap the function body
- and a `return` statement needs to be used to actually return something from the function.





#### "this" and Arrow Functions

The value of `this` *inside* an arrow function is the same as the value of `this` *outside* the function.

##### problems:

```javascript
function IceCream() {
  this.scoops = 0;
}

// adds scoop to ice cream
IceCream.prototype.addScoop = function() {
  setTimeout(function() {
    this.scoops++;
    console.log('scoop added!');
  }, 500);
};

const dessert = new IceCream();
dessert.addScoop();
```

The function passed to `setTimeout()` is called without `new`, without `call()`, without `apply()`, and without a context object. That means the value of `this` inside the function is the global object and **NOT** the `dessert` object. So what actually happened was that a new `scoops` variable was created (with a default value of `undefined`) and was then incremented (`undefined + 1` results in `NaN`):



##### old ways:

```javascript
// constructor
function IceCream() {
  this.scoops = 0;
}

// adds scoop to ice cream
IceCream.prototype.addScoop = function() {
  const cone = this; // sets `this` to the `cone` variable
  setTimeout(function() {
    cone.scoops++; // references the `cone` variable
    console.log('scoop added!');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();
```

##### es6:

```javascript
function IceCream() {
  this.scoops = 0;
}

// adds scoop to ice cream
IceCream.prototype.addScoop = function() {
  setTimeout(() => { // an arrow function is passed to setTimeout
    this.scoops++;
    console.log('scoop added!');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();
```

##### addition:

```javascript
function IceCream() {
    this.scoops = 0;
}

// adds scoop to ice cream
IceCream.prototype.addScoop = () => { // addScoop is now an arrow function
  setTimeout(() => {
    this.scoops++;
    console.log('scoop added!');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();
```

This doesn't work for the same reason - arrow functions inherit their `this` value from their surrounding context. Outside of the `addScoop()` method, the value of `this` is the global object. So if `addScoop()` is an arrow function, the value of `this` *inside* `addScoop()` is the global object. Which then makes the value of `this` in the function passed to `setTimeout()` *also* set to the global object!



### 2. Default Function Parameters

old way:

```javascript
function greet(name, greeting) {
  name = (typeof name !== 'undefined') ?  name : 'Student';
  greeting = (typeof greeting !== 'undefined') ?  greeting : 'Welcome';

  return `${greeting} ${name}!`;
}

greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!	
```

es6:

```javascript
function greet(name = 'Student', greeting = 'Welcome') {
  return `${greeting} ${name}!`;
}

greet(); // Welcome Student!
greet('James'); // Welcome James!
greet('Richard', 'Howdy'); // Howdy Richard!
```



#### Defaults and destructuring arrays

```javascript
function createGrid([width = 5, height = 5]) {
  return `Generates a ${width} x ${height} grid`;
}

createGrid([]); // Generates a 5 x 5 grid
createGrid([2]); // Generates a 2 x 5 grid
createGrid([2, 3]); // Generates a 2 x 3 grid
createGrid([undefined, 3]); // Generates a 5 x 3 grid


createGrid(); // throws an error
```

```javascript
function createGrid([width = 5, height = 5] = []) {
  return `Generating a grid of ${width} by ${height}`;
}

createGrid(); // Generates a 5 x 5 grid

```

#### Array defaults vs. object defaults

Default function parameters are a simple addition, but it makes our lives so much easier! One benefit of object defaults over array defaults is how they handle skipped options. Check this out:

```javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']} = {}) { … }
```

...with the `createSundae()` function using object defaults with destructuring, if you want to use the default value for `scoops` but change the `toppings`, then all you need to do is pass in an object with `toppings`:

```javascript
createSundae({toppings: ['Hot Fudge', 'Sprinkles', 'Caramel']});
```

Compare the above example with the same function that uses array defaults with destructuring.

```javascript
function createSundae([scoops = 1, toppings = ['Hot Fudge']] = []) { … }
```

With this function setup, if you want to use the default number of `scoops` but change the `toppings`, you'd have to call your function a little...oddly:

```javascript
createSundae([undefined, ['Hot Fudge', 'Sprinkles', 'Caramel']]);
```

Since arrays are positionally based, we have to pass `undefined` to "skip" over the first argument (and accept the default) to get to the second argument.

Unless you've got a strong reason to use array defaults with array destructuring, we recommend going with object defaults with object destructuring!





### 3. JavaScript Classes

#### ES5 "Class" Recap

Since ES6 classes are just a mirage and hide the fact that prototypal inheritance is actually going on under the hood, let's quickly look at how to create a "class" with ES5 code:

```javascript
function Plane(numEngines) {
  this.numEngines = numEngines;
  this.enginesActive = false;
}

// methods "inherited" by all instances
Plane.prototype.startEngines = function () {
  console.log('starting engines...');
  this.enginesActive = true;
};

const richardsPlane = new Plane(1);
richardsPlane.startEngines();

const jamesPlane = new Plane(4);
jamesPlane.startEngines();
```

In the code above, the `Plane` function is a *constructor function* that will create new Plane objects. The data for a specific Plane object is passed to the `Plane` function and is set on the object. Methods that are "inherited" by each Plane object are placed on the `Plane.prototype` object. Then `richardsPlane` is created with one engine while `jamesPlane` is created with 4 engines. Both objects, however, use the same `startEngines` method to activate their respective engines.

Things to note:

- the constructor function is called with the `new` keyword
- the constructor function, by convention, starts with a capital letter
- the constructor function controls the setting of data on the objects that will be created
- "inherited" methods are placed on the constructor function's prototype object

Keep these in mind as we look at how ES6 classes work because, remember, ES6 classes set up all of this for you under the hood.

#### ES6 Classes

Here's what that same `Plane` class would like like if it were written using the new `class` syntax:

```javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}
```

#### Things to look out for when using classes

1. class is not magic
   - The `class` keyword brings with it a lot of mental constructs from other, class-based languages. It doesn't magically add this functionality to JavaScript classes.
2. class is a mirage over prototypal inheritance
   - We've said this many times before, but under the hood, a JavaScript class just uses prototypal inheritance.
3. Using classes requires the use of new
   - When creating a new instance of a JavaScript class, the `new` keyword must be used



#### Static methods

To add a static method, the keyword `static` is placed in front of the method name. Look at the `badWeather()` method in the code below.

```javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  static badWeather(planes) {
    for (plane of planes) {
      plane.enginesActive = false;
    }
  }

  startEngines() {
    console.log('starting engines…');
    this.enginesActive = true;
  }
}
```

See how `badWeather()` has the word `static` in front of it while `startEngines()` doesn't? That makes `badWeather()` a method that's accessed directly on the `Plane` class, so you can call it like this:

```javascript
Plane.badWeather([plane1, plane2, plane3]);
```



#### Subclasses(Super and Extends)

Now that we've looked at creating classes in JavaScript. Let's use the new `super` and `extends` keywords to extend a class.

```javascript
class Tree {
  constructor(size = '10', leaves = {spring: 'green', summer: 'green', fall: 'orange', winter: null}) {
    this.size = size;
    this.leaves = leaves;
    this.leafColor = null;
  }

  changeSeason(season) {
    this.leafColor = this.leaves[season];
    if (season === 'spring') {
      this.size += 1;
    }
  }
}

class Maple extends Tree {
  constructor(syrupQty = 15, size, barkColor, leaves) {
    super(size, barkColor, leaves);
    this.syrupQty = syrupQty;
  }

  changeSeason(season) {
    super.changeSeason(season);
    if (season === 'spring') {
      this.syrupQty += 1;
    }
  }

  gatherSyrup() {
    this.syrupQty -= 3;
  }
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
```

Both `Tree` and `Maple` are JavaScript classes. The `Maple` class is a "subclass" of `Tree` and uses the `extends` keyword to set itself as a "subclass". To get from the "subclass" to the parent class, the `super` keyword is used. Did you notice that `super`was used in two different ways? In `Maple`'s constructor method, `super` is used as a function. In `Maple`'s `changeSeason()`method, `super` is used as an object!

##### Compared to ES5 subclasses

Let's see this same functionality, but written in ES5 code:

```javascript
function Tree() {
  this.size = size || 10;
  this.leaves = leaves || {spring: 'green', summer: 'green', fall: 'orange', winter: null};
  this.leafColor;
}

Tree.prototype.changeSeason = function(season) {
  this.leafColor = this.leaves[season];
  if (season === 'spring') {
    this.size += 1;
  }
}

function Maple (syrupQty, size, barkColor, leaves) {
  Tree.call(this, size, barkColor, leaves);
  this.syrupQty = syrupQty || 15;
}

Maple.prototype = Object.create(Tree.prototype);
Maple.prototype.constructor = Maple;

Maple.prototype.changeSeason = function(season) {
  Tree.prototype.changeSeason.call(this, season);
  if (season === 'spring') {
    this.syrupQty += 1;
  }
}

Maple.prototype.gatherSyrup = function() {
  this.syrupQty -= 3;
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
```



#### Working with subclasses

Like most of the new additions, there's a lot less setup code and it's a lot cleaner syntax to create a subclass using `class`, `super`, and `extends`.

Just remember that, under the hood, the same connections are made between functions and prototypes.

### `super` must be called before `this`

In a subclass constructor function, before `this` can be used, a call to the super class must be made.

```javascript
class Apple {}
class GrannySmith extends Apple {
  constructor(tartnessLevel, energy) {
    this.tartnessLevel = tartnessLevel; // `this` before `super` will throw an error!
    super(energy); 
  }
}
```

