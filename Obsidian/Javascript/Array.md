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

# Iteration & Transformation Methods

- These methods let you go through each element of an array and perform operations like transforming data, filtering, or accumulating results.
## forEach()
- Executes a function once for each element in the array.
- Does not return a new array (it always returns undefined).
```js
let numbers = [1, 2, 3];
numbers.forEach(num => console.log(num * 2));
// Output: 2, 4, 6

```

🧠 Use when you want to perform an action (like logging or updating) but not create a new array.
## map()
- Creates a new array by applying a function to each element.
```js
let numbers = [1, 2, 3];
let doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6]
```
🧠 Use when you want to transform data (e.g., convert prices, capitalize names, etc.).
## filter()
- Creates a new array with elements that pass a condition.  
```js
let numbers = [5, 10, 15, 20];
let result = numbers.filter(num => num > 10);
console.log(result); // [15, 20]
```

🧠 Use for selecting specific elements based on a test (like filtering users or scores).
## Reduce()
- The reduce() method in JavaScript is an iterative method available on Array.prototype that executes a  "reducer" callback function on each element of the array, in ascending-index order, accumulating them into a single value. It effectively "reduces" the array to a single output value.
- Executes a function on each element to reduce the array to a single value.
```js
let numbers = [1, 2, 3, 4];
let sum = numbers.reduce((acc, curr , index , sum ) => acc + curr, 0);
console.log(sum); // 10
```
🧠 Use for totals, averages, or combining elements into one (e.g., sum, concatenation, etc.).
callback (required): A function to execute on each element in the array. It takes four arguments:
accumulator (required): The accumulated value returned from the previous invocation of the callback, or the initialValue if provided.
currentValue (required): The current element being processed in the array.
currentIndex (optional): The index of the current element being processed.
array (optional): The array reduce() was called upon.
## reduceRight()
- Same as reduce(), but starts from the rightmost element.
```js
let words = ['is', 'this', 'Hello'];
let sentence = words.reduceRight((acc, curr) => acc + ' ' + curr);
console.log(sentence); // "Hello this is"
```
- Use when the order of accumulation matters
## some()
- Returns true if any element satisfies a condition.
```js
let numbers = [10, 20, 30];
let hasLarge = numbers.some(num => num > 25);
console.log(hasLarge); // true
```
- Use when checking if at least one match exists (e.g., any user online?).
## every()
Returns true if all elements satisfy a condition.
```js
let numbers = [2, 4, 6];
let allEven = numbers.every(num => num % 2 === 0);
console.log(allEven); // true
```
- Use when you want to verify all elements meet a rule (like all scores > 50).
## flatMap()
Combines map() + flat(1) — maps each element and flattens one level.
```js
let arr = [1, 2, 3];
let result = arr.flatMap(x => [x, x * 2]);
console.log(result); // [1, 2, 2, 4, 3, 6]
```

- Use when mapping produces arrays and you want them flattened.


# Adding & Removing Elements in Arrays

## push()
Purpose: Adds one or more elements to the end of an array.
Returns: The new length of the array.
Example:
```js
let fruits = ["apple", "banana"];
fruits.push("mango");
console.log(fruits); // ["apple", "banana", "mango"]
```
## pop()
Purpose: Removes the last element from an array.
Returns: The removed element.
Example:
```js
let fruits = ["apple", "banana", "mango"];
let removed = fruits.pop();
console.log(fruits); // ["apple", "banana"]
console.log(removed); // "mango"
```
## unshift()
Purpose: Adds one or more elements to the beginning of an array.
Returns: The new length of the array.
Example:
```js
let fruits = ["banana", "mango"];
fruits.unshift("apple");
console.log(fruits); // ["apple", "banana", "mango"]
```
## shift()
Purpose: Removes the first element from an array.
Returns: The removed element.
Example:
```js
let fruits = ["apple", "banana", "mango"];
let first = fruits.shift();
console.log(fruits); // ["banana", "mango"]
console.log(first);  // "apple"
```
## splice()
Purpose: Adds, removes, or replaces elements at a specific index.
Syntax: array.splice(start, deleteCount, item1, item2, ...)
Returns: An array of removed elements.
Examples:
```js
let fruits = ["apple", "banana", "mango", "grape"];
// Remove elements
fruits.splice(1, 1);
console.log(fruits); // ["apple", "mango", "grape"]
// Add elements
fruits.splice(2, 0, "kiwi", "pear");
console.log(fruits); // ["apple", "mango", "kiwi", "pear", "grape"]
// Replace elements
fruits.splice(1, 2, "cherry");
console.log(fruits); // ["apple", "cherry", "pear", "grape"]
```
## concat()
Purpose: Combines two or more arrays into one.
Does not mutate the original array.
Returns: A new array.
Example:
```js
let fruits = ["apple", "banana"];
let veggies = ["carrot", "potato"];
let mix = fruits.concat(veggies);
console.log(mix); // ["apple", "banana", "carrot", "potato"]
```
## slice()
Purpose: Returns a shallow copy of a portion of an array into a new array.
Does not modify the original array.
Syntax: array.slice(start, end)
Example:
```js
let fruits = ["apple", "banana", "mango", "grape"];
let sliced = fruits.slice(1, 3);
console.log(sliced); // ["banana", "mango"]
console.log(fruits); // ["apple", "banana", "mango", "grape"]
```



