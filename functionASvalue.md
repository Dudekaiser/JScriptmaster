## 3. Function as Value?

### 1) In Javascript, functions are also objects.
* This means that a function is also a type of value.

```javascript
function a() {}
```
<br>

* This code and the code below are same.
```javascript
var a = function() {}
```
<br>

### 2) Because functions are values, they can also be sent as arguments to other functions.
```javascript
function cal(func, num){
    return func(num)
}
function increase(num){
    return num+1
}
function decrease(num){
    return num-1
}
console.log(cal(increase, 1)); // 2
console.log(cal(decrease, 1)); // 0
```
<br>

* ```console.log(cal(increase, 1));``` When we execute, the function ```increase``` and the value ```1``` are sent as arguments to the function ```cal```.
* The function ```cal``` executes increase sent as the first argument, and at this time, ```1``` is sent as the value of the second argument.
* The function ```increase``` returns the calculated result, and ```cal``` returns that value again.

### 3) Functions can also be used as return values of functions.
```javascript
function cal(mode){
    var funcs = {
        'plus' : function(left, right){return left + right;},
        'minus' : function(left, right){return left - right;}
    }
    return funcs[mode];
}
console.log(cal('plus')(2,1)); // 3
console.log(cal('minus')(2,1)); // 1
```
<br>

* ```return funcs[mode]``` serves to return ```function(left, right) {return left + right;}```.

### 4) Functions can also be used as array value.
```javascript
var process = [
    function(input){ return input + 10;},
    function(input){ return input * input;},
    function(input){ return input / 2;}
];
var input = 1;
for(var i = 0; i < process.length; i++){
    input = process[i](input);
}
console.log(input); // 60.5
```
<br>

* Because functions are values, they can also be stored in an array, which is a container for storing values.
* Data that can be used as variables, parameters, and return values are called ```first-class citizens```, ```first-class objects```.


