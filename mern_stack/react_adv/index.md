# Advanced React Interview Questions

## 101. What is React Reconciliation?

### Answer

Reconciliation is React's process of comparing the old Virtual DOM with the new Virtual DOM and updating only the changed elements.

### Flow

```text
State Change
      ↓
New Virtual DOM
      ↓
Compare with Old Virtual DOM
      ↓
Find Differences
      ↓
Update Real DOM
```

### Example

Before:

```jsx
<h1>Hello</h1>
```

After:

```jsx
<h1>Hello John</h1>
```

React updates only the text node instead of re-rendering the whole page.

### Interview Answer

"Reconciliation is React's diffing process that efficiently updates the DOM by changing only what has actually changed."

---

## 102. What is React.memo?

### Answer

React.memo prevents unnecessary re-renders of functional components.

### Example

```jsx
const UserCard = React.memo(({ user }) => {
  console.log("Rendered");
  return <div>{user.name}</div>;
});
```

Without memo:

```text
Parent Re-render
 ↓
Child Re-render
```

With memo:

```text
Parent Re-render
 ↓
Child Skipped (if props unchanged)
```

### Use Case

Expensive components.

---

## 103. What is useMemo?

### Answer

useMemo memoizes computed values.

### Example

```jsx
const total = useMemo(() => {
  return products.reduce(
    (sum, item) => sum + item.price,
    0
  );
}, [products]);
```

### Benefit

Avoids recalculating expensive operations.

### Use When

- Large datasets
- Expensive calculations

---

## 104. What is useCallback?

### Answer

useCallback memoizes functions.

### Example

```jsx
const handleClick = useCallback(() => {
  console.log("clicked");
}, []);
```

### Problem

Without useCallback:

```jsx
const handleClick = () => {};
```

Function recreated on every render.

### Benefit

Prevents unnecessary child re-renders.

---

## 105. Difference Between useMemo and useCallback

### useMemo

Caches value.

```jsx
const total = useMemo(() => {
 return calculate();
}, []);
```

---

### useCallback

Caches function.

```jsx
const fn = useCallback(() => {
 console.log("Hello");
}, []);
```

### Summary

| Hook | Returns |
|--------|---------|
| useMemo | Value |
| useCallback | Function |

---

## 106. What Are Error Boundaries?

### Answer

Error Boundaries catch JavaScript errors in child components.

### Example

```jsx
class ErrorBoundary
extends React.Component {

 state = {
  hasError:false
 };

 static getDerivedStateFromError() {
   return {
     hasError:true
   };
 }

 render() {

  if(this.state.hasError){
    return <h1>Error</h1>;
  }

  return this.props.children;
 }
}
```

### Benefit

Prevents application crash.

---

## 107. What is Lazy Loading?

### Answer

Loads components only when needed.

### Example

```jsx
const Dashboard =
 React.lazy(() =>
  import("./Dashboard")
 );
```

### Usage

```jsx
<Suspense
 fallback={<Loader/>}
>
 <Dashboard/>
</Suspense>
```

### Benefits

- Smaller bundle size
- Faster initial load

---

## 108. What is Code Splitting?

### Answer

Breaking large JavaScript bundles into smaller chunks.

### Example

```jsx
React.lazy()
```

Webpack automatically creates separate chunks.

### Benefit

Improves page load performance.

---

## 109. What Are Custom Hooks?

### Answer

Custom hooks allow reusable stateful logic.

### Example

```jsx
function useCounter(){

 const [count,setCount]
 = useState(0);

 const increment =
 () => setCount(
   prev=>prev+1
 );

 return {
   count,
   increment
 };
}
```

Usage:

```jsx
const {
 count,
 increment
} = useCounter();
```

---

## 110. Explain React Rendering Process

### Initial Render

```text
Component Render
 ↓
Virtual DOM
 ↓
Real DOM
```

---

### Re-render

Triggered by:

```jsx
setState()
```

```jsx
props change
```

```jsx
context change
```

### Optimization

- React.memo
- useMemo
- useCallback

---

# Redux Toolkit Questions

## 111. What is Redux?

### Answer

Redux is a predictable state management library.

### Flow

```text
Component
   ↓
Dispatch Action
   ↓
Reducer
   ↓
Store Updated
   ↓
UI Updated
```

---

## 112. Why Redux Toolkit?

### Problems with Redux

