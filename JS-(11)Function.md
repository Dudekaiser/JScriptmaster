## 11. Function Declaration and Function Expression

### 1) Function Declaration
With ```function keyword we can define.
```javascript
function print(a, b) {
  return a + b;
}
print(10, 20); // 30
```
<br>

### 2) Function Expression
* JavaScript functions can be defined using expressions, and function expressions can be stored as variables.
```javascript
let print = function (a, b) {
  return a + b;
};
console.log(print(10, 20)); // 30
```
<br>

* If a function is defined as a function expression, the function name can be omitted. These functions are called ```anonymous functions```. It is common to use anonymous functions in function expressions.
```javascript
// anonymous function
let print = function (a, b) {
  return a + b;
};
// named function(registered function)
let value = function sum(a, b) {
  return a + b;
};
// print
console.log(print(10, 20)); // 30
console.log(sum(10, 20)); // Uncaught ReferenceError: sum is not defined
```
<br>

* Because a function is an object, the variable containing the function stores a reference value pointing to the function, not the function name. Therefore, when calling a function, you must use the variable name that points to the function, not the function name.
```javascript
console.log(value(10, 20)); // 30
```
<br>

### 3) Function Hoisting
JavaScript hoists all declarations (var, let, const, function, function*, class), including let and const in ES6. ```Hoisting``` is a characteristic that operates as if all declarations, such as var declarations or function declarations, have been moved to the top of the scope.

#### (1) Function Declaration
A function declared using a function declaration expression is hoisted, and the function itself is hoisted.
```javascript
sum(10, 20);
function sum(a, b) {
  return a + b;
}
```
This example actually same as below example
```javascript
function sum(a, b) {
  return a + b;
}
sum(10, 20);
```

#### (2) Function Expression
In the case of function expressions, variable ```hoisting``` occurs, not function ```hoisting```.
```javascript
sum(10, 20);			// TDZ
let sum = function (a, b) {	// TDZ
  return a + b;
};

// Uncaught TypeError: sum is not a function
```
Therefore, ```hoisting``` is performed on the let variable, and let is a syntax affected by ```TDZ```. Therefore, ```hoisting``` is performed, but a ReferenceError is output because the reference is made before initialization.
<br>

* Then what happens when the keyword ```var``` is used instead of ```let```?
```javascript
sum(10, 20);
var sum = function (a, b) {
  return a + b;
};
// Uncaught TypeError: sum is not a function
```
<br>

This example actually acts like below example
```javascript
var sum;
sum(10, 20);
sum = function (a, b) {
  return a + b;
};
```
<br>

We declare the variable ```sum``` and call ```sum(10,20)```, but because ```sum``` is ```undefined``` in this state, the message ```sum is not a function``` appears.

### 4) Advantages of Function Expression
In addition to being unaffected by hoisting, function expressions are sometimes more useful than function declarations.
* Used as a closure
* Used as a callback (can be sent as an argument to another function)

#### (1) Used as a closure
A closure is used when we want to send a variable to a function before executing the function.
```javascript
function tabsHandler(index) {
  return function tabClickEvent(event) {
    // 바깥 함수인 tabsHandler() 의 index 인자를 여기서 접근할 수 있다.
    console.log(index); // 탭을 클릭할 때 마다 해당 탭의 index 값을 표시
  };
}

var tabs = document.querySelectorAll(".tab");
var i;

for (i = 0; i < tabs.length; i++) {
  tabs[i].onclick = tabsHandler(i);
}
```
<br>

This example adds a click event to every ```.tab``` element. The argument value ```index``` of the outer function ```tabsHandler()``` was accessed in ```tabClickEvent()``` using a closure. 
<br>

After the execution of the ```for``` loop statement is completed, ```tabClickEvent()``` is executed when the user clicks the ```tab```. If ```closure``` had not been used, the ```index``` value of all ```tabs``` would have been ```tabs.length```.
```javascript
for (i = 0; i < tabs.length; i++) {
  tabs[i].onclick = tabsHandler(i);
}
```
<br>

The example below does not use closures.
```javascript
var tabs = document.querySelectorAll(".tab");
var i;

for (i = 0; i < tabs.length; i++) {
  tabs[i].onclick = function (event) {
    console.log(i); // No matter which tab we click, ```tabs.length``` (the final value of i) is always displayed.
  };
}
```
<br>

In this code, no matter which tab we click, ```i``` is set to ```tabs.length```, which is the final value of the ```for``` loop.
<br>

To make it easier to see, take the ```function()``` inside the ```for``` statement outside and declare it.
```javascript
var tabs = document.querySelectorAll(".tab");
var i;
var logIndex = function (event) {
  console.log(i); // 3
};

for (i = 0; i < tabs.length; i += 1) {
  tabs[i].onclick = logIndex;
}
```
<br>

When ```logIndex``` is executed, the execution of the ```for``` statement has already finished, so ```tabs.length```, which is the final value of the ```for``` statement, is output no matter which ```tab``` is pressed.
<br>

Let's apply closure to solve this problem.
```javascript
function tabsHandler(index) {
  return function tabClickEvent(event) {
    // The index argument of the outer function tabsHandler can be accessed here.
    console.log(index); // Whenever a tab is clicked, the index value of that tab is displayed.
  };
}

var tabs = document.querySelectorAll(".tab");
var i;

for (i = 0; i < tabs.length; i += 1) {
  tabs[i].onclick = tabsHandler(i);
}
```
<br>

When the ```for``` loop is executed, ```i``` value is sent to ```tabsHandler()```, and the argument value ```index``` of ```tabsHandler()``` can be accessed in the closure ```tabClickEvent()```. 
Therefore, we can access the ```index``` of each tab we want.
<br>

#### (2) Used as callback
It can be passed to an argument of another function.
<br>

Function expression is generally stored in temporary variable.
<br>

```javascript
var value = function () {
  // ...
};
```
We can use a function expression as a ```callback``` function as follows without putting it in a temporary variable
```javascript
$(document).ready(function () {
  console.log("An anonymous function"); // 'An anonymous function'
});
```
<br>

Below example has same meaning as above.
```javascript
var logMessage = function () {
  console.log("An anonymous function");
};

$(document).ready(logMessage); // 'An anonymous function'
```
<br>

We can also use a ```callback``` function when using ```forEach()```. ```Foreach()``` is a built-in JavaScript API.
