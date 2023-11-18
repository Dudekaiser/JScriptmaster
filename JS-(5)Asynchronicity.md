## 5. JavaScript asynchronous processing method

### 1) Asynchronicity
* Executing the next code first without stopping the execution of the code until the operation of a specific code is completed.
* Javascript is a single-thread language.
* This means that there is only one ```call stack``` where the called functions accumulate, which means that Javascript can only perform one thing at a time.

### 2) Call stack
* Javascript Engine consists of ```Memory Heap``` and ```Call Stack```.
* Memory Heap : Storage space for functions, arrays, objects, etc. created by using JS
* Call Stack: Where function execution calls are accumulated. When the call is over, it disappears.

```javascript
function a() {
  b();
  console.log("a");
}

function b() {
  console.log("b");
}
a();
```

### 3) Runtime
* Runtime operation order : Call stack -> Web API -> Callback Queue -> Event Loop operation -> Call stack

#### (1) Web API
* Executes asynchronous tasks such as ```AJAX``` or ```Timeout```.
* When a function such as ```setTimeout``` is executed in JavaScript, the JavaScript engine requests ```setTimeout``` from the ```Web API``` and at the same time transmits the ```Callback``` entered in ```setTimeout```.
* In the ```call stack```, the ```setTimeout``` operation is completed and removed after the ```Web API``` request.
* ```Web API``` completes the ```setTimeout``` just received, and at the same time sends the received ```Callback``` to a place called ```Task Queue```.

#### (2) Task Queue and Event Loop
* ```TaskQueue``` is also called ```Callback Queue```. 
* Stores the ```callback``` function received from ```Web API``` in the form of a queue.
* These ```callback``` functions are added to the ```call stack``` in order when all tasks in the JavaScript engine's ```call stack``` are completed.
* At this time, the ```Event Loop``` determines whether there is a ```task``` running in the ```call stack``` and whether a ```task``` exists in the ```task queue```, and moves the ```task``` from the ```task queue``` to the ```call stack```.

