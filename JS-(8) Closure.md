## 8. Closure
* Combination of a function and the lexical environment in which the function is declared.

### 1) Lexical Scoping
* When functions are overlapped, if there is no identifier in the inner function due to scope chain, the identifier is searched in the upper scope.
```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); 
bar();
```
<br>

* Since ```x``` is not defined in ```bar()```, move to the upper scope through the scope chain and check where ```x``` is declared.
* If the upper scope of ```bar()``` is ```foo()```, ```x``` will be 10, and if the ```upper``` scope is global, ```x``` will be 1.
* Then how ```bar()``` can determine the upper scope?
  - A method of determining the upper scope depending on where the function is called (Dynamic scope)
  - A method of determining the upper scope depending on where the function is declared (Lexical or Static scope)

* Mostly, choosed ```Lexical scope```.
* Since it is determined based on where it is declared, the upper scope of ```bar()``` is the global scope and the value of ```x``` is 1.
<br>

### 2) Closure
* By remembering the lexical scope to which the function belongs, we can access this scope even when the function is executed outside the lexical scope.
* In this case, the lexical scope that the function can access is called a ```closure```.
```javascript
function outter() {
  var title = "coding everybody";
  return function () {
    console.log(title);
  };
}
var inner = outter();
inner();
```
<br>

* The result (return) of executing the external function ```outter()``` is stored in the ```inner``` variable. When we call function contained in ```inner``` variable ```function() { alert(title);}```, ```coding everybody``` is output.
* But when the function contained in the ```inner``` variable is called, ```inner``` accesses ```title``` that exists in the outer function ```outter()``` even though the ```outter()``` function has already ended.

 ### 3) Conjugation Closure
 #### (1) information hiding
 ```javascript
function factory_movie(title) {
  return {
    get_title: function () {
      return title;
    },
    set_title: function (_title) {
      title = _title;
    },
  };
}
ghost = factory_movie("Ghost in the shell");
matrix = factory_movie("Matrix");

console.log(ghost.get_title());
console.log(matrix.get_title());

ghost.set_title("squad");

console.log(ghost.get_title());
console.log(matrix.get_title());
```
<br>

* The output is 
```html
Ghost in the shell
Matrix
Squad
Matrix
```
<br>

* Closure can also be used in object methods. This example above returns an object with ```get_title``` and ```set_title``` methods. These methods use the local variable ```title``` sent as an argument value to the external function ```factory_movie```.
* Internal functions or methods created within the same external function share the local variables of the external function. The local variable title was changed to "squad" using ```ghost.set_title('squad');```. The reason why the output value of the second ```console.log(ghost.get_title());``` = "squad" is the ```set_title``` and ```get_title``` methods share the value of the local variable ```title```.
* The reason why ```get_title``` values of ```ghost``` and ```matrix```, which share the same external function ```factory_movie```, are different is because a closure containing a new local variable is created every time the external function is executed, so ```ghost``` and ```matrix``` become completely independent entities.
* Only objects created through ```factory_movie``` method can read and modify the value of the local variable ```title``` of ```factory_movie```.
* Since the function is terminated when ```factory_movie``` returns a certain value, the local variable ```title``` becomes a ```private``` variable that can only be accessed through ```factory_movie```'s internal functions, ```get_title``` and ```set_title```.

#### (2) Example
```javascript
var arr = [];

for (var i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```
* The output is 5 5 5 5 5.
* If we interpret the for statement,
```javascript
arr[i] = function () { return i; };
i = i + 1; // i = 1
arr[i] = function () { return i; };
i = i + 1; // i = 2
arr[i] = function () { return i; };
i = i + 1; // i = 3
arr[i] = function () { return i; };
i = i + 1; // i = 4
arr[i] = function () { return i; };
i = i + 1; // i = 5
```
* Then ```i = 5```. If we interpret the second for statement,
```javasccript
console.log(arr[0]());
console.log(arr[1]());
console.log(arr[2]());
console.log(arr[3]());
console.log(arr[4]());
```
* When we call each function, in ```function() {return i}``` ```i``` refers to the global variable ```var i```, so 5 is output 5 times.
* If we change like below, it will be perfect.
```javascript
var arr = [];
for (var i = 0; i < 5; i++) {
  arr[i] = (function (id) {
    return function () {
      return id;
    };
  })(i);
}
for (var index in arr) {
  console.log(arr[index]());
}
```
* In array ```arr``` function is returned by ```IIFE```
* At this point, ```IIFE``` receives ```i``` as argument, assigns to parameter ```id```, returns internal function then life-cycle will be terminated. The parameter ```id``` will be a free variable.
* The function assigned to the array ```arr``` returns ```id```. At this time, ```id``` is a free variable of the upper scope, so its value is maintained.
<br>

* If we interpret ```for```statment,
```javascript
arr[i] = (function (id) { return function () { return id; };})(i); 
// var id = 0
i = i + 1;
arr[i] = (function (id) { return function () { return id; };})(i); 
// var id = 1
i = i + 1;
arr[i] = (function (id) { return function () { return id; };})(i); 
// var id = 2
i = i + 1;
arr[i] = (function (id) { return function () { return id; };})(i); 
// var id = 3
i = i + 1;
arr[i] = (function (id) { return function () { return id; };})(i); 
// var id = 4
i = i + 1;
```
<br>

* When the ```arr[index]()``` function is called, 0 1 2 3 4 is output because it returns a local variable ```id``` rather than a function that returns the global variable ```i```.

 
