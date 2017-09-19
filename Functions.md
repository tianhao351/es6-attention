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