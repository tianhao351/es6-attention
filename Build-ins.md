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





### 2.Sets

#### Sets

In ES6, there’s a new built-in object that behaves like a mathematical set and works similarly to an array. This new object is conveniently called a "Set". The biggest differences between a set and an array are:

- Sets are not indexed-based - you do not refer to items in a set based on their position in the set
- items in a Set can’t be accessed individually

Basically, a Set is an object that lets you store unique items. You can add items to a Set, remove items from a Set, and loop over a Set. These items can be either primitive values or objects.

#### How to Create a Set

There’s a couple of different ways to create a Set. The first way, is pretty straightforward:

```javascript
const games = new Set();
console.log(games);
```

> `Set {}`

This creates an empty Set `games` with no items.

If you want to create a Set from a list of values, you use an array:

```javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);
console.log(games);
```

> `Set {'Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart'}`

Notice the example above automatically removes the duplicate entry `"Super Mario Bros."` when the Set is created. Pretty neat!



#### Modifying Sets

After you’ve created a Set, you’ll probably want to add and delete items from the Set. So how do you that? You use the appropriately named, `.add()` and `.delete()` methods:

```javascript
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);

games.add('Banjo-Tooie');
games.add('Age of Empires');
games.delete('Super Mario Bros.');

console.log(games);
```

> `Set {'Banjo-Kazooie', 'Mario Kart', 'Banjo-Tooie', 'Age of Empires'}`

On the other hand, if you want to delete all the items from a Set, you can use the `.clear()` method.

```javascript
games.clear()
console.log(games);
```

> `Set {}`

> **TIP**: If you attempt to `.add()` a duplicate item to a Set, you won’t receive an error, but the item will not be added to the Set. Also, if you try to `.delete()` an item that is not in a Set, you won’t receive an error, and the Set will remain unchanged.
>
> `.add()` returns the `Set` if an item is successfully added. On the other hand, `.delete()` returns a Boolean (`true` or `false`) depending on successful deletion.

.size

.values()

.has()

#### Sets & Iterators

##### Using the SetIterator

```javascript
const iterator = months.values();
iterator.next();
```

![屏幕快照 2017-09-21 下午4.44.31](/Users/computer/Desktop/note/es6/屏幕快照 2017-09-21 下午4.44.31.png)

##### Using for...of

```javascript
const colors = new Set(['red', 'orange', 'yellow', 'green', 'blue', 'violet', 'brown', 'black']);
for (const color of colors) {
  console.log(color);
}
```
#### What is a WeakSet?

A WeakSet is just like a normal Set with a few key differences:

1. a WeakSet can only contain objects
2. a WeakSet is not iterable which means it can’t be looped over
3. a WeakSet does not have a `.clear()` method





### 3.Maps

#### maps

you add key-values by using the Map’s `.set()` method.

```javascript
const employees = new Map();

employees.set('james.parkes@udacity.com', { 
    firstName: 'James',
    lastName: 'Parkes',
    role: 'Content Developer' 
});
employees.set('julia@udacity.com', {
    firstName: 'Julia',
    lastName: 'Van Cleve',
    role: 'Content Developer'
});
employees.set('richard@udacity.com', {
    firstName: 'Richard',
    lastName: 'Kalehoff',
    role: 'Content Developer'
});

console.log(employees);
```

The `.set()` method takes two arguments. The first argument is the key, which is used to reference the second argument, the value.

To remove key-value pairs, simply use the `.delete()` method.

```javascript
employees.delete('julia@udacity.com');
employees.delete('richard@udacity.com');
console.log(employees);
```

> `Map {'james.parkes@udacity.com' => Object {firstName: 'James', lastName: 'Parkes', role: 'Course Developer'}}`

Again, similar to Sets, you can use the `.clear()` method to remove all key-value pairs from the Map.

```javascript
employees.clear()
console.log(employees);
```



#### working with maps

Map, you can use the `.has()` method to check if a key-value pair exists in your Map by passing it a key.

```javascript
const members = new Map();

members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

console.log(members.has('Xavier'));
console.log(members.has('Marcus'));
```

> `false`
> `true`

And you can also retrieve values from a Map, by passing a key to the `.get()` method.

```javascript
console.log(members.get('Evelyn'));
```

> `75.68`



#### looping Through Maps

