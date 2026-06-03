
# Project-Based Interview Questions

## 151. Explain Your Current Project Architecture

### Sample Answer

```text
Frontend:
React + Redux Toolkit + React Query

Backend:
Node.js + Express.js

Database:
MongoDB

Caching:
Redis

Authentication:
JWT + Refresh Token

Deployment:
AWS EC2 + Nginx

CI/CD:
GitHub Actions
```

### Architecture

```text
React Frontend
      ↓
Nginx
      ↓
Express API
      ↓
Redis Cache
      ↓
MongoDB
```

### Interview Tip

Always explain:

- Project Goal
- Architecture
- Your Responsibilities
- Challenges
- Optimizations

---

## 152. What Was Your Role in the Project?

### Sample Answer

```text
1. Developed REST APIs
2. Designed MongoDB schemas
3. Implemented JWT authentication
4. Integrated third-party APIs
5. Optimized database queries
6. Deployed application on AWS
7. Fixed production issues
```

Interviewers want YOUR contribution.

---

## 153. Describe a Challenging Production Issue

### Sample Answer

Problem:

```text
Product listing API became slow.
```

Investigation:

```text
MongoDB query taking 3-4 seconds.
```

Root Cause:

```text
Missing index.
```

Solution:

```javascript
db.products.createIndex({
 category:1,
 status:1
});
```

Result:

```text
Response time reduced
from 4 seconds to 150ms.
```

---

## 154. How Did You Optimize Performance?

### Example Answer

### Frontend

```text
React.memo
Code Splitting
Lazy Loading
Image Optimization
```

### Backend

```text
Redis Cache
Database Indexing
Pagination
Compression
```

### Database

```text
Compound Indexes
Aggregation Optimization
```

---

## 155. Explain Authentication Flow in Your Project

### Flow

```text
Login
 ↓
Verify Password
 ↓
Generate Access Token
 ↓
Generate Refresh Token
 ↓
Store Refresh Token
 ↓
Return Tokens
```

### Protected Route

```text
Frontend
 ↓
JWT Token
 ↓
Middleware
 ↓
Verify Token
 ↓
Allow Access
```

---

# System Design Questions

## 156. Design URL Shortener

### Requirements

```text
Input:
https://google.com

Output:
short.ly/abc123
```

### Architecture

```text
User
 ↓
API
 ↓
Generate Unique ID
 ↓
Store Mapping
 ↓
MongoDB
```

### Database

```javascript
{
 shortId:"abc123",
 originalUrl:"https://..."
}
```

### Redirection

```text
short.ly/abc123
 ↓
Find URL
 ↓
Redirect
```

---

## 157. Design Chat Application

### Requirements

```text
Real-time Messaging
Online Status
Read Receipts
Group Chat
```

### Architecture

```text
React
 ↓
Socket.IO
 ↓
Node.js
 ↓
MongoDB
```

### Message Schema

```javascript
{
 senderId:"",
 receiverId:"",
 message:"",
 createdAt:""
}
```

---

## 158. Design Notification System

### Flow

```text
Order Created
 ↓
Queue
 ↓
Worker
 ↓
Email/SMS/Push
```

### Tools

```text
Redis Queue
BullMQ
RabbitMQ
```

---

## 159. Design E-Commerce Backend

### Modules

```text
Auth
Products
Cart
Orders
Payments
Inventory
```

### Database

```text
Users
Products
Orders
Payments
Reviews
```

### Architecture

```text
Frontend
 ↓
API
 ↓
Redis
 ↓
MongoDB
```

---

## 160. How Would You Handle 1 Million Requests?

### Techniques

### Load Balancer

```text
Nginx
```

### Multiple Servers

```text
Server1
Server2
Server3
```

### Redis Cache

```text
Reduce DB Hits
```

### Database Indexes

```text
Faster Queries
```

### CDN

```text
Static Assets
```

---

# Redis Questions

## 161. Why Redis?

### Uses

```text
Caching
Sessions
Rate Limiting
Queues
Pub/Sub
```

### Benefits

```text
In-memory
Very Fast
```

---

## 162. Redis vs MongoDB

| Redis | MongoDB |
|---------|----------|
| In Memory | Disk Based |
| Cache | Database |
| Faster | Persistent |
| Key Value | Document |

---

## 163. Rate Limiting with Redis

### Example

```text
User Requests API
 ↓
Increment Count
 ↓
Check Limit
 ↓
Allow/Block
```

### Benefit

Prevent abuse.

---

# Docker Questions

## 164. What is Docker?

### Answer

Docker packages application and dependencies into containers.

### Benefits

```text
Consistency
Portability
Isolation
```

---

## 165. Difference Between VM and Docker

| VM | Docker |
|------|-------|
| Heavy | Lightweight |
| Own OS | Shared OS |
| Slower | Faster |

---

## 166. Basic Dockerfile

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

EXPOSE 5000

CMD ["npm","start"]
```

---

## 167. Dockerize MERN Application

### Backend

```dockerfile
Node Container
```

### Frontend

```dockerfile
React Container
```

### Database

```dockerfile
Mongo Container
```

### Compose

```yaml
services:
 frontend:
 backend:
 mongodb:
```

---

# CI/CD Questions

## 168. What is CI/CD?

### CI

Continuous Integration

```text
Code Push
 ↓
Build
 ↓
