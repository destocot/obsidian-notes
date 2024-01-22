```javascript
/**
 * setTimeout
 */
const myFunction = function () {
  console.log("times up");
};
const delay = 1000; // one second
setTimeout(myFunction, delay);

// clearTimeout
const timeoutId = setTimeout(myFunction, delay * 5);
// nothing runs since the timeout immediately
clearTimeout(timeoutId);
```

```javascript
/**
 * setInterval
 */
let countDown = 3;

// intervalId needed for clearInterval
const intervalId = setInterval(() => {
  console.log(countDown);
  countDown -= 1;

  // clearInterval
  if (countDown === 0) {
    clearInterval(intervalId);
    console.log("Blast Off!");
  }
}, delay);
```

```javascript
/**
 * Debounce
 */
let debounceTimeoutId;
const myDebounceFunction = function (num) {
  clearInterval(debounceTimeoutId);
  debounceTimeoutId = setTimeout(() => {
    console.log("debounce function is ran", num);
  }, 1000);
};

myDebounceFunction(1000);
// will clear previous function
myDebounceFunction(1001);
// will clear previous function
myDebounceFunction(1002);
// "debounce function is ran 1002"

// reusable debounce
const debounce = (callback, delay) => {
  let timeoutId;
  return (...args) => {
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    timeoutId = setTimeout(() => {
      callback(...args);
    }, delay);
  };
};
```

```javascript
/**
 * Throttling
 */

// to ensure something is not called more frequently
// than a certain delay
```

```javascript
/**
 * Request Animation Frame
 */

// requestAnimationFrame(callback)
// will run before next browser paint
const boxAnimationFrame = document.getElementById("boxAnimationFrame");

let animationFrameAngle = 0;

let previousTime;
function animateWithAnimationFrame(currentTime) {
  console.log(currentTime - previousTime);
  previousTime = currentTime;
  boxAnimationFrame.style.transform = "rotate(" + animationFrameAngle + "deg)";
  animationFrameAngle += 2;
  // will be in sync with monitor refresh rate
  requestAnimationFrame(animateWithAnimationFrame);
}

requestAnimationFrame(animateWithAnimationFrame);
// runs only once


```