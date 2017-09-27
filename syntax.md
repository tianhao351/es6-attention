# ES6-attention

## I. Syntax

### 1.Destructuring

#### array

```javascript
const members = new Map();

members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

for (const member of members) {
	let [key,value] = member
	console.log(key, value);
}
```

#### object

```javascript
const circle = {
  radius: 10,
  color: 'orange',
  getArea: function() {
    return Math.PI * this.radius * this.radius;
  },
  getCircumference: function() {
    return 2 * Math.PI * this.radius;
  }
};

let {radius, getArea, getCircumference} = circle;
```

 Calling `getArea()` will return `NaN`. When you destructure the object and store the `getArea()` method into the `getArea` variable, it no longer has access to `this` in the `circle` object which results in an area that is `NaN`.



### 2.Object Literal Shorthand

#### old way:

```javascript
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = {
  type: type,
  color: color,
  carat: carat,
  calculateWorth: function() {
  	...   
  }
};
```

#### es6:

```javascript
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

let gemstone = {
  type,
  color,
  carat,
  calculateWorth() { ... }
};
```



### 3. for loop

#### basic for loop:

```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (let i = 0; i < digits.length; i++) {
  console.log(digits[i]);
}
```

#### for in loop

```javascript
Array.prototype.decimalfy = function() {
  for (let i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(2);
  }
};

const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in digits) {
  console.log(digits[index]);
}
```

> **Prints:**
> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> function() {
>  for (let i = 0; i < this.length; i++) {
>   this[i] = this[i].toFixed(2);
>  }
> }

the for...in loop can get you into big trouble when you need to add an extra method to an array (or another object). Because for...in loops loop over all enumerable properties, this means if you add any additional properties to the array's prototype, then those properties will also appear in the loop.

#### forEach

**forEach loop** is another type of for loop in JavaScript. However, `forEach()` is actually an array method, so it can only be used exclusively with arrays. There is also no way to stop or break a forEach loop.



#### es6:For...of loop

```javascript
Array.prototype.decimalfy = function() {
  for (i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(2);
  }
};

const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;
  }
  console.log(digit);
}
```

**Prints:**
1
3
5
7
9



### 4.Spread operator and Rest parameter

```javascript
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];

const produce = [...fruits,...vegetables];

console.log(produce);
```

```javascript
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(total, subtotal, tax, items);
```

sum a unknown numbers of things:

old ways:

```javascript
function sum() {
  let total = 0;  
  for(const argument of arguments) {
    total += argument;
  }
  return total;
}
```

es6:

```javascript
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```

