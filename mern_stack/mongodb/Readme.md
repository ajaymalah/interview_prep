# MongoDB Interview Questions

## 31. What is MongoDB?

### Answer

MongoDB is a NoSQL document-oriented database.

Instead of storing data in tables and rows like SQL databases,
MongoDB stores data as JSON-like documents (BSON).

### Example Document

```json
{
  "_id": "123",
  "name": "John",
  "email": "john@gmail.com",
  "age": 25
}
```

### Features

- Schema Flexible
- High Performance
- Horizontal Scaling
- Replication Support
- Aggregation Framework

### Interview Answer

"MongoDB is a document-based NoSQL database that stores data in BSON format and provides flexibility, scalability, and high performance."

---

## 32. Difference Between SQL and MongoDB

| SQL | MongoDB |
|-------|---------|
| Tables | Collections |
| Rows | Documents |
| Fixed Schema | Dynamic Schema |
| Vertical Scaling | Horizontal Scaling |
| Joins | Embedded Documents / Lookup |

### SQL Example

```sql
SELECT * FROM users;
```

### MongoDB Example

```javascript
db.users.find({});
```

### When to Choose MongoDB?

- Rapid development
- Flexible data structure
- Large-scale applications

---

## 33. What is BSON?

### Answer

BSON stands for Binary JSON.

MongoDB stores documents internally as BSON.

### Advantages

- Faster parsing
- Supports additional data types
- More efficient storage

### Supported Types

```javascript
String
Number
Boolean
Date
Array
Object
ObjectId
```

Example:

```javascript
{
  _id: ObjectId("64f7ab")
}
```

---

## 34. What is ObjectId?

### Answer

MongoDB automatically generates ObjectId for documents.

### Example

```javascript
{
  "_id": ObjectId("507f191e810c19729de860ea")
}
```

### Contains

```text
Timestamp
Machine ID
Process ID
Counter
```

### Benefits

- Unique globally
- Efficient indexing

---

## 35. What is Indexing?

### Answer

Indexes improve query performance.

Without index:

```text
Collection Scan
O(n)
```

With index:

```text
Index Lookup
O(log n)
```

### Create Index

```javascript
db.users.createIndex({
  email: 1
});
```

### Example

```javascript
User.find({
  email: "john@gmail.com"
});
```

### Interview Point

Indexes speed up reads but increase storage and write costs.

---

## 36. Types of Indexes

### Single Field Index

```javascript
db.users.createIndex({
  email:1
});
```

---

### Compound Index

```javascript
db.users.createIndex({
  name:1,
  age:-1
});
```

---

### Text Index

```javascript
db.posts.createIndex({
  title:"text"
});
```

Search:

```javascript
db.posts.find({
  $text: {
    $search:"react"
  }
});
```

---

### Unique Index

```javascript
db.users.createIndex(
  { email:1 },
  { unique:true }
);
```

---

## 37. What is Aggregation Framework?

### Answer

Aggregation processes data through multiple stages.

Similar to SQL GROUP BY.

### Example

```javascript
db.orders.aggregate([
  {
    $group:{
      _id:"$status",
      total:{
        $sum:"$amount"
      }
    }
  }
]);
```

### Common Stages

```javascript
$match
$group
$sort
$lookup
$project
$limit
$skip
```

### Interview Answer

"Aggregation Framework allows transforming and analyzing data through a pipeline of stages."

---

## 38. Explain $match

### Answer

Filters documents.

### Example

```javascript
db.users.aggregate([
 {
   $match:{
     age:{
       $gte:18
     }
   }
 }
]);
```

Equivalent to:

```javascript
db.users.find({
 age:{
   $gte:18
 }
});
```

---

## 39. Explain $group

### Answer

Groups documents by a field.

### Example

```javascript
db.orders.aggregate([
 {
   $group:{
     _id:"$category",
     totalSales:{
       $sum:"$amount"
     }
   }
 }
]);
```

Output:

```json
[
 {
   "_id":"Electronics",
   "totalSales":5000
 }
]
```

---

## 40. Explain $lookup

### Answer

MongoDB equivalent of JOIN.

### Users Collection

```json
{
 "_id":1,
 "name":"John"
}
```

### Orders Collection

```json
{
 "userId":1,
 "amount":100
}
```

### Aggregation

```javascript
db.orders.aggregate([
 {
   $lookup:{
     from:"users",
     localField:"userId",
     foreignField:"_id",
     as:"user"
   }
 }
]);
```

