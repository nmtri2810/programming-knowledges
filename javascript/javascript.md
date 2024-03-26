# Javascript

## Sync/async, Promise

- Handle **callback hell** problem

```javascript
let promise = new Promise()

promise
  .then(...)
  .catch(...)
  .finally(...)
```

## Closure

- Gọi đến 1 biến nằm ngoại phạm vi của block

## ECMAScript 6

### let, var

- Hoisting: moving declarations to the top (`var`)

### Template literals + multiline strings

`${}`

### Arrow functions

...

### Classes

...

### Default parameters values

...

### Enhanced object literals (Shortcut)

#### Obj key-value

```javascript
let name = "js";
let price = 1000;

let course = {
  name,
  price,
};
```

#### Obj method

Instead of:

```javascript
let name = "js";

let course = {
  name,
  getName: function () {
    return name;
  },
};
```

Use:

```javascript
let name = "js";

let course = {
  name,
  getName() {
    return name;
  },
};
```

#### Obj key base on a variable

```javascript
let nameField = "name";
let priceField = "price";

let course = {
  [nameField]: "javascrit",
  [priceField]: 1000,
};
```

### Destructuring + rest parameters

- rest parameters: use with tham số

#### Array

```javascript
let arr = ["one", "two", "three"];

let [a, b, c] = arr;
console.log(a, b, c);
// # => one two three

let [a, ...rest] = arr;
console.log(rest);
// # => ["two", "three"]
```

```javascript
function logger([name, ...rest]) {
  console.log(name);
  console.log(...rest);
}

logger([1, 2, 3]);
```

#### Object

```javascript
let course = {
  name: "Js",
  price: 1000,
  student: 123,
};

let { name, price, student } = course;
console.log(name, price, student);
// # => Js 1000 123

let { name, ...rest } = course;
console.log(rest);
// # => { price: 1000, student: 123 }
```

```javascript
function logger({ name, ...rest}) {
  console.log(name)
  console.log(...rest)
}

logger({ 1, 2, 3})
```

### Spread

- Same `...`, but different usage
- Only take the content of _arrays_ and _objects_, and **_spread_** them to new arr or obj
- Use with đối số

#### Array

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5];

let arr3 = [...arr2, ...arr1];
console.log(arr3);
// # => [4, 5, 1, 2, 3]
```

#### Obj

```javascript
let obj1 = {
  name: "Js",
};

let obj2 = {
  price: 1000,
};

let obj3 = {
  ...obj1,
  ...obj2,
};

console.log(obj3);
// # => { name: "Js", price: 1000 }
```

### Tagged Template literals

...

### Modules

...

### Optional chaining

`?.` (same as Ruby)

## Kiến thức ngoài lề
