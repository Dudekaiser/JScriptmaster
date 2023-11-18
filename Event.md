## 4. EVENT
### 1) Event Bubbling
* A phenomenon in which when an event occurs in one element, the handler assigned to this element operates, and then the handler of the parent element operates, and the handler operates repeatedly until the top-parent element is encountered.

 ```javscript
 <body>
    <div class="DIV1">
      DIV1
      <div class="DIV2">
        DIV2
        <div class="DIV3">DIV3</div>
      </div>
    </div>
  </body>
```
<br>

```javascript
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener("click", clickEvent);
});
```
<br>

* This code displays the name of the corresponding class to the console when we click on the ```div```.
* Since bubbling occurs by default in JavaScript, if we click ```<div class="DIV3">DIV3</div>```, first DIV3, then DIV2, and lastly DIV1 will be displayed in the console.

### 2) Event Capturing
* Contrary to bubbling, Event Capturing searches for the corresponding tag from the top tag and goes down.
```javascript
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
  console.log(e.currentTarget.className);
};

divs.forEach((div) => {
  div.addEventListener("click", clickEvent, { capture: true });
});
```
<br>

* Capturing can be implemented by setting ```{ capture: true }``` or ```true``` in the option object of ```addEventListener```.
* If we click ```<div class="DIV3">DIV3</div>```, DIV1, DIV2, and DIV3 will be displayed in that order in the console.

### 3) Prevent event spread
* Bubbling : all handlers are called until a document object is encountered in the target.
* When we write code, sometimes we want to trigger an event only in the desired target.
<br>

* In this case, we can use ```event.stopPropagation()```.
* In the case of bubbling, only the event of the clicked target occurs and the event can be prevented to spread to higher elements.
<br>

* Add ```event.stopPropagation()``` to the code written in bubbling.
```javascript
const clickEvent = (e) => {
  e.stopPropagation();
  console.log(e.currentTarget.className);
};
```
<br>
* Previously, when DIV3 was clicked, the event was propagated so DIV3, DIV2, and DIV1 were output, but this time, we can see that only the event of the clicked target occurs.
* That is DIV3. So only DIV3 is output.
