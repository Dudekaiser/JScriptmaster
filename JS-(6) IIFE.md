## 6. IIFE = Immediately Invoked Function Expression

### 1) IIFE
* Immediate Execution Function. This is a Javascript function that is executed immediately as soon as it is defined.
```javascript
 (function () {
  // our code
})();
```
1. The first is an anonymous function surrounded by bracket (()). This part not only prevents polluting the global scope by adding unnecessary variables, but also prevents that other variables accessing into inside of the IIFE.
2. The second part is the bracket () which creates an immediate execution function. Through this, the JavaScript engine immediately interprets and executes the function.
<br>

### 2) example

#### (1) Possible creating ```private``` variables.
```javascript
(function () {
  var i = "hello world";
})();
console.log(i);
// Uncaught ReferenceError: i is not defined
```
<br>

* Variables defined inside IIFE cannot be accessed from external scope.
<br>

#### (2) We can store an immediately executed function in a variable.
* It is also possible to store the return value of an immediately executed function in a variable.
```javascript
var mySquare = (function (x) {
  return x * x;
})(2);
console.log(mySquare); 
// 4
```
<br>

* And because the immediate execution function is also a function, the immediate execution function can be stored in a variable.
```javascript
(mySquare = function (x) {
  console.log(x * x);
})(2);
mySquare(3);
// 4
// 9
```
<br>

* The function is saved in ```mySquare``` and this function is executed immediately. Because ```mySquare``` stores an immediately executed function, it can be recalled.
* Since the function varies depending on the position of the bracket, it seems necessary to pay attention to the position of the bracket.
<br>

### 3) Why use immediately executed functions?

#### (1) Initialize
* To avoid declaring variables globally, immediately executed functions are often used in parts of initialization code that only need to be executed once.
* Since there is no need to add variables globally, it can be implemented without code conflicts, so it is often used when we create plug-ins or libraries etc.
```javascript
var initText;
(function (number) {
    var textList = ["is Odd Text", "is Even Text"];
    if (number % 2 == 0) {
        initText = textList[1];
    } else {
        initText = textList[0];
    }
})(5);
console.log(initText);
console.log(textList);
// is Odd Text
// Uncaught ReferenceError: textList is not defined
```
<br>

* ```TextList``` is not stored globally, and only ```initText``` is initialized. Also, ```textList``` is a local variable, so it can be initialized without conflict with global variables.
<br>

#### (2) Conflicts in library global variables
* The ```jQuery``` and ```Prototype``` libraries use the same global variable called ```$```, but if these two libraries are used together, a ```$``` variable conflict will occur.
```javascript
(function ($) {
    // $ 는 jQuery object
})(jQuery);
```
<br>

* In the immediately executed function, @$@ becomes a local variable of the ```jQuery``` object rather than a global variable, so it can be used without conflict with ```$``` of the Prototype library.
