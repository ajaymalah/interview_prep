
# JavaScript Coding Interview Questions

## 126. Reverse a String

### Problem

Reverse a string without using built-in reverse().

### Solution

```javascript
function reverseString(str) {

 let reversed = "";

 for(let i = str.length - 1; i >= 0; i--) {
   reversed += str[i];
 }

 return reversed;
}

console.log(
 reverseString("hello")
);
```

### Output

```text
olleh
```

### Time Complexity

```text
O(n)
```

---

## 127. Find Duplicate Elements in Array

### Solution

```javascript
function findDuplicates(arr){

 const seen = new Set();
 const duplicates = new Set();

 for(const item of arr){

  if(seen.has(item)){
    duplicates.add(item);
  }

  seen.add(item);
 }

 return [...duplicates];
}

console.log(
 findDuplicates(
  [1,2,3,2,4,5,1]
 )
);
```

### Output

```javascript
[2,1]
```

---

## 128. Find First Non-Repeating Character

### Solution

```javascript
function firstUnique(str){

 const map = {};

 for(const ch of str){
  map[ch] =
   (map[ch] || 0) + 1;
 }

 for(const ch of str){

  if(map[ch] === 1){
    return ch;
  }

 }

 return null;
}

console.log(
 firstUnique("aabbcdde")
);
```

### Output

```text
c
```

---

## 129. Check Palindrome

### Solution

```javascript
function isPalindrome(str){

 return str ===
 str.split("")
 .reverse()
 .join("");
}

console.log(
 isPalindrome("madam")
);
```

### Output

```text
true
```

---

## 130. Flatten Nested Array

### Input

```javascript
[1,[2,[3,4]],5]
```

### Solution

```javascript
function flatten(arr){

 let result = [];

 for(let item of arr){

  if(Array.isArray(item)){
   result.push(
    ...flatten(item)
   );
  }else{
   result.push(item);
  }
 }

 return result;
}
```

### Output

```javascript
[1,2,3,4,5]
```

---

## 131. Deep Clone an Object

### Basic Solution

```javascript
const clone =
 JSON.parse(
  JSON.stringify(obj)
 );
```

### Limitation

Doesn't support:

```text
Date
Map
Set
Functions
```

---

### Modern Solution

```javascript
const cloned =
 structuredClone(obj);
```

---

## 132. Debounce Implementation

### Definition

Execute function only after user stops triggering events.

### Example

Search Input.

### Implementation

```javascript
function debounce(
 fn,
 delay
){

 let timer;

 return function(...args){

  clearTimeout(timer);

  timer =
   setTimeout(()=>{
    fn(...args);
   },delay);
 };
}
```

### Use Cases

- Search
- Resize
- Scroll

---

## 133. Throttle Implementation

### Definition

Execute function once every specified interval.

### Implementation

```javascript
function throttle(
 fn,
 limit
){

 let waiting = false;

 return function(...args){

  if(!waiting){

   fn(...args);

   waiting = true;

   setTimeout(()=>{
    waiting = false;
   },limit);

  }

 };
}
```

### Use Cases

- Scroll Events
- Mouse Movement
- Resize Events

---

## 134. Difference Between Debounce and Throttle

### Debounce

```text
Waits until user stops
```

Example:

```text
Search Box
```

---

### Throttle

```text
Runs every interval
```

Example:

```text
Scroll Tracking
```

---

## 135. Implement Promise.all()

### Polyfill

```javascript
function promiseAll(
 promises
){

 return new Promise(
  (resolve,reject)=>{

   const results = [];
   let completed = 0;

   promises.forEach(
    (promise,index)=>{

     Promise.resolve(
      promise
     )
     .then(result=>{

      results[index] =
       result;

      completed++;

      if(
       completed ===
       promises.length
      ){
       resolve(results);
      }

     })
     .catch(reject);

    }
   );

  }
 );
}
```

---

## 136. Implement Array.map()

### Polyfill

```javascript
Array.prototype.myMap =
 function(callback){

  const result = [];

  for(let i=0;
      i<this.length;
      i++){

   result.push(
    callback(
      this[i],
      i,
      this
    )
   );
  }

  return result;
 };
```

### Usage