- Too much boilerplate
- Complex setup

### Solution

Redux Toolkit.

### Benefits

- Simpler
- Built-in Immer
- Better DevTools Support

---

## 113. Create Redux Slice

### Example

```jsx
import {
 createSlice
}
from "@reduxjs/toolkit";

const counterSlice =
 createSlice({

  name:"counter",

  initialState:{
   value:0
  },

  reducers:{
   increment:(state)=>{
     state.value++;
   }
  }
 });

export const {
 increment
}
=
counterSlice.actions;

export default
counterSlice.reducer;
```

---

## 114. What is createAsyncThunk?

### Answer

Handles asynchronous actions.

### Example

```jsx
export const fetchUsers =
 createAsyncThunk(
 "users/fetch",
 async ()=>{

  const response =
  await axios.get(
   "/users"
  );

  return response.data;
 }
);
```

### Benefits

- Loading State
- Success State
- Error State

---

## 115. Explain Redux Store

### Example

```jsx
import {
 configureStore
}
from "@reduxjs/toolkit";

const store =
 configureStore({
  reducer:{
   user:userReducer
  }
 });
```

### Purpose

Stores global application state.

---

# React Query / TanStack Query

## 116. What is React Query?

### Answer

React Query manages server state.

### Example

```jsx
const {
 data,
 isLoading
}
=
useQuery({
 queryKey:["users"],
 queryFn:getUsers
});
```

### Benefits

- Caching
- Auto Refetching
- Pagination
- Optimistic Updates

---

## 117. Redux vs React Query

### Redux

Client State

Examples:

```text
Theme
Sidebar State
Cart
```

---

### React Query

Server State

Examples:

```text
API Data
Users
Products
Orders
```

### Interview Answer

"I use Redux Toolkit for global client state and React Query for server-side API state."

---

## 118. What is Optimistic Update?

### Answer

UI updates before server response.

### Example

```text
Like Post
 ↓
Immediately Update UI
 ↓
Call API
 ↓
Rollback if Failed
```

### Benefit

Better user experience.

---

## 119. What is Query Invalidation?

### Answer

Forces React Query to refetch data.

### Example

```jsx
queryClient.invalidateQueries({
 queryKey:["users"]
});
```

### Use Case

After create/update/delete operations.

---

## 120. How Do You Handle API Errors?

### Example

```jsx
try{

 const response =
 await axios.get("/users");

}catch(error){

 toast.error(
  error.message
 );
}
```

### Global Error Handling

```jsx
Axios Interceptors
```

---

# Frontend System Design

## 121. Design a Large Dashboard

### Structure

```text
Dashboard
 ├─ Header
 ├─ Sidebar
 ├─ Charts
 ├─ Tables
 └─ Filters
```

### Optimization

- Lazy Loading
- React Query
- Memoization
- Pagination

---

## 122. How Would You Design an E-Commerce Frontend?

### Modules

```text
Authentication
Products
Cart
Orders
Payments
Profile
```

### State Management

Redux Toolkit:

```text
Cart
Auth
Wishlist
```

### API State

React Query:

```text
Products
Orders
```

---

## 123. How Do You Handle Large Lists?

### Problem

10,000 DOM Elements.

### Solution

Virtualization.

### Example

```jsx
react-window
```

```jsx
react-virtualized
```

### Benefit

Render only visible items.

---

## 124. How Do You Optimize React Application Performance?

### Techniques

1. React.memo
2. useMemo
3. useCallback
4. Lazy Loading
5. Code Splitting
6. Virtualization
7. Debouncing
8. Throttling
9. Image Optimization
10. CDN

### Interview Answer

"I first profile the application using React DevTools and then optimize bottlenecks using memoization, code splitting, virtualization, and caching."

---

## 125. Explain a Production-Level React Architecture

### Folder Structure

```text
src/
│
├── api/
├── components/
├── features/
├── hooks/
├── layouts/
├── pages/
├── routes/
├── services/
├── store/
├── utils/
└── App.jsx
```

### Features Folder

```text
features/
 ├─ auth/
 ├─ users/
 ├─ orders/
 └─ products/
```

### Benefits

- Scalable
- Maintainable
- Easy Testing
- Modular Design

### Interview Answer

"I follow a feature-based architecture with Redux Toolkit for client state, React Query for server state, reusable hooks, and lazy-loaded routes for scalability."