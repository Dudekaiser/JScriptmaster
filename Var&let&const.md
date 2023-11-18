## When we use var, let and const?
### 1) Var
#### (1) ```var``` can be just omitted.
```javascript
(var) x = 1; // with () can be omitted.
console.log(x); // 1
```
<br>

#### (2) Re-declaration and re-allocation are possible.
```javascript
var a = "1";
console.log(a); //1

var a = "2";
console.log(a); //2

a = "3";
console.log(a); //3
```
<br>

#### (3) ```Var``` is function level scope.
* ```Function level scope``` means that when it is used within a function, it can be accessed anywhere except outside the function, and all variables declared outside the function are global variables.
```javascript
function f() {
  var hello = "hello world";
  console.log(hello);
}
f(); // hello world
console.log(hello); // ReferenceError
```
* Since ```hello``` is a function level scope declared with var, a ReferenceError occurs if ```hello``` is accessed from outside the function.

### 2) let & const
* To solve the problems described above, from ES6, block level scope can be used with the introduction of ```let``` and ```const```.
<br>

#### (1) Redeclaration is impossible for both ```let``` and ```const```.
```javascript
let a = 1;
let a = 2; // SyntaxError

const b = 1;
const b = 2; // SyntaxError
```
<br>

#### (2) Reallocation is possible for ```let```, but not for ```const```.
```javascript
let a = 1;
a = 2;
console.log(a); // 2

const b = 1;
b = 2;
console.log(b); // TypeError
```
<br>

#### (3) ```let```, ```const``` are block level scope.
* ```Block level scope``` means that variables declared within all code blocks (functions, if, for, while, try/catch, etc.) are valid only within the code block and cannot be referenced outside the code block.
* Variables declared inside a code block are local variables.
```javascript
function f() {
  if (true) {
    let a = 11;
  }
  console.log(a); // ReferenceError
}
```
<br>

* Because the ```let``` keyword is valid only within the block, a ReferenceError is output. (same for const)
<br>

### 3) Hoisting
* A variable declaration anywhere in a scope is equivalent to one declared at the top level of that scope.
```javascript
var a = 10;
function f() {
  var a; // hosting
  console.log(a); // undefined
  a = 20;
  console.log(a); // 20
}
f();
```

#### (1) Hoisting is slightly different for ```let``` and ```const```.
* Declaration and initialization of ```let``` are separated.
* It registers a variable in the scope, but the initialization step is performed when the variable declaration is reached.
* When ```hoisting```, the declaration is performed at the top of the scope and a ReferenceError occurs because it has not been initialized.
```javascript
console.log(a); // ReferenceError
let a = 10; // Executing initialization steps in variable declarations
```
<br>

#### (2) Variables cannot be referenced from the start of scope to the start of initialization.
* This section is called the Temporal Dead Zone (TDZ). ```let``` and ```const``` are constructs affected by TDZ.
```javascript
// 					TDZ
console.log(a); // ReferenceError	TDZ
let a = 10;
```
<br>

```javascript
// 					TDZ
console.log(a); // ReferenceError	TDZ
const a = 10;
```
<br>

* The above code performs ```hoisting```, but references are made before initialization, so a ReferenceError is output.
<br>

### 4) ```var``` vs ```let``` vs ```const```

* Basically, it is recommended to use ```const``` when declaring variables and ```let``` to use when reassignment is necessary.
* Using the ```const``` keyword is safer because it prevents unintentional reallocation.
* ```Let``` is used only when reallocation is necessary.
* At this time, the scope of the variable is made as narrow as possible.
* Use ```const``` for constants and objects that do not change.
