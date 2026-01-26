
## 1️ What is a REST API?

**REST (Representational State Transfer)** is an architectural style used to build **web APIs** that allow **client ↔ server communication** using HTTP.

 A **REST API** lets a client (browser, mobile app, Postman, frontend)
**request / send data** to a backend server in a **standardized way**.



## 2️ Core Principles of REST API Design

### 1. **Client–Server Architecture**

* Frontend (React, Angular) = **client**
* Backend (Node, Java, Spring, etc.) = **server**
* They are independent of each other


### 2. **Stateless**

* Each request contains **all required information**
* Server does **not remember previous requests**

Example

```http
GET /users/101
Authorization: Bearer <token>
```

---

### 3. **Resource-Based**

* Everything is a **resource**
* Resources are represented using **URLs**

 Good

```
/users
/users/101
/orders/25
```

❌ Bad:

```
/getUser
/createUser
```

---

### 4. **Use HTTP Methods Correctly**

| HTTP Method | Purpose                | Example           |
| ----------- | ---------------------- | ----------------- |
| GET         | Read data              | GET /users        |
| POST        | Create data            | POST /users       |
| PUT         | Update entire resource | PUT /users/101    |
| PATCH       | Partial update         | PATCH /users/101  |
| DELETE      | Remove data            | DELETE /users/101 |

---

### 5. **Use Proper HTTP Status Codes**

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |

Example

```json
{
  "message": "User created successfully"
}
```

Status: **201 Created**

---

## 3️ REST API Endpoint Design 

### User Management API

| Action         | Method | Endpoint     |
| -------------- | ------ | ------------ |
| Get all users  | GET    | /api/users   |
| Get user by ID | GET    | /api/users/1 |
| Create user    | POST   | /api/users   |
| Update user    | PUT    | /api/users/1 |
| Delete user    | DELETE | /api/users/1 |

---

### Sample POST Request Body

```json
{
  "name": "Guru",
  "email": "guru@gmail.com",
  "password": "123456"
}
```

---

### Sample Response

```json
{
  "id": 1,
  "name": "Guru",
  "email": "guru@gmail.com"
}
```

---

## 4️ REST API Best Practices (Very Important for Interviews)

 Use **nouns**, not verbs
 Use **plural resource names** (`/users`)
 Version your API:

```
/api/v1/users
```

 Use **JSON** format
 Secure APIs with **JWT / OAuth**
 Handle errors properly

Error example

```json
{
  "error": "User not found",
  "status": 404
}
```

---

## 5️ What is Postman?

**Postman** is a tool used to:

* Test REST APIs
* Send HTTP requests
* Check responses
* Debug backend APIs

 Used by **developers, testers, backend engineers**

---

## 6️ Postman Testing – Step by Step

### Step 1. Create a Request

* Open Postman
* Click **New → HTTP Request**
* Select Method (GET / POST / PUT / DELETE)

---

### Step 2. Enter API URL

```
http://localhost:8080/api/users
```

---

### Step 3. Headers (Important)

```
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>
```

---

### Step 4. Body (for POST / PUT)

* Choose **Body → raw → JSON**

```json
{
  "name": "Guru",
  "email": "guru@gmail.com"
}
```

---

### Step 5. Send Request

* Click **Send**
* Check

  * Status code
  * Response body
  * Response time

---

## 7️ Postman Testing Example (CRUD)

### 1️ GET Users

* Method- GET
* URL- `/api/users`
* Expected

  * Status- 200
  * JSON list of users

---

### 2️ POST User

* Method- POST
* Body- JSON
* Expected

  * Status- 201
  * Created user data

---

### 3️ PUT User

* Method- PUT
* URL- `/api/users/1`
* Expected

  * Status- 200
  * Updated data

---

### 4️ DELETE User

* Method- DELETE
* URL- `/api/users/1`
* Expected

  * Status- 204 (No Content)

---

## 8️ Postman Tests (Automation – Basic)

Postman allows writing **tests in JavaScript**

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

Check response time

```javascript
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});
```

---

---

##  One-Line Overview 

> **REST API** is a stateless, client-server architecture that uses HTTP methods to perform CRUD operations on resources, and **Postman** is used to test, validate, and automate API requests and responses.

---

