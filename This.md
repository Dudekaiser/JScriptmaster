## 1. This
* In object-oriented languages such as Java, ```this``` refers to the class (the instantiated object) itself.
* In JavaScript, the object to bind to ```this``` is dynamically determined depending on the function call method.
<br>

 How to invoke a function?
* call function
* call method
* call generator function
* call apply/call/bind
<br>

### 1) call function
* The global object refers to the top-level object of all objects, and in browsers, it means ```window```.
```javascript
this === window; //true
```
<br>

* A function declared in the global scope is a method of a global variable that can be accessed as a property of the global object.
```javascript
var i = "Global variable";

console.log(i);
console.log(window.i);

function inv() {
  console.log("invoked!");
}
window.inv();
```
<br>

* ```This``` is bound to the global object. In the case of global functions as well as internal functions, ```this``` is bound to the global object.
```javascript
function hey() {
  console.log("hey's this: ", this); // window
  function there() {
    console.log("there's this: ", this); // window
  }
  there();
}
hey();
```
* Even for case of internal functions, ```this``` is ```window```
```javascript
var val = 1;

var obj = {
  value: 100,
  hey: function () {
    console.log("hey's this: ", this); // obj
    console.log("hey's this.val: ", this.val); // 100
    function there() {
      console.log("there's this: ", this); // window
      console.log("there's this.val: ", this.val); // 1
    }
    there();
  },
};

obj.hey();
```
* Even in the case of call-back functions, ```this``` is bound to the global object.
```javascript
var value = 1;

var obj = {
  value: 100,
  hey: function () {
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  },
};

obj.hey();
```
* Regardless of whether an internal function is declared as a general function, method, or callback function, ```this``` binds the global object.
<br>

* What should we do if we want to avoid the ```this``` of an internal function referencing the global object?
```javascript
var value = 1;

var obj = {
  value: 100,
  foo: function () {
    var that = this; //  this === obj

    console.log("hey's this: ", this); // obj
    console.log("hey's this.value: ", this.value); // 100
    function hey() {
      console.log("there's this: ", this); // window
      console.log("there's this.value: ", this.value); // 1

      console.log("there's that: ", that); // obj
      console.log("there's that.value: ", that.value); // 100
    }
    there();
  },
};

obj.hey();
```
* We can store ```this```, which points to ```obj```, in the ```that``` variable and then use ```that``` in the internal function.
* Alternatively, we can use ```apply``` or ```invoke```.

### 2) call Method
* When a method is invoked, it is bound to the object that invoked that method.
```javascript
function hey() {
  this.check = function () {
    console.log("this is", this);
  };
}

var v = new hey();
v.check(); // this is hey {check: v}
```
<br>

### 3) call generator function
* The generator function in JavaScript is responsible for creating an object.
* If we call an existing function with the ```new``` operator, the function operates as a generator function.
* Even if we call a normal function with the ```new``` operator, it can operate like a generator function, so the first letter of a generator function is generally capitalized.
* ```This``` is bound to the object created by the generator function.
```javascript
function Person(name) {
  this.name = name;
}

var tom = new Person("tom");

console.log(tom.name); // {name: tom}
```
<br>

* However, if the ```new``` keyword is omitted, it does not function as a generator function.
```javascript
var tom = Person("tom");

console.log(tom.name); // undefined
```
<br>

### 4) Call apply/call/bind
* The object bound to ```this``` is implicitly determined by the JavaScript engine based on the function call pattern.
* ```Apply``` and ```call``` are ways to explicitly bind ```this``` to a specific object.
* ```Apply``` and ```call``` are both methods of the ```Function.prototype``` object.
<br>

*Function.prototype.call()
```javascript
func.call(thisArg[, arg1[, arg2[, ...]]])

// thisArg : Object to bind to this inside the function
// arg1, arg2, ... : Arguments to send to the function
<br>

* In following code, Although the function of person2 is being called, since we enter person1 in the first parameter of the ```call``` function, ```this``` will point to person1.
```javascript
var person1 = {
  name: "Cho",
};

var person2 = {
  name: "Kim",
  study: function () {
    console.log(this.name + "is studying.");
  },
};

person2.study(); // Kim is studying.

// call()
person2.study.call(person1); // cho is studying.
<br>

* Function.prototype.apply()
```javascript
func.apply(thisArg, [argsArray]);

// thisArg: Object to bind to this inside the function
// argsArray: Array of arguments to be sent to the function
```
* ```apply``` is same as ```call``` except that the second parameter is entered in the form of an array.
<br>

### 5) THIS in arrow function
* ```This``` in arrow function is different from ```this``` in normal functions.
* When we declare an ```arrow``` function, the object to bind to ```this``` is statically determined.
* Unlike normal functions, ```this``` of an arrow function always points to the ```this``` of the parent scope.
*  This is called Lexical ```this```.
```javascript
function Person() {
  this.age = 0;

  setTimeout(() => { this refers to the Person object, which is the parent scope.
    this.age++; // 
    console.log(this.age);
  }, 1000);
}

Person(); // 1
``` 
