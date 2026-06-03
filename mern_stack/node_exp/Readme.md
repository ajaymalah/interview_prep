# Node.js Interview Questions

## 11. What is Node.js?

### Answer

Node.js is an open-source JavaScript runtime built on Google's V8 engine.

It allows JavaScript to run outside the browser.

### Features

- Event-driven
- Non-blocking I/O
- Single-threaded
- Highly scalable
- Fast execution using V8 engine

### Architecture

```text
Client Request
      ↓
 Event Loop
      ↓
 Worker Threads
      ↓
 Database/File System
      ↓
 Response
```

### Use Cases

- REST APIs
- Real-time chat applications
- Streaming services
- Microservices

### Interview Answer

"Node.js uses an event-driven, non-blocking architecture that makes it suitable for handling many concurrent requests efficiently."

---

## 12. Explain the Event Loop

### Answer

Node.js uses a single thread to execute JavaScript.

The Event Loop enables asynchronous operations without blocking the main thread.

### Phases

1. Timers
2. Pending Callbacks
3. Poll
4. Check
5. Close Callbacks

### Example

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

Output:

```text
Start
End
Promise
Timeout
```

### Why?

Promise callbacks (Microtasks) execute before Timer Queue tasks.

---

## 13. What is Non-Blocking I/O?

### Answer

Node.js does not wait for long-running operations.

### Blocking Example

```javascript
const data = fs.readFileSync("file.txt");
console.log(data);
```

Thread waits.

---

### Non-Blocking Example

```javascript
fs.readFile("file.txt", (err, data) => {
  console.log(data);
});

console.log("Next task");
```

Output:

```text
Next task
file content
```

### Benefit

Improves scalability and responsiveness.

---

## 14. What are Streams?

### Answer

Streams process data in chunks instead of loading entire data into memory.

### Types of Streams

1. Readable
2. Writable
3. Duplex
4. Transform

### Example

```javascript
const fs = require("fs");

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);
```

### Benefits

- Low memory usage
- Faster processing
- Suitable for large files

---

## 15. What is Middleware?

### Answer

Middleware functions execute during request-response lifecycle.

### Example

```javascript
app.use((req, res, next) => {
  console.log("Request received");
  next();
});
```

### Common Uses

- Logging
- Authentication
- Validation
- Error handling

---

## 16. Difference Between process.nextTick(), setImmediate(), and setTimeout()

### process.nextTick()

Runs immediately after current operation.

```javascript
process.nextTick(() => {
  console.log("nextTick");
});
```

---

### setImmediate()

Runs during Check phase.

```javascript
setImmediate(() => {
  console.log("Immediate");
});
```

---

### setTimeout()

Runs after specified delay.

```javascript
setTimeout(() => {
  console.log("Timeout");
}, 0);
```

### Execution Order

```text
process.nextTick()
Promise
setTimeout()
setImmediate()
```

(Usually)

---

## 17. What is Clustering?

### Answer

Node.js normally uses one CPU core.

Cluster module allows using multiple CPU cores.

### Example

```javascript
const cluster = require("cluster");

if (cluster.isMaster) {
  cluster.fork();
  cluster.fork();
} else {
  app.listen(3000);
}
```

### Benefits

- Better CPU utilization
- Increased throughput
- Improved availability

---

## 18. How Do You Handle Errors in Node.js?

### Try-Catch

```javascript
try {
  throw new Error("Something wrong");
} catch(error) {
  console.log(error.message);
}
```

---

### Async Error Handling

```javascript
try {
  const user = await User.findById(id);
} catch(err) {
  console.log(err);
}
```

---

### Express Error Middleware

```javascript
app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message
  });
});
```

---

## 19. What is the Difference Between CommonJS and ES Modules?

### CommonJS

```javascript
const express = require("express");

module.exports = app;
```

---

### ES Modules

```javascript
import express from "express";

export default app;
```

### Comparison

