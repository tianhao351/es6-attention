# ES6

## 1.Destructuring

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