Test
```

### CD

Continuous Deployment

```text
Deploy Automatically
```

---

## 169. GitHub Actions Example

```yaml
name: Deploy

on:
 push:
  branches:
   - main

jobs:
 build:
  runs-on:
   ubuntu-latest
```

### Purpose

Automate testing and deployment.

---

## 170. Deployment Process

### Flow

```text
Developer Pushes Code
 ↓
GitHub Actions
 ↓
Run Tests
 ↓
Build Docker Image
 ↓
Deploy AWS
```

---

# AWS Questions

## 171. What AWS Services Have You Used?

### Common Answer

```text
EC2
S3
CloudFront
RDS
IAM
Route53
```

---

## 172. What is EC2?

### Answer

Elastic Compute Cloud.

Virtual Server in AWS.

### Uses

```text
Host Backend
Host Full Application
```

---

## 173. What is S3?

### Answer

Simple Storage Service.

### Uses

```text
Images
Videos
Documents
Backups
```

---

## 174. What is CloudFront?

### Answer

AWS CDN.

### Benefits

```text
Faster Delivery
Global Distribution
```

---

## 175. What is IAM?

### Answer

Identity and Access Management.

Controls permissions.

Example:

```text
Admin
Developer
Read Only
```

---

# Microservices Questions

## 176. Monolith vs Microservices

### Monolith

```text
Single Application
```

### Microservices

```text
Auth Service
Product Service
Order Service
Payment Service
```

### Benefits

```text
Independent Scaling
Independent Deployment
```

---

## 177. Communication Between Microservices

### Methods

```text
REST
gRPC
RabbitMQ
Kafka
```

---

## 178. Why Microservices?

### Benefits

```text
Scalable
Flexible
Independent Teams
```

### Drawbacks

```text
Complexity
Monitoring
Networking
```

---

## 179. What is API Gateway?

### Architecture

```text
Client
 ↓
API Gateway
 ↓
Services
```

### Benefits

```text
Authentication
Routing
Rate Limiting
```

---

## 180. What is Service Discovery?

### Answer

Helps services locate each other dynamically.

### Tools

```text
Consul
Eureka
Kubernetes
```

---

# Behavioral Questions

## 181. Tell Me About Yourself

### Structure

```text
Current Role
Experience
Tech Stack
Achievements
```

### Sample

"I am a MERN Stack Developer with 3 years of experience building scalable web applications using React, Node.js, Express, and MongoDB."

---

## 182. Why Are You Changing Jobs?

### Good Answer

```text
Career Growth
Learning Opportunities
Larger Challenges
```

Avoid:

```text
Bad Manager
Bad Company
```

---

## 183. Biggest Technical Challenge?

### Structure

```text
Problem
Investigation
Solution
Result
```

---

## 184. How Do You Handle Production Bugs?

### Process

```text
Identify
Reproduce
Analyze Logs
Fix
Test
Deploy
Monitor
```

---

## 185. How Do You Learn New Technologies?

### Example

```text
Documentation
Projects
Courses
Open Source
```

---

## 186. Describe a Conflict in a Team

### Structure

```text
Situation
Task
Action
Result
```

Use STAR Method.

---

## 187. How Do You Estimate Tasks?

### Process

```text
Requirement Analysis
Break Into Tasks
Estimate Complexity
Add Buffer
```

---

## 188. How Do You Review Code?

### Checklist

```text
Readability
Performance
Security
Testing
Best Practices
```

---

## 189. What Makes Good Software?

### Answer

```text
Scalable
Maintainable
Secure
Testable
Reliable
```

---

## 190. Where Do You See Yourself in 5 Years?

### Sample

"I want to grow into a Senior Full-Stack Engineer or Technical Lead role while continuing to deepen my expertise in scalable system design and cloud technologies."

---

# Bonus Production Questions

## 191. How Do You Monitor Applications?

Tools:

```text
Winston
Morgan
Datadog
New Relic
Grafana
Prometheus
```

---

## 192. What Logging Strategy Do You Use?

Levels:

```text
INFO
WARN
ERROR
DEBUG
```

---

## 193. How Do You Handle File Uploads?

Libraries:

```text
Multer
AWS S3
Cloudinary
```

---

## 194. How Do You Handle Large CSV Imports?

### Approach

```text
Streams
Queues
Batch Processing
```

---

## 195. How Do You Send Emails?

### Tools

```text
Nodemailer
AWS SES
SendGrid
```

---

## 196. How Do You Implement Background Jobs?

### Tools

```text
BullMQ
Redis
RabbitMQ
```

---

## 197. How Do You Handle Database Migrations?

### Process

```text
Version Control
Migration Scripts
Rollback Support
```

---

## 198. What Happens When an API Becomes Slow?

### Investigation

```text
Logs
Indexes
Profiling
Caching
Infrastructure
```

---

## 199. How Do You Secure Secrets?

### Store In

```text
Environment Variables
AWS Secrets Manager
Vault
```

Never commit secrets to Git.

---

## 200. What Are the Top Skills of a Senior MERN Developer?

### Technical

```text
System Design
Performance Optimization
Security
Cloud
CI/CD
```

### Soft Skills

```text
Communication
Leadership
Problem Solving
Mentoring
```

### Final Interview Answer

"A strong MERN developer should be able to build scalable applications end-to-end, optimize performance, ensure security, design maintainable architectures, and collaborate effectively with teams."