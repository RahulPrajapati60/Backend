
## 🌐 REST API 

A **REST API** is a way for **frontend (website/app)** to talk to **backend (server)**.

  Example

* You click **Login**
* App sends data to server
* Server checks and sends response

This communication happens using **URLs + HTTP methods**.

---

## 🔧 HTTP Methods (Easy Meaning)

| Method | What it does  |
| ------ | ------------- |
| GET    | Get data      |
| POST   | Send new data |
| PUT    | Update data   |
| DELETE | Delete data   |

Example

```http
GET /users
```

 means “give me all users”

---

##  REST API URL Example

```http
POST /users
```

 create a new user

```http
GET /users/1
```

 get user with id 1

---

##  Data Format

REST APIs mostly use **JSON**

Example

```json
{
  "name": "Guru",
  "email": "guru@gmail.com"
}
```

---

##  Postman 

**Postman** is a tool to **test APIs without frontend**.

 It helps you

* Send requests
* See responses
* Check errors

---

##  How Postman Works 

1. Open Postman
2. Choose method (GET / POST)
3. Enter API URL
4. Add data (if needed)
5. Click **Send**
 

---

##  Example: Test API in Postman

### Create User

* Method: **POST**
* URL:

```http
http://localhost:3000/users
```

* Body

```json
{
  "name": "Guru"
}
```

Click **Send** ➝ user created 

---

##  Simple Flow

```text
Postman / Frontend
        ↓
     REST API
        ↓
     Backend
        ↓
     Database
```

---

###  summary 

**REST API** = rules to send & receive data
**Postman** = tool to test those rules
