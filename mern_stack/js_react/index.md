# MERN Stack Interview Guide (3 Years Experience)

# JavaScript Questions

## 1. What is the difference between var, let, and const?

### Answer

JavaScript provides three ways to declare variables:

| Feature | var | let | const |
|----------|------|------|------|
| Scope | Function | Block | Block |
| Redeclaration | Allowed | Not Allowed | Not Allowed |
| Reassignment | Allowed | Allowed | Not Allowed |
| Hoisted | Yes | Yes (TDZ) | Yes (TDZ) |

### Example

```javascript
var a = 10;
var a = 20; // Allowed

let b = 10;
// let b = 20; // Error

const c = 10;
// c = 20; // Error
```

### Interview Point

Prefer `const` by default.
Use `let` when value changes.
Avoid `var` in modern applications.

---

## 2. Explain Closures

### Answer

A closure is created when an inner function remembers variables from its outer function even after the outer function has finished execution.

### Example

```javascript
function outer() {
    let count = 0;

    return function inner() {
        count++;
        return count;
    }
}

const counter = outer();

console.log(counter()); // 1
console.log(counter()); // 2
```

### Use Cases

- Data hiding
- Private variables
- Memoization
- Event handlers

### Interview Definition

"A closure gives a function access to variables from its lexical scope even after the outer function has executed."

---

## 3. Explain Event Loop

### Answer

JavaScript is single-threaded.

The Event Loop allows asynchronous operations without blocking execution.

### Components

1. Call Stack
2. Web APIs
3. Callback Queue
4. Event Loop

### Example

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 0);

console.log("End");
```

Output:

```text
Start
End
Timer
```

### Explanation

The timer callback enters the callback queue.
Event Loop pushes it to stack only after stack becomes empty.

---

## 4. Difference Between Promise and Async/Await

### Promise

```javascript
fetchData()
    .then(data => console.log(data))
    .catch(err => console.log(err));
```

### Async Await

```javascript
async function getData() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch(err) {
        console.log(err);
    }
}
```

### Difference

| Promise | Async/Await |
|----------|------------|
| Chain based | Synchronous-looking |
| Harder to read | Cleaner |
| Nested chaining possible | Easier debugging |

### Interview Point

Async/Await internally uses Promises.

---

# React Questions

## 5. What is Virtual DOM?

### Answer

Virtual DOM is a lightweight copy of the real DOM.

React updates the Virtual DOM first and compares it with previous version using Diffing Algorithm.

Only changed nodes are updated in Real DOM.

### Benefits

- Faster rendering
- Better performance
- Less DOM manipulation

### Flow

```text
State Change
      ↓
Virtual DOM Created
      ↓
Diffing
      ↓
Update Only Changed Elements
      ↓
Real DOM Updated
```

---

## 6. Explain React Hooks

### Answer

Hooks allow functional components to use React features.

### Common Hooks

#### useState

```javascript
const [count, setCount] = useState(0);
```

Used for state management.

#### useEffect

```javascript
useEffect(() => {
    fetchData();
}, []);
```

Used for side effects.

#### useContext

```javascript
const user = useContext(UserContext);
```

Used for global state sharing.

#### useMemo

```javascript
const result = useMemo(() => {
    return expensiveCalculation(data);
}, [data]);
```

Optimizes expensive calculations.

#### useCallback

```javascript
const handleClick = useCallback(() => {
    console.log("clicked");
}, []);
```

Prevents unnecessary function recreation.

---

## 7. Difference Between useEffect, useMemo and useCallback

### useEffect

Performs side effects.

```javascript
useEffect(() => {
    fetchData();
}, []);
```

Examples:

- API calls
- Event listeners
- Timers

---

### useMemo

Caches value.

```javascript
const total = useMemo(() => {
    return items.reduce((a,b) => a+b.price, 0);
}, [items]);
```

---

### useCallback

Caches function.

```javascript
const handleSubmit = useCallback(() => {
    submitForm();
}, []);
```

---

### Summary

| Hook | Purpose |
|--------|---------|
| useEffect | Side effects |
| useMemo | Memoize value |
| useCallback | Memoize function |

---

## 8. Controlled vs Uncontrolled Components

### Controlled

React controls form state.

```javascript
const [name,setName] = useState("");

<input
 value={name}
 onChange={(e)=>setName(e.target.value)}
/>
```

### Benefits

- Validation
- Predictable state
- Easier debugging

---

### Uncontrolled

DOM controls state.

```javascript
const ref = useRef();

<input ref={ref}/>
```

### Benefits

- Less code
- Better performance for simple forms

---

## 9. Context API vs Redux

### Context API

Suitable for:

- Theme
- Authentication
- Language

```javascript
<UserContext.Provider value={user}>
```

### Redux

Suitable for:

- Large applications
- Complex state management
- Predictable updates

### Redux Advantages

- Middleware support
- DevTools
- Better scalability

### Interview Answer

Use Context for small-to-medium global state.
Use Redux Toolkit for complex enterprise applications.

---

## 10. How Do You Optimize React Performance?

### Techniques

### 1. React.memo

```javascript
export default React.memo(Component);
```

Prevents unnecessary re-renders.

---

### 2. useMemo

```javascript
const total = useMemo(() => calculate(), []);
```

Caches expensive values.

---

### 3. useCallback

```javascript
const handleClick = useCallback(() => {}, []);
```

Caches functions.

---

### 4. Lazy Loading

```javascript
const Dashboard = React.lazy(() =>
    import('./Dashboard')
);
```

---

### 5. Code Splitting

Loads bundles only when needed.

---

### 6. Pagination

Avoid rendering huge datasets.

---

### 7. Debouncing Search

```javascript
lodash.debounce()
```

Reduces API calls.

### Interview Point

Always measure performance using React DevTools Profiler before optimizing.