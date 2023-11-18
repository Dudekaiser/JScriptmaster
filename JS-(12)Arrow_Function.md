## 12. Arrow Function
Arrow Function can declare function easily just with ```=>```.

### 1) Syntax
```javascript
(param1, param2) => { ... } // If there are two or more parameters, bracket cannot be omitted.
singleParam => { ... } // If there is one parameter, the parentheses can be omitted.
() => { ... } // If there are no parameters, parentheses cannot be omitted.

param => { return expression; } // single line block
param => expression // If the function body is only 1 line, the braces {} can be omitted and return is implicit.
```
<br>

```javascript
() => { return {foo : bar}; }
() => ({ foo : bar }) // Use bracket() when returning object literal representations

() => {         // multi line block
    const x = 10;
    return x * x;
}
```
<br>

### 2) Call arrow function
Because arrow function can only be used as anonymous function, function expression is used to call an arrow function.
```javascript
var pow = function (x) {
  return x * x;
};
console.log(pow(10));
```
If we make above function as arrow function,
```javascript
var pow = (x) => x * x;
console.log(pow(10));
```
Of course, we can use in ```callback``` function.
```javascript
var arr = [1, 2, 3];
var pow = arr.map(function (x) {
  // x is element value
  return x * x;
});

console.log(pow); // [ 1, 4, 9 ]
```
change to arrow function
```javascript
const arr = [1, 2, 3];
const pow = arr.map((x) => x * x);

console.log(pow); // [ 1, 4, 9 ]
```

### 3) this
When ```this``` in a general function is called, the object to bind to ```this``` is dynamically determined depending on how the function was called.
<br>

When we declare an arrow function, the object to bind to ```this``` is statically determined. Unlike regular functions, ```this``` of an ```arrow function``` always points to ```this``` of the upper scope. This is called ```Lexical this```.
<br>

The method of determining ```this``` binding object of an arrow function is similar to ```lexical scope```, which is a method of determining the upper scope of a function.
```javascript
function Person() {
  this.age = 0;

  setTimeout(() => {
    this.age++; // this refers to the Person object, which is the upper scope.
    console.log(this.age);
  }, 1000);
}

Person(); // 1
```

### 4) When we do not use arrow function

#### (1) Use as a method
```javascript
var obj = {
  //
  i: 10,
  b: () => console.log(this.i, this),
  c: function () {
    console.log(this.i, this);
  },
};
obj.b(); // window
obj.c(); // 10
```
<br>

```This``` of an arrow function defined as a method does not point to ```obj```, the object that owns the method, but points to ```window```, the upper scope.
<br>

#### (2) use ```new``` Operator
Arrow function cannot be used as constructors, and an error occurs if used with ```new```.
```javascript
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```