### Result

Order data + User data.

---

## 41. What is Sharding?

### Answer

Sharding distributes data across multiple servers.

### Architecture

```text
Application
      ↓
Router (mongos)
      ↓
Shard1
Shard2
Shard3
```

### Benefits

- Horizontal Scaling
- High Availability
- Better Performance

### Use Cases

Large datasets (millions of records).

---

## 42. What is Replication?

### Answer

Replication creates copies of data.

### Replica Set

```text
Primary
   ↓
Secondary
   ↓
Secondary
```

### Benefits

- Fault Tolerance
- Backup
- High Availability

### Interview Point

Writes go to Primary.
Reads can be served by Secondaries.

---

## 43. What are Transactions?

### Answer

Transactions ensure multiple operations succeed or fail together.

### Example

Bank Transfer

```text
Deduct Money
Add Money
```

Both operations must complete.

### Mongoose Example

```javascript
const session =
 await mongoose.startSession();

session.startTransaction();

try {

 await User.updateOne(
   {_id:sender},
   {$inc:{balance:-100}},
   {session}
 );

 await User.updateOne(
   {_id:receiver},
   {$inc:{balance:100}},
   {session}
 );

 await session.commitTransaction();

} catch(error){

 await session.abortTransaction();

}
```

### ACID Properties

- Atomicity
- Consistency
- Isolation
- Durability

---

## 44. How Do You Optimize Slow MongoDB Queries?

### Techniques

### 1. Create Indexes

```javascript
db.users.createIndex({
 email:1
});
```

---

### 2. Use Projection

Bad:

```javascript
User.find();
```

Good:

```javascript
User.find({},{
 name:1,
 email:1
});
```

---

### 3. Pagination

Bad:

```javascript
User.find();
```

Good:

```javascript
User.find()
.limit(20)
.skip(0);
```

---

### 4. Use Lean()

```javascript
const users =
 await User.find().lean();
```

Faster because Mongoose skips document creation.

---

### 5. Avoid Unnecessary Lookup

Only join when required.

---

## 45. Difference Between Embedded Documents and References

### Embedded

```javascript
{
  name:"John",
  address:{
    city:"Delhi",
    state:"Delhi"
  }
}
```

### Benefits

- Faster Reads
- Single Query

### Drawbacks

- Data Duplication

---

### Reference

```javascript
{
  name:"John",
  addressId:ObjectId("123")
}
```

### Benefits

- No Duplication
- Better Normalization

### Drawbacks

- Extra Queries

---

### Interview Answer

Use Embedded Documents when:

- Data is frequently accessed together
- One-to-few relationship

Use References when:

- Data grows independently
- One-to-many or many-to-many relationships

Example:

Embedded:

```javascript
User -> Profile
```

Reference:

```javascript
User -> Orders
```

---

# Mongoose Questions

## 46. What is Mongoose?

### Answer

Mongoose is an ODM (Object Data Modeling) library for MongoDB.

It provides:

- Schema Validation
- Middleware
- Population
- Query Builder

Example:

```javascript
const userSchema =
 new mongoose.Schema({
   name:String,
   email:String
 });

const User =
 mongoose.model(
   "User",
   userSchema
 );
```

---

## 47. What is Population?

### Answer

Population automatically fetches referenced documents.

### Example

```javascript
const orders =
 await Order.find()
 .populate("user");
```

Equivalent to SQL JOIN.

---

## 48. What is Middleware (Hooks) in Mongoose?

### Example

```javascript
userSchema.pre(
 "save",
 async function(next){

   this.password =
   await bcrypt.hash(
     this.password,
     10
   );

   next();
 }
);
```

### Types

- pre()
- post()

### Use Cases

- Hash Password
- Audit Logs
- Validation

---

## 49. Difference Between save() and updateOne()

### save()

```javascript
const user =
 await User.findById(id);

user.name = "John";

await user.save();
```

Triggers middleware.

---

### updateOne()

```javascript
await User.updateOne(
 {_id:id},
 {name:"John"}
);
```

Faster but bypasses some document middleware.

---

## 50. Explain lean()

### Answer

Returns plain JavaScript objects instead of Mongoose documents.

```javascript
const users =
 await User.find().lean();
```

### Benefits

- Faster
- Less Memory Usage

### Use Cases

Read-only APIs
Large datasets

### Interview Answer

"I use lean() in GET APIs where document methods are not required because it significantly improves performance."