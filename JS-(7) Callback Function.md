## 7. Callback Function

### 1) Asynchronous processing
* Executes the next code first without stopping the execution of the code until the operation of a specific code is completed.

#### (1) First example
```javascript
function getData() {
  var tableData;
  $.get("https://domain.com/products/1", function (response) {
    tableData = response;
  });
  return tableData;
}

console.log(getData()); // undefined
```
<br>

* ```$.get()``` is the ```ajax``` communication part, and this is the code that sends a request to ```https://domain.com``` to request product information.
* The received data is contained in the ```response``` argument. And ```tableData = response;``` saves the received data in the ```tableData``` variable.
* Next step, When we run ```console.log(getData())```, the received data should be output, but we can see that undefined is output.
<br>

* Undefined is output because ```tableData = response``` is executed without waiting for data to be requested and received with ```$.get()```.
* Nevertheless, the reason why asynchronous processing is necessary is because when data is requested from the screen to the server, we cannot wait forever without knowing when the server will respond to the request.

#### (2) Second example : ```setTimeout()```
* Instead of executing the code right away, it waits for the specified time and then executes the code.
```javascript
console.log("Hello");
setTimeout(function () {
  console.log("Bye");
}, 3000);
console.log("Hello Again");
```
<br>

* Output is Hello -> Hello Again -> (after 3s) Bye
<br>

* ```setTimeout()``` receives a callback function as an argument and is executed asynchronously.
* Therefore, NOT waiting 3 seconds after outputting Hello and then outputting Bye, after outputting Hello, BUT ```setTimeout()``` is executed, Hello Again is output, and then Bye is output 3 seconds later.

### 2) Solving problems with asynchronous processing using callback functions
* In the first example, we saw a problem caused by the asynchronous processing method, and the solution to this problem is to use a ```callback``` function.
* ```callback``` function is a function that is called at a certain time to solve problems with JavaScript's asynchronous processing method.
```javascript
function getData(callbackFunc) {
  $.get("https://domain.com/products/1", function (response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
  });
}

getData(function (tableData) {
  console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```
<br>

* Using a callback function like this, we can execute the desired action when specific logic is completed.
<br>

### 3) Callback Hell
* A phenomenon in which the indentation level of the code becomes unbearably deep as the process of passing a callback function to an anonymous function is repeated.
```javascript
setTimeout(
  function (name) {
    let coffeeList = name;
    console.log(name);

    setTimeout(
      function (name) {
        coffeeList += ", " + name;
        console.log(name);

        setTimeout(
          function (name) {
            coffeeList += ", " + name;
            console.log(name);

            setTimeout(
              function (name) {
                coffeeList += ", " + name;
                console.log(name);
              },
              500,
              "Latte"
            );
          },
          500,
          "Mocha"
        );
      },
      500,
      "Americano"
    );
  },
  500,
  "Espresso"
);
// Espresso
// Espresso, Americano
// Espresso, Americano, Mocha
// Espresso, Americano, Mocha, Latte
```
<br>

* This example has excessively deep indentation levels and values are sent from bottom to top, making it difficult to read.
<br>

* There are several ways to solve this ```callback hell```, 1. express as a registered(not secret) function, 2. ```Promise``` introduced from ES6, and ```async/await``` introduced from ES8.
<br>

#### (1) Solving ```callback hell``` with ```registered functions```
```javascript
let coffeeList = "";

const addEspresso = function (name) {
  coffeeList = name;
  console.log(coffeeList);
  setTimeout(addAmericano, 500, "americano");
};

const addAmericano = function (name) {
  coffeeList += ", " + name;
  console.log(coffeeList);
  setTimeout(addMocha, 500, "mocha");
};

const addMocha = function (name) {
  coffeeList += ", " + name;
  console.log(coffeeList);
  setTimeout(addLatte, 500, "latte");
};

const addLatte = function (name) {
  coffeeList += ", " + name;
  console.log(coffeeList);
};

setTimeout(addEspresso, 500, "espresso");
// Espresso
// Espresso, Americano
// Espresso, Americano, Mocha
// Espresso, Americano, Mocha, Latte
```
<br>

* Changing an anonymous ```callback function``` to a registered function improves readability and eliminates indentation.
* However, if there are 100 types of coffee, creating 100 functions is burdensome.