```javascript
[1,2,3].myMap(
 x => x*2
);
```

Output:

```javascript
[2,4,6]
```

---

## 137. Implement Array.filter()

### Polyfill

```javascript
Array.prototype.myFilter =
 function(callback){

  const result = [];

  for(let i=0;
      i<this.length;
      i++){

   if(
    callback(
      this[i],
      i,
      this
    )
   ){
    result.push(this[i]);
   }

  }

  return result;
 };
```

---

## 138. Implement Array.reduce()

### Polyfill

```javascript
Array.prototype.myReduce =
 function(callback,
          initial){

  let accumulator =
   initial;

  for(let i=0;
      i<this.length;
      i++){

   accumulator =
    callback(
      accumulator,
      this[i]
    );

  }

  return accumulator;
 };
```

---

## 139. What is Currying?

### Answer

Transforming a function with multiple arguments into multiple functions with one argument.

### Example

```javascript
function add(a){

 return function(b){
  return a + b;
 };

}

console.log(
 add(5)(10)
);
```

### Output

```text
15
```

---

## 140. What is Memoization?

### Answer

Caching expensive function results.

### Example

```javascript
function memoize(fn){

 const cache = {};

 return function(n){

  if(cache[n]){
   return cache[n];
  }

  const result =
   fn(n);

  cache[n] =
   result;

  return result;
 };
}
```

### Benefit

Avoid repeated calculations.

---

# DSA Questions

## 141. Find Maximum Element

### Solution

```javascript
function max(arr){

 return Math.max(...arr);
}
```

### Complexity

```text
O(n)
```

---

## 142. Find Missing Number

### Input

```javascript
[1,2,3,5]
```

### Output

```javascript
4
```

### Solution

```javascript
function missing(arr){

 const n =
  arr.length + 1;

 const expected =
  n*(n+1)/2;

 const actual =
  arr.reduce(
   (a,b)=>a+b,
   0
  );

 return expected -
        actual;
}
```

---

## 143. Remove Duplicates

### Solution

```javascript
function removeDuplicates(arr){

 return [...new Set(arr)];
}
```

### Output

```javascript
[1,2,3]
```

---

## 144. Count Character Frequency

### Solution

```javascript
function frequency(str){

 const map = {};

 for(const ch of str){
  map[ch] =
   (map[ch] || 0) + 1;
 }

 return map;
}
```

---

## 145. Sort Array Without sort()

### Bubble Sort

```javascript
function bubbleSort(arr){

 for(let i=0;
     i<arr.length;
     i++){

  for(let j=0;
      j<arr.length-i-1;
      j++){

   if(arr[j] >
      arr[j+1]){

    [
      arr[j],
      arr[j+1]
    ] =
    [
      arr[j+1],
      arr[j]
    ];
   }
  }
 }

 return arr;
}
```

---

# Machine Coding Questions

## 146. Build Search with Debouncing

### Requirements

```text
Input Field
↓
User Types
↓
Wait 500ms
↓
Call API
```

### Interview Focus

- Debounce
- API Integration
- Loading State

---

## 147. Build Todo Application

### Features

```text
Add Todo
Delete Todo
Edit Todo
Mark Complete
```

### Concepts Tested

- useState
- Component Design
- CRUD Operations

---

## 148. Build Infinite Scroll

### Flow

```text
User Scrolls
↓
Bottom Reached
↓
Fetch Next Page
↓
Append Data
```

### Concepts

- Pagination
- Intersection Observer
- Performance

---

## 149. Build Custom useFetch Hook

### Example

```javascript
function useFetch(url){

 const [data,setData] =
 useState(null);

 useEffect(()=>{

  fetch(url)
  .then(res=>res.json())
  .then(setData);

 },[url]);

 return data;
}
```

---

## 150. LRU Cache Design

### Definition

Least Recently Used Cache removes least-used item first.

### Example

Capacity:

```text
3
```

Operations:

```text
Put A
Put B
Put C
Access A
Put D
```

Removed:

```text
B
```

Because B is least recently used.

### Interview Answer

Use:

```javascript
Map
```

because insertion order is maintained.

### Complexity

```text
get() => O(1)

put() => O(1)
```
