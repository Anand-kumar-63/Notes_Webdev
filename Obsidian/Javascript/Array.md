## **1. Array Creation & Conversion**

```js
// Array constructor
let arr = new Array(3); // [empty × 3]

// Array.of()
let nums = Array.of(1, 2, 3); // [1, 2, 3]

// Array.from()
let strArr = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']

// Array.isArray()
Array.isArray(nums); // true
```
---
## 🧩 **2. Adding & Removing Elements**

```js
let fruits = ['apple', 'banana'];

// push()
fruits.push('mango'); // ['apple', 'banana', 'mango']

// pop()
fruits.pop(); // ['apple', 'banana']

// unshift()
fruits.unshift('grapes'); // ['grapes', 'apple', 'banana']

// shift()
fruits.shift(); // ['apple', 'banana']

// splice()
fruits.splice(1, 0, 'kiwi'); // insert 'kiwi' at index 1 → ['apple', 'kiwi', 'banana']
fruits.splice(0, 1); // remove 1 element from index 0 → ['kiwi', 'banana']

// slice()
let sliced = fruits.slice(0, 1); // ['kiwi'] (non-mutating)

// concat()
let newArr = fruits.concat(['orange', 'papaya']); // ['kiwi', 'banana', 'orange', 'papaya']

```
---
## 🔍 **3. Searching & Checking**

```js
let numbers = [10, 20, 30, 40, 50];

// indexOf()
numbers.indexOf(30); // 2

// lastIndexOf()
numbers.lastIndexOf(50); // 4

// includes()
numbers.includes(20); // true

// find()
numbers.find(num => num > 25); // 30

// findIndex()
numbers.findIndex(num => num > 25); // 2

// findLast() (ES2023)
numbers.findLast(num => num < 45); // 40

// findLastIndex() (ES2023)
numbers.findLastIndex(num => num < 45); // 3

// some()
numbers.some(num => num > 40); // true

// every()
numbers.every(num => num > 5); // true

```
---
## 🔁 **4. Iteration & Transformation**

```js
let scores = [5, 10, 15];

// forEach()
scores.forEach(score => console.log(score * 2)); // 10, 20, 30

// map()
let doubled = scores.map(score => score * 2); // [10, 20, 30]

// filter()
let highScores = scores.filter(score => score > 8); // [10, 15]

// reduce()
let total = scores.reduce((sum, curr) => sum + curr, 0); // 30

// reduceRight()
let reversedTotal = scores.reduceRight((sum, curr) => sum + curr, 0); // 30 (same result here)
```
---
## 🧮 **5. Sorting & Reordering**

```js
let nums2 = [3, 1, 4, 2];

// sort()
nums2.sort(); // [1, 2, 3, 4]

// reverse()
nums2.reverse(); // [4, 3, 2, 1]

// toSorted() (ES2023)
let sortedCopy = nums2.toSorted(); // [2, 3, 4, 1] → returns new array, doesn’t mutate original

// toReversed() (ES2023)
let reversedCopy = nums2.toReversed(); // reversed version, non-mutating

// toSpliced() (ES2023)
let newList = nums2.toSpliced(1, 2, 99); // removes 2 items from index 1, inserts 99
// [4, 99, 1]
```
---
## 🧱 **6. Flattening & Combining**

```js
let nested = [1, [2, [3, [4]]]];

// flat()
nested.flat(2); // [1, 2, 3, [4]]

// flatMap()
let doubledFlat = [1, 2, 3].flatMap(n => [n, n * 2]); // [1, 2, 2, 4, 3, 6]
```
---
## 🪞 **7. Copying & Filling**

```js
let arr2 = [1, 2, 3, 4];

// copyWithin()
arr2.copyWithin(1, 2); // [1, 3, 4, 4]

// fill()
arr2.fill(0, 1, 3); // [1, 0, 0, 4]

// with() (ES2023)
let replaced = arr2.with(2, 99); // returns new array → [1, 0, 99, 4]

```
---
## 🧵 **8. Joining & String Conversion**

```js
let colors = ['red', 'green', 'blue'];

// join()
colors.join('-'); // "red-green-blue"

// toString()
colors.toString(); // "red,green,blue"

// toLocaleString()
colors.toLocaleString(); // "red,green,blue" (locale dependent)

```
---
## 🔑 **9. Iterators**

```js
let letters = ['a', 'b', 'c'];

// keys()
for (let key of letters.keys()) console.log(key); // 0, 1, 2

// values()
for (let value of letters.values()) console.log(value); // a, b, c

// entries()
for (let [i, v] of letters.entries()) console.log(i, v); // 0 a, 1 b, 2 c

// Symbol.iterator
for (let char of letters) console.log(char); // a, b, c

```
---
## 🧰 **10. Utility (Static)**

```js
Array.isArray([1, 2, 3]); // true
Array.from('123');        // ['1', '2', '3']
Array.of(4, 5, 6);        // [4, 5, 6]
```
---
## 💡 Bonus — Real Example Combo

```js
let data = [5, 10, 15, 20, 25];

let result = data
  .filter(n => n > 10)     // [15, 20, 25]
  .map(n => n / 5)         // [3, 4, 5]
  .reduce((sum, n) => sum + n, 0); // 12

console.log(result);
```