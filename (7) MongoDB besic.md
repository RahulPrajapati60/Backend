

#  MongoDB Basics (Install + CRUD with Compass)

##  What is MongoDB? (In simple words)

MongoDB is a **NoSQL database**.

* Data is stored in **documents (JSON-like)**
* No fixed table structure like SQL
* Fast & flexible
* Uses **collections** instead of tables

Example document:

```json
{
  "name": "Rahul",
  "age": 22,
  "skills": ["React", "Node"]
}
```



##  Installation (Windows)

###  Step 1: Download MongoDB

Go to official site:
 [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)

Download:

* MongoDB Community Server
* Select Windows

###  Step 2: Install MongoDB

During install:
✔ Select **Complete setup**
✔ Tick **Install MongoDB Compass**

After install:
MongoDB service automatically start ho jata hai.

---

##  What is MongoDB Compass?

MongoDB Compass = **GUI Tool**
Means database ko visually dekh sakte ho, bina terminal ke.

You can:

* Create database
* Insert data
* Update, Delete
* Run queries
* See collections

Open Compass → Click **Connect** (default localhost)

---

#  CRUD Operations using MongoDB Compass

CRUD = **Create, Read, Update, Delete**

---

## 🟢 1️ Create (Insert Data)

### Step 1: Create Database & Collection

Compass →
Click **Create Database**

Example:

```
Database Name: schoolDB
Collection Name: students
```

### Step 2: Insert Document

Click **Insert Document**

Example:

```json
{
  "name": "Aman",
  "age": 21,
  "course": "BCA"
}
```

✔ Click **Insert**

---

## 🔵 2️ Read (Find Data)

To see all data:

```js
{}
```

To find specific:

```js
{ "name": "Aman" }
```

You can also use filter bar in Compass.

---

## 🟡 3️ Update Data

Example: Update age of Aman

Filter:

```js
{ "name": "Aman" }
```

Update:

```js
{ $set: { "age": 22 } }
```

Result:
Aman age updated to 22.

---

## 🔴 4️ Delete Data

Delete Aman record:

Filter:

```js
{ "name": "Aman" }
```

Click **Delete**

Or query:

```js
{ "name": "Aman" }
```

---

#  CRUD using Mongo Shell (Optional for Interview)

```js
use schoolDB
```

### Insert

```js
db.students.insertOne({ name: "Rahul", age: 22 })
```

### Read

```js
db.students.find()
db.students.find({ name: "Rahul" })
```

### Update

```js
db.students.updateOne(
  { name: "Rahul" },
  { $set: { age: 23 } }
)
```

### Delete

```js
db.students.deleteOne({ name: "Rahul" })
```

---

#  SQL vs MongoDB (Interview Point)

| SQL (MySQL)  | MongoDB         |
| ------------ | --------------- |
| Table        | Collection      |
| Row          | Document        |
| Column       | Field           |
| Fixed Schema | Flexible Schema |

---

#  Real-life Example (Node.js + MongoDB)

```js
import mongoose from "mongoose";

mongoose.connect("mongodb://localhost:27017/schoolDB");

const studentSchema = new mongoose.Schema({
  name: String,
  age: Number
});

const Student = mongoose.model("Student", studentSchema);

// Create
await Student.create({ name: "Ravi", age: 20 });

// Read
const data = await Student.find();
console.log(data);
```

---

#  Interview Short Answer

**Q: What is MongoDB?**
MongoDB is a NoSQL document-based database used to store data in JSON-like format. It is schema-less, scalable and fast.

**Q: What is MongoDB Compass?**
MongoDB Compass is a GUI tool to manage MongoDB visually without using commands.

**Q: What is CRUD?**
CRUD stands for Create, Read, Update, Delete – basic database operations.

---
