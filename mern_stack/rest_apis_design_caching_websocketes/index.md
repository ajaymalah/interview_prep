# REST API Interview Questions

## 76. What is REST?

### Answer

REST stands for Representational State Transfer.

It is an architectural style used for designing web services.

### Principles of REST

1. Client-Server Architecture
2. Stateless Communication
3. Cacheable Responses
4. Uniform Interface
5. Layered System

### Example

```http
GET /users
```

Returns all users.

### Interview Answer

"REST is an architectural style where resources are identified by URLs and manipulated using standard HTTP methods."

---

## 77. What is a RESTful API?

### Answer

A RESTful API follows REST principles.

### Example

```http
GET    /users
POST   /users
GET    /users/1
PUT    /users/1
DELETE /users/1
```

### Characteristics

- Uses HTTP methods correctly
- Resource-based URLs
- Stateless
- Consistent responses

---

## 78. What is Statelessness?

### Answer

Each request contains all information needed to process it.

Server does not store client state between requests.

### Example

Every request contains:

```http
Authorization: Bearer token
```

Server validates token every time.

### Benefit

- Easy scaling
- Better performance
- Simpler architecture

---

## 79. Explain HTTP Methods

### GET

Retrieve data.

```http
GET /users
```

---

### POST

Create data.

```http
POST /users
```

---

### PUT

Replace entire resource.

```http
PUT /users/1
```

---

### PATCH

Update specific fields.

```http
PATCH /users/1
```

---

### DELETE

Remove resource.

```http
DELETE /users/1
```

---

## 80. Difference Between PUT and PATCH

### PUT

Replaces entire resource.

Request:

```json
{
  "name":"John",
  "email":"john@gmail.com"
}
```

---

### PATCH

Updates only changed fields.

Request:

```json
{
  "name":"John"
}
```

### Interview Answer

"PUT replaces a complete resource while PATCH partially updates a resource."

---

## 81. Common HTTP Status Codes

### Success

```http
200 OK
201 Created
204 No Content
```

---

### Client Errors

```http
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
```

---

### Server Errors

```http
500 Internal Server Error
503 Service Unavailable
```

### Interview Tip

Know when to use:

```text
401 = Not Logged In

403 = Logged In But No Permission
```

---

## 82. How Do You Design Good APIs?

### Best Practices

### Use Nouns

Bad

```http
/getUsers
```

Good

```http
/users
```

---

### Version APIs

```http
/api/v1/users
```

---

### Consistent Naming

```http
/users
/products
/orders
```

---

### Return Meaningful Responses

```json
{
  "success": true,
  "message": "User created",
  "data": {}
}
```

---

## 83. What is API Versioning?

### Answer

Versioning prevents breaking existing clients.

### Example

```http
/api/v1/users
/api/v2/users
```

### Benefits

- Backward Compatibility
- Safer Updates
- Easier Maintenance

---

## 84. How Do You Handle Pagination?

### Problem

Returning 100,000 records is inefficient.

---

### Solution

```javascript
const page = 1;
const limit = 10;

User.find()
.skip((page-1)*limit)
.limit(limit);
```

---

### API Example

```http
GET /users?page=1&limit=10
```

### Response

```json
{
 "page":1,
 "limit":10,
 "total":500,
 "data":[]
}
```

---

## 85. Pagination Types

### Offset Pagination

```javascript
skip()
limit()
```

Easy but slower on large datasets.

---

### Cursor Pagination

```javascript
GET /users?cursor=abc123
```

Faster and scalable.

### Used By

- Facebook
- Instagram
- Twitter

---

## 86. How Do You Implement Filtering?

### Example

```http
GET /products?category=mobile
```

Backend:

```javascript
Product.find({
 category:req.query.category
});
```

---

### Multiple Filters

```http
GET /products?
category=mobile&
brand=apple
```

---

## 87. How Do You Implement Sorting?

### API

```http
GET /products?sort=price
```

Backend

```javascript
Product.find()
.sort({
 price:1
});
```