| CommonJS | ES Modules |
|-----------|-----------|
| require | import |
| module.exports | export |
| Synchronous | Static Analysis |

---

## 20. How Do You Improve Node.js API Performance?

### Techniques

1. Caching
2. Database Indexing
3. Compression
4. Pagination
5. Load Balancing
6. Clustering
7. Redis Caching

### Example Redis

```javascript
const cache = await redis.get(key);

if(cache){
   return JSON.parse(cache);
}
```

### Interview Answer

"I use Redis caching, MongoDB indexes, pagination, compression, and clustering to improve API performance."

---

# Express.js Interview Questions

## 21. What is Express.js?

### Answer

Express.js is a lightweight web framework for Node.js.

### Benefits

- Routing
- Middleware support
- API creation
- Error handling

### Example

```javascript
const express = require("express");

const app = express();

app.get("/", (req,res)=>{
   res.send("Hello");
});
```

---

## 22. Explain Express Request Lifecycle

### Flow

```text
Client Request
      ↓
Middleware 1
      ↓
Middleware 2
      ↓
Route Handler
      ↓
Response
```

### Example

```javascript
app.use(authMiddleware);

app.get("/users", getUsers);
```

---

## 23. How Does Routing Work?

### Example

```javascript
app.get("/users", getUsers);

app.post("/users", createUser);

app.put("/users/:id", updateUser);

app.delete("/users/:id", deleteUser);
```

### REST Convention

| Method | Purpose |
|----------|---------|
| GET | Read |
| POST | Create |
| PUT | Update |
| DELETE | Delete |

---

## 24. What is Global Error Handling?

### Example

```javascript
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
      success:false,
      message:err.message
  });
});
```

### Benefits

- Centralized error management
- Consistent responses
- Cleaner code

---

## 25. How Do You Validate Requests?

### Using express-validator

```javascript
const { body } = require("express-validator");

app.post(
  "/register",
  body("email").isEmail(),
  body("password").isLength({ min: 6 }),
  register
);
```

### Why?

- Prevent invalid data
- Improve security
- Reduce bugs

---

## 26. How Do You Secure Express APIs?

### Security Measures

#### Helmet

```javascript
const helmet = require("helmet");

app.use(helmet());
```

#### CORS

```javascript
app.use(cors());
```

#### Rate Limiting

```javascript
const rateLimit = require("express-rate-limit");

app.use(rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
}));
```

#### JWT Authentication

```javascript
jwt.verify(token, secret);
```

---

## 27. What is CORS?

### Answer

Cross-Origin Resource Sharing controls whether resources can be accessed from another domain.

### Example

Frontend:

```text
http://localhost:3000
```

Backend:

```text
http://localhost:5000
```

Browser blocks request unless CORS allows it.

### Solution

```javascript
app.use(cors({
  origin:"http://localhost:3000"
}));
```

---

## 28. What is Rate Limiting?

### Answer

Restricts number of requests from a client.

### Example

```javascript
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
});
```

### Benefits

- Prevent abuse
- Prevent brute-force attacks
- Reduce server load

---

## 29. How Do You Implement JWT Authentication?

### Login

```javascript
const token = jwt.sign(
  { userId:user._id },
  process.env.JWT_SECRET,
  { expiresIn:"1h" }
);
```

---

### Verify

```javascript
const decoded = jwt.verify(
   token,
   process.env.JWT_SECRET
);
```

### Flow

```text
Login
 ↓
Generate JWT
 ↓
Send Token
 ↓
Store Token
 ↓
Protected Routes
 ↓
Verify Token
```

---

## 30. What Folder Structure Do You Follow?

### Example

```text
project/
│
├── controllers/
├── routes/
├── models/
├── middleware/
├── services/
├── utils/
├── config/
├── validations/
├── app.js
└── server.js
```

### Why?

- Scalability
- Separation of concerns
- Easier maintenance

### Interview Answer

"I follow a layered architecture separating routes, controllers, services, and database logic for maintainability and scalability."