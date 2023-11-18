## 5. JavaScript asynchronous processing method

### 1) Asynchronicity
* Executing the next code first without stopping the execution of the code until the operation of a specific code is completed.
* Javascript is a single-thread language.
* This means that there is only one ```call stack``` where the called functions accumulate, which means that Javascript can only perform one thing at a time.

#### (1) Call stack
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
