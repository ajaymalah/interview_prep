# Authentication & Security Interview Questions

## 51. What is Authentication?

### Answer

Authentication verifies the identity of a user.

Example:

```text
Email + Password
```

The system checks whether the credentials are valid.

### Examples

- Login with Email & Password
- OTP Login
- Google Login
- GitHub Login

### Real Flow

```text
User Login
      ↓
Verify Credentials
      ↓
Generate Token
      ↓
Send Response
```

### Interview Answer

"Authentication is the process of verifying who the user is."

---

## 52. What is Authorization?

### Answer

Authorization determines what an authenticated user is allowed to do.

Example:

```text
Admin → Create/Delete Users

Customer → View Products
```

### Flow

```text
Login
 ↓
Authentication
 ↓
Authorization
 ↓
Access Resource
```

### Interview Answer

"Authentication identifies the user, while authorization determines their permissions."

---

## 53. Authentication vs Authorization

| Authentication | Authorization |
|---------------|---------------|
| Who are you? | What can you do? |
| Login Process | Permission Process |
| First Step | Second Step |
| User Identity | User Access Rights |

### Example

```text
Login Success
     ↓
User Role Check
     ↓
Access Granted
```

---

## 54. What is JWT?

### Answer

JWT stands for JSON Web Token.

Used for stateless authentication.

### Structure

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

### Header

```json
{
 "alg":"HS256",
 "typ":"JWT"
}
```

### Payload

```json
{
 "userId":"123",
 "role":"admin"
}
```

### Signature

```javascript
HMACSHA256(
  header + payload,
  secret
)
```

### Benefits

- Stateless
- Scalable
- Compact

---

## 55. How Do You Generate JWT?

### Example

```javascript
const jwt = require("jsonwebtoken");

const token = jwt.sign(
 {
   userId:user._id,
   role:user.role
 },
 process.env.JWT_SECRET,
 {
   expiresIn:"1h"
 }
);
```

### Response

```json
{
  "token":"eyJhbGci..."
}
```

---

## 56. How Do You Verify JWT?

### Example

```javascript
const decoded =
 jwt.verify(
   token,
   process.env.JWT_SECRET
 );
```

### Middleware

```javascript
const authMiddleware =
 (req,res,next)=>{

  const token =
  req.headers.authorization;

  if(!token){
    return res.status(401);
  }

  const decoded =
  jwt.verify(
    token,
    process.env.JWT_SECRET
  );

  req.user = decoded;

  next();
 };
```

---

## 57. What Are Access Tokens and Refresh Tokens?

### Access Token

Short lifespan.

Example:

```text
15 minutes
```

Used for API requests.

---

### Refresh Token

Long lifespan.

Example:

```text
7 days
```

Used to generate new access tokens.

---

### Flow

```text
Login
 ↓
Access Token
Refresh Token
 ↓
Access Token Expires
 ↓
Use Refresh Token
 ↓
New Access Token
```

### Benefits

- Better Security
- Improved User Experience

---

## 58. Why Use Refresh Tokens?

### Problem

Long-lived JWTs are risky.

```text
JWT valid for 30 days
```

If stolen, attacker gets access for 30 days.

---

### Solution

```text
Access Token = 15 min
Refresh Token = 7 days
```

### Benefit

Reduced attack window.

---

## 59. Where Should JWT Be Stored?

### Option 1: LocalStorage

```javascript
localStorage.setItem(
 "token",
 token
);
```

Pros:

- Easy

Cons:

- Vulnerable to XSS

---

### Option 2: HttpOnly Cookies

Pros:

- Cannot be accessed via JavaScript
- More secure

Cons:

- Requires CSRF protection

### Interview Answer

"In production applications I prefer HttpOnly Secure Cookies because they provide better protection against XSS attacks."

---

## 60. What is Password Hashing?

### Answer

Passwords should never be stored in plain text.

Bad:

```json
{
 "password":"admin123"
}
```

Good:

```json
{
 "password":"$2b$10..."
}
```

### Hashing Library

```javascript
bcrypt
```

---

## 61. How Do You Hash Passwords?

### Example

```javascript
const bcrypt =
 require("bcrypt");

const hashedPassword =
 await bcrypt.hash(
   password,
   10
 );
```

Store:

```javascript
user.password =
 hashedPassword;
```

---

## 62. How Do You Compare Passwords?

### Example

```javascript
const isMatch =
 await bcrypt.compare(
   password,
   user.password
 );
```

### Login Flow

```javascript
if(!isMatch){
 throw Error("Invalid");
}
```

---

## 63. What is OAuth?

### Answer

OAuth allows login using third-party providers.

Examples:

- Google
- GitHub
- Facebook
- LinkedIn

### Flow