## 🔍 **Searching & Sorting Methods in Arrays**

---

### 1. **indexOf()**
- **Purpose:** Finds the **first index** of a given element in an array.
- **Returns:** The index if found, or `-1` if not found.
**Example:**

`let fruits = ["apple", "banana", "mango", "banana"]; console.log(fruits.indexOf("banana")); // 1 console.log(fruits.indexOf("cherry")); // -1`

---

### 2. **lastIndexOf()**
- **Purpose:** Finds the **last index** of a given element.
- **Returns:** The last occurrence index or `-1`.
**Example:**
`let fruits = ["apple", "banana", "mango", "banana"]; console.log(fruits.lastIndexOf("banana")); // 3`

---
### 3. **includes()**
- **Purpose:** Checks if an array **contains** a specific element.
- **Returns:** `true` or `false`. 
**Example:**
`let fruits = ["apple", "banana", "mango"]; console.log(fruits.includes("banana")); // true console.log(fruits.includes("grape"));  // false`

---
### 4. **find()**
- **Purpose:** Returns the **first element** that satisfies a given condition (callback).
- **Returns:** The element if found, otherwise `undefined`.
**Example:**
`let numbers = [5, 10, 15, 20]; let found = numbers.find(num => num > 10); console.log(found); // 15`

---
### 5. **findIndex()**
- **Purpose:** Returns the **index** of the first element that satisfies the condition.
- **Returns:** Index or `-1` if not found.
**Example:**
`let numbers = [5, 10, 15, 20]; let index = numbers.findIndex(num => num > 10); console.log(index); // 2`

---
### 6. **filter()**
- **Purpose:** Returns a **new array** containing elements that meet the condition.
- **Does not modify** the original array.
**Example:**
`let numbers = [5, 10, 15, 20]; let filtered = numbers.filter(num => num > 10); console.log(filtered); // [15, 20]`

---
### 7. **sort()**
- **Purpose:** Sorts array elements **in place** (modifies original array).
- **Default behavior:** Converts elements to strings and sorts lexicographically.
- **Use compare function** for numerical sorting.
**Examples:**
`let fruits = ["banana", "apple", "cherry"]; fruits.sort(); console.log(fruits); // ["apple", "banana", "cherry"]  let numbers = [40, 5, 25, 100]; numbers.sort((a, b) => a - b); // Ascending console.log(numbers); // [5, 25, 40, 100]`

---
### 8. **reverse()**
- **Purpose:** Reverses the order of elements **in place**.
- **Modifies** the original array.
**Example:**
`let fruits = ["apple", "banana", "cherry"]; fruits.reverse(); console.log(fruits); // ["cherry", "banana", "apple"]`

---
### 9. **some()**
- **Purpose:** Checks if **at least one** element satisfies the condition.
- **Returns:** `true` or `false`.
**Example:**
`let numbers = [5, 10, 15, 20]; console.log(numbers.some(num => num > 18)); // true`

---
### 10. **every()**
- **Purpose:** Checks if **all** elements satisfy the condition.
- **Returns:** `true` or `false`.
**Example:**
`let numbers = [5, 10, 15, 20]; console.log(numbers.every(num => num > 0));  // true console.log(numbers.every(num => num > 10)); // false`