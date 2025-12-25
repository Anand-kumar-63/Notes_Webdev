These two functions are the standard way to handle **timing** in JavaScript. They work together like setting a timer on your phone and then cancelling it before it goes off.1

Here is the breakdown of how they work, why they are used, and how to use them together.

---
### 1. `setTimeout` (The Timer)2
`setTimeout` allows you to execute a block of code once after a specified amount of time has passed.3 It is **asynchronous**, meaning the rest of your code continues to run while the timer counts down in the background.4
JavaScript
```js
let timerId = setTimeout(function, delayInMilliseconds);
```
- **function:** The code you want to run.
- **delay:** How long to wait (1000 ms = 1 second).5
- **Returns:** A unique numeric **ID** (token) representing that specific timer.6

**Example:**
JavaScript
```js
console.log("1. Start");
setTimeout(() => {
  console.log("2. This runs after 3 seconds");
}, 3000);
console.log("3. End");
// Output order:
// "1. Start"
// "3. End"
// ... (3 seconds later) ...
// "2. This runs after 3 seconds"
```

---
### 2. `clearTimeout` (The Cancel Button)
`clearTimeout` is used to stop a timer **before** it has a chance to run.7 If the timer has already executed, calling this does nothing.
To use it, you must pass the **ID** that `setTimeout` returned.
JavaScript
```js
clearTimeout(timerId);
```
Example:
Imagine a user clicks a "Buy" button, but then quickly clicks "Cancel";
JavaScript
```js
console.log("Transaction started...");

// 1. Schedule the transaction for 5 seconds later
const transactionId = setTimeout(() => {
  console.log("Transaction Complete! $$$");
}, 5000);

// 2. Logic to cancel the transaction (e.g., user clicked a button)
const userCancelled = true;

if (userCancelled) {
  console.log("Cancelling transaction...");
  clearTimeout(transactionId); // The function above will NEVER run
}
```

---
### Common Use Case: React `useEffect`
In modern web development (like React), `setTimeout` and `clearTimeout` are frequently paired to prevent **memory leaks**.
If a component starts a timer but is removed from the screen (unmounted) before the timer finishes, the timer will still try to run, potentially causing errors.
JavaScript
```js
import { useEffect, useState } from 'react';

function Notification() {
  const [visible, setVisible] = useState(true);

  useEffect(() => {
    // Set a timer to hide the notification after 3 seconds
    const timer = setTimeout(() => {
      setVisible(false);
    }, 3000);

    // CLEANUP FUNCTION
    // If the user closes the page before 3 seconds, this runs
    // and kills the timer so it doesn't try to update state on a closed component.
    return () => clearTimeout(timer);
  }, []);

  if (!visible) return null;
  return <div>Welcome back!</div>;
}
```
### Summary Table

| **Feature**  | **setTimeout**               | **clearTimeout**              |
| ------------ | ---------------------------- | ----------------------------- |
| **Purpose**  | Schedules code to run later. | Cancels a scheduled code run. |
| **Returns**  | A unique Number (ID).        | `undefined`.                  |
| **Argument** | Function + Delay (ms).       | The ID from `setTimeout`.     |
| **Behavior** | Runs only once.              | Prevents execution.           |