### Descending

```javascript
.sort({
 price:-1
});
```

---

## 88. How Do You Implement Search?

### MongoDB Text Search

```javascript
Product.find({
 $text:{
   $search:"iphone"
 }
});
```

---

### Regex Search

```javascript
Product.find({
 name:{
   $regex:"iphone",
   $options:"i"
 }
});
```

---

## 89. What is API Caching?

### Answer

Caching stores frequently accessed data temporarily.

### Benefits

- Faster Responses
- Reduced Database Load
- Better Scalability

### Example

```text
First Request
 ↓
Database

Second Request
 ↓
Cache
```

---

## 90. What is Redis?

### Answer

Redis is an in-memory key-value database.

### Uses

- Caching
- Sessions
- Rate Limiting
- Queues

### Example

```javascript
await redis.set(
 "users",
 JSON.stringify(data)
);
```

Retrieve

```javascript
await redis.get("users");
```

---

## 91. Redis Caching Example

### Check Cache

```javascript
const cache =
 await redis.get("users");
```

### Return Cached Data

```javascript
if(cache){
 return JSON.parse(cache);
}
```

### Database Query

```javascript
const users =
 await User.find();
```

### Save Cache

```javascript
await redis.set(
 "users",
 JSON.stringify(users),
 "EX",
 3600
);
```

---

## 92. What is Cache Invalidation?

### Answer

Removing stale data from cache.

### Example

User Updated

```javascript
await User.updateOne();
```

Remove Cache

```javascript
await redis.del("users");
```

### Why?

Prevent outdated responses.

---

# WebSocket Interview Questions

## 93. What is WebSocket?

### Answer

WebSocket provides full-duplex communication between client and server.

### HTTP

```text
Request
 ↓
Response
```

---

### WebSocket

```text
Client ↔ Server
```

Both can send messages anytime.

---

## 94. WebSocket vs HTTP

| HTTP | WebSocket |
|--------|----------|
| Request/Response | Persistent Connection |
| Stateless | Stateful |
| Higher Overhead | Low Overhead |
| Not Real-Time | Real-Time |

### Use Cases

- Chat Apps
- Notifications
- Live Tracking
- Multiplayer Games

---

## 95. What is Socket.IO?

### Answer

Socket.IO is a library for real-time communication.

### Server

```javascript
const io =
 require("socket.io")(server);

io.on(
 "connection",
 socket=>{
   console.log("Connected");
 });
```

### Client

```javascript
const socket =
 io("http://localhost:5000");
```

---

## 96. Send and Receive Messages

### Server

```javascript
socket.emit(
 "message",
 "Hello"
);
```

### Client

```javascript
socket.on(
 "message",
 data=>{
   console.log(data);
 });
```

---

## 97. Build Chat Application Flow

### Flow

```text
User A
 ↓
Socket Emit
 ↓
Server
 ↓
Socket Broadcast
 ↓
User B
```

### Event

```javascript
socket.emit(
 "sendMessage",
 message
);
```

---

## 98. What Are Rooms in Socket.IO?

### Answer

Rooms group connected users.

### Join Room

```javascript
socket.join("room1");
```

### Send Message

```javascript
io.to("room1")
.emit(
 "message",
 data
);
```

### Use Cases

- Group Chat
- Team Chat
- Notifications

---

## 99. How Do You Scale Socket.IO?

### Techniques

1. Redis Adapter
2. Load Balancer
3. Clustering
4. Microservices

### Example

```text
Server1
Server2
Server3
     ↓
 Redis Adapter
```

---

## 100. Explain REST API vs WebSocket

### REST API

Best For:

- CRUD Operations
- Authentication
- Standard APIs

Example

```http
GET /users
```

---

### WebSocket

Best For:

- Chat
- Notifications
- Live Data

Example

```javascript
socket.emit("message");
```

### Interview Answer

"I use REST APIs for CRUD operations and WebSockets for real-time features such as chat, notifications, and live tracking."