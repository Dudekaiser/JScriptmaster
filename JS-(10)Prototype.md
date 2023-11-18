## 10. Prototype
* All objects in JavaScript are connected to the object that serves as their parent. This allows us to inherit and use the properties or methods of the parent object, just like the concept of object-oriented inheritance.
* This parent object is called a Prototype object or ```Prototype```.
<br>

* Since function is object in Javascript, below 2 codes have same meaning.
```javascript
function Person() {}
```
<br>

```javascript
var Person = new Function();
```
<br>

* Object is generated always as function,
```javascript
function Person() {}
var personObject = new Person(); // as function object is generated
```
* The object ```personObject``` is generated as function ```Person```.
```javascript
var obj = {};
```
* This object generation code actually is same as below :
```javascript
var obj = new Object();
```
* ```Object``` is a function that offered in javascript. ```Function```, ```Array``` are both defined as function, too.

### 1) Example
```javascript
function Person(name, first, second) {
  this.name = name;
  this.first = first;
  this.second = second;
  this.sum = function () {
    return this.first + this.second;
  };
}
var kim = new Person("kim", 10, 20);
var lee = new Person("lee", 10, 10);
console.log("kim.sum()", kim.sum());
console.log("lee.sum()", lee.sum());
```
<br>

* The function ```sum``` is created newly every time when function ```Person``` operates as generator.
* Here, there are only kim and lee, but if we imagine there are 100 or 1000, it is a huge waste of memory. These problems can be solved with ```prototype```.

```javascript
function Person(name, first, second ){
    this.name = name;
    this.first = first;
    this.second = second;
    }
}
Person.prototype.sum = function(){
    return this.first + this.second;
}
var kim = new Person('kim', 10, 20);
var lee = new Person('lee', 10, 10);
console.log("kim.sum()", kim.sum());
console.log("lee.sum()", lee.sum());
```
<br>

* There is ```Object``` called ```Person.prototype```, and objects (kim, lee) created with ```Person``` function can use the values which are contained in ```Object```.

### 2) Prototype Object
* When we define ```Person``` function, not only a function is created, but a Prototype Object of ```Person``` is also created internally.
  - ```Person``` function has a property called ```prototype```, and ```Person```'s ```Prototype Object``` cross-references each other with a property called ```constructor```.
  - ```Prototype Object``` has ```constructor``` and __proto__ property.
  - ```Constructor``` points to the function ```Person``` created like ```Prototype Object```, and __proto__ is also called ```Prototype Link```.
  - It points to the ```Prototype Object``` of the parent that created it.
<br>

```javascript
function Person(name, first, second ){
    this.name = name;
    this.first = first;
    this.second = second;
    }
}
Person.prototype.sum = function(){
    return this.first + this.second;
}
var kim = new Person('kim', 10, 20);
var lee = new Person('lee', 10, 10);
console.log("kim.sum()", kim.sum());
console.log("lee.sum()", lee.sum());
```
<br>

__proto__ of kim and lee points to Person's ```Prototype Object```, so ```Person.prototype``` can be referenced.

### 3) Characteristics of Prototype Object
```Person```'s ```Prototype Object``` duplicates information at the time of generation.
```javascript
var person = function () {
  this.hp = function () {
    console.log("100");
  };
};

person.hp = function () {
  console.log("50");
};

var p1 = new person();
p1.hp(); // 100
```
The reason the result is 100 is because the Prototype Object duplicates the information at the time of creation, that is, the HP at the time of Person's creation is 100.
<br>

If we want to have 50 as output, what should we do?
```javascript
var person = function () {};

person.hp = function () {
  console.log("100");
};

person.prototype.hp = function () {
  console.log("50");
};
var p1 = new person();
p1.hp(); // 50
```
<br>

Just access to ```Prototype Object``` directly. ```Prototype Object``` at the time of generation had no value and was changed to 50 after the code ```person.prototype.hp = function() {console.log("50");```}, so the output value becomes 50.
<br>

* ```prototype``` : Its Prototype Object
* ```constructor``` : Literally as a "Constructor(Generator)", points to a function that was created with Prototype Object.
* ```__proto__``` : It points to the Prototype Object of the parent that created it. All objects have it by default.
