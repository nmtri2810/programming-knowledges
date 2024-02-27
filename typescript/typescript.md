# Typescript

- Js: dynamic typing - running the code to see what happens
- Ts: static type system to make predictions about what the code is expected to do before it runs

## Types

### Primitives: `string`, `number`, `boolean`

```typescript
let hello = "Hello, world!" // plain js
let hello: string = "Hello, world!" // ts
```

### Arrays

- Array like `[1, 2, 3]` can be written with syntax:
  - `number[]` is an array of numbers (`string[]` - array of strings, ...)
  - `Array<number>` same thing with `number[]` - generics

### `any`

- Special type, can use any types => same as pure js

### Functions

- Có thể định nghĩa kiểu dữ liệu của tham số truyền vào và kiểu dữ liệu trả ra

```typescript
function isEvenNumber(param: number): boolean {
  return param % 2 === 0;
}
```

### Objects

- Function that takes a point-like object

```typescript
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

#### Optional properties

- Add a `?` after the property name

```typescript
function printName(obj: { first: string; last?: string }) {
  // ...
}
// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

### Type aliases

- A *type alias* is exactly that - a *name* for any type.

```typescript
type Point = {
  x: number;
  y: number;
};

// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

### Interfaces

- An *interface declaration* is another way to name an object type:

```typescript
interface Point {
  x: number;
  y: number;
}

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

#### Differences Between Type Aliases and Interfaces

- A `type` cannot be re-opened to add new properties vs an `interface` which is always extendable.

### Composing types

- Tự tạo ra types phức tạp bằng cách kết hợp các kiểu dữ liệu đã có
- 2 cách thường dùng: Unions và Generics

#### Unions

- Một type được định nghĩa có thể có 1 hoặc nhiều types

```typescript
type WindowStates = "open" | "closed" | "minimized"; // Type của WindowStates có thể là "open", "closed" hoặc "minimized"
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

- Ngoài ra Unions cũng hỗ trợ cách để handle different types. VD: function bên dưới có thể nhận một `number` hoặc một `string`

```typescript
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
// OK
printId(101);
// OK
printId("202");
// Error
printId({ myID: 22342 });
```

- If you have the union `string | number`, you can’t use methods that are only available on `string`

```typescript
function printId(id: number | string) {
  console.log(id.toUpperCase()); // Error
}
```

#### Generics

- Tạo types cho Array,... (Tương tự generics trong Java, C#, ...)
- VD: 1 Array không có generics có thể có nhiều types, còn nếu có generics thì có thể thể hiện types trong Array đó

```typescript
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

```typescript
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
```

### Type Assertions

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;

// Another way
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

### Structural Type System

- Kiểm tra type bằng cách check *shape* the value has, gọi là "duck typing" hoặc "structural typing"
- Nếu 2 obj cùng 1 shape thì được coi như cùng 1 type
- Ở VD dưới, biến `point` không được khai báo là `Point` type. Typescript so sánh shape của biến `point` với `Point` type

```typescript
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```