```text
User Clicks Login
 ↓
Google Authentication
 ↓
Google Returns Token
 ↓
Application Login Success
```

### Benefits

- No Password Storage
- Better User Experience

---

## 64. What is RBAC?

### Answer

RBAC = Role Based Access Control.

### Example

```text
Admin
Manager
User
```

### Middleware

```javascript
const authorize =
 (...roles) => {

 return (
  req,res,next
 ) => {

  if(
   !roles.includes(
    req.user.role
   )
  ){
   return res.status(403);
  }

  next();
 };
};
```

### Route

```javascript
app.delete(
 "/users",
 authMiddleware,
 authorize("admin"),
 deleteUser
);
```

---

# Security Questions

## 65. What is XSS?

### Cross Site Scripting

Attackers inject malicious JavaScript into web pages.

### Example

```html
<script>
alert("Hacked")
</script>
```

### Risks

- Cookie Theft
- Session Hijacking
- Data Theft

---

## 66. How Do You Prevent XSS?

### 1. Input Sanitization

```javascript
sanitize-html
```

### 2. Escape HTML

```javascript
DOMPurify
```

### 3. Content Security Policy

```javascript
helmet()
```

### 4. HttpOnly Cookies

Prevents cookie theft.

---

## 67. What is CSRF?

### Cross Site Request Forgery

Victim is tricked into performing actions unknowingly.

### Example

User logged into bank.

Attacker sends malicious request:

```html
<img src=
"https://bank.com/transfer?amount=1000">
```

Browser automatically sends session cookie.

---

## 68. How Do You Prevent CSRF?

### Techniques

#### CSRF Tokens

```javascript
csurf
```

#### SameSite Cookies

```javascript
sameSite:"strict"
```

#### Double Submit Cookies

Additional verification.

---

## 69. What is NoSQL Injection?

### Example

Bad Code

```javascript
User.findOne({
 email:req.body.email,
 password:req.body.password
});
```

Attacker sends:

```json
{
 "email":{
   "$ne":null
 },
 "password":{
   "$ne":null
 }
}
```

May bypass authentication.

---

## 70. How Do You Prevent NoSQL Injection?

### Validation

```javascript
Joi
```

### Express Validator

```javascript
express-validator
```

### Sanitize Input

```javascript
mongo-sanitize
```

---

## 71. What is Helmet?

### Answer

Helmet secures Express applications by setting HTTP headers.

### Example

```javascript
const helmet =
 require("helmet");

app.use(helmet());
```

### Protection

- XSS
- Clickjacking
- MIME Sniffing

---

## 72. What is Rate Limiting?

### Answer

Limits requests from a client.

### Example

```javascript
const limiter =
 rateLimit({
  windowMs:
   15*60*1000,
  max:100
 });
```

### Benefits

- Prevent brute force
- Prevent DDoS
- Reduce abuse

---

## 73. How Do You Secure a Production MERN Application?

### Backend

✅ JWT Authentication

✅ Refresh Tokens

✅ Rate Limiting

✅ Helmet

✅ Input Validation

✅ Password Hashing

✅ HTTPS

✅ Environment Variables

---

### Frontend

✅ Secure Cookies

✅ CSP Headers

✅ Route Protection

✅ Input Sanitization

---

### Database

✅ MongoDB Authentication

✅ Least Privilege Access

✅ Index Optimization

---

## 74. What Environment Variables Do You Store?

### Example

```env
PORT=5000

MONGO_URI=...

JWT_SECRET=...

JWT_REFRESH_SECRET=...

REDIS_URL=...

SMTP_USER=...

SMTP_PASSWORD=...
```

### Why?

Prevent exposing sensitive information.

---

## 75. Explain Complete Login Flow in MERN

### Step 1

User enters credentials.

```text
Email + Password
```

---

### Step 2

Backend validates user.

```javascript
User.findOne()
```

---

### Step 3

Compare password.

```javascript
bcrypt.compare()
```

---

### Step 4

Generate Access Token.

```javascript
jwt.sign()
```

---

### Step 5

Generate Refresh Token.

```javascript
jwt.sign()
```

---

### Step 6

Store Refresh Token.

```javascript
Database
or
Redis
```

---

### Step 7

Send Response.

```json
{
 "accessToken":"...",
 "refreshToken":"..."
}
```

---

### Step 8

Frontend stores tokens.

Prefer:

```text
HttpOnly Secure Cookies
```

---

### Step 9

Protected APIs use middleware.

```javascript
jwt.verify()
```

---

### Step 10

Access token expires.

Frontend calls:

```text
/refresh-token
```

New token generated.

### Interview Answer

"I implement JWT authentication using short-lived access tokens and long-lived refresh tokens stored in HttpOnly secure cookies. Passwords are hashed with bcrypt, APIs are protected using middleware, and role-based access control is implemented for authorization."