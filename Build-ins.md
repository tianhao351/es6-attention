# ES6-attention

## III. Build-ins

### 1.symbol

#### what

 A **symbol** is a unique and immutable data type that is often used to identify object properties.

```javascript
const sym1 = Symbol('apple');
console.log(sym1);
```

> Symbol(apple)

if you compare two symbols with the same description…

```javascript
const sym2 = Symbol('banana');
const sym3 = Symbol('banana');
console.log(sym2 === sym3);
```

> false

…then the result is `false` because the description is *only* used to described the symbol. It’s not used as part of the symbol itself—each time a new symbol is created, regardless of the description.

#### example

```javascript
const bowl = {
  'apple': { color: 'red', weight: 136.078 },
  'banana': { color: 'yellow', weight: 183.15 },
  'orange': { color: 'orange', weight: 170.097 }
};
```

If we want to add a "banana"

```javascript
const bowl = {
  'apple': { color: 'red', weight: 136.078 },
  'banana': { color: 'yellow', weight: 183.151 },
  'orange': { color: 'orange', weight: 170.097 },
  'banana': { color: 'yellow', weight: 176.845 }
};
console.log(bowl);
```

> Object {apple: Object, banana: Object, orange: Object}

At this time, **symbol** comes in handy  

```javascript
const bowl = {
  [Symbol('apple')]: { color: 'red', weight: 136.078 },
  [Symbol('banana')]: { color: 'yellow', weight: 183.15 },
  [Symbol('orange')]: { color: 'orange', weight: 170.097 },
  [Symbol('banana')]: { color: 'yellow', weight: 176.845 }
};
console.log(bowl);
```

> Object {Symbol(apple): Object, Symbol(banana): Object, Symbol(orange): Object, Symbol(banana): Object}

