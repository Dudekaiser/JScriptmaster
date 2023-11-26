## 13. Operator

### 1) Short Circuit Evaluation
* When we use ```&&```, if the left value is true, return right value.
* When we use ```&&```, if the left value is false, return left value.
* When we use ```||```, if the left value is true, return left value.
* When we use ```||```, if the left value is false, return right value.
<br>

```javascript
console.log(true || 'hello');        // true
console.log(false || 'hello');       // hello

console.log(true && 'hello');        // hello
console.log(false && 'hello');      // false

console.log(true && true && 'hello');    // hello
console.log(true && false && 'hello;);    // false
```

### 2) ** Operator
```javascript
console.log(10 ** 3); // 10^3 = 1000
```

### 3) null operator
* left value ?? right value : if left value is null or undefined, return right value.

```javascript
let name;
console.log(name);

name = name ?? 'cho';
console.log(name);       // cho

name = name ?? 'choi';
console.log(name);      // cho, because value name is no longer undefined or null

let name2;
name2 ??= 'kim';
console.log(name2);      // kim
```





