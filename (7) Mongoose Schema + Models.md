
#  What is Mongoose?

**Mongoose** is an **ODM (Object Data Modeling) library** for Node.js.
It helps you:

* define **structure (schema)** for MongoDB documents
* add **validation**
* work with data using **models & methods**

> MongoDB is schema-less, but **Mongoose gives structure** to your data.

---

#  What is a Schema?

A **Schema** defines:

* fields
* data types
* rules (required, default, unique, etc.)

 Think of Schema as a **blueprint** of your document.

### Example: User Schema

```js
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: {
    type: Number,
    min: 18
  },
  isActive: {
    type: Boolean,
    default: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

### Common Schema Types

| Type     | Example                   |
| -------- | ------------------------- |
| String   | name                      |
| Number   | age                       |
| Boolean  | isActive                  |
| Date     | createdAt                 |
| Array    | skills: [String]          |
| ObjectId | ref to another collection |

---

#  What is a Model?

**Model** is a **class created from schema**.
You use it to perform **CRUD operations**.

 Schema = structure
 Model = tool to interact with DB

### Create Model

```js
const User = mongoose.model("User", userSchema);

export default User;
```

MongoDB collection name will be:
👉 `users` (Mongoose auto plural + lowercase)

---

#  CRUD using Model

##  Create (Insert)

```js
const user = new User({
  name: "Rahul",
  email: "rahul@gmail.com",
  age: 22
});

await user.save();
```

OR

```js
await User.create({
  name: "Aman",
  email: "aman@gmail.com",
  age: 20
});
```

---

##  Read (Find)

```js
const users = await User.find();              // all users
const oneUser = await User.findOne({ email: "rahul@gmail.com" });
const byId = await User.findById("65abc123...");
```

---

## ✏ Update

```js
await User.findByIdAndUpdate(id, { age: 25 });
```

With options:

```js
await User.findByIdAndUpdate(
  id,
  { age: 25 },
  { new: true }   // updated document return karega
);
```

---

## ❌ Delete

```js
await User.findByIdAndDelete(id);
```

---

# 🔐 Validation in Schema

```js
const productSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true,
    minlength: 3
  },
  price: {
    type: Number,
    required: true,
    min: 1
  }
});
```

If invalid data bheja → Mongoose error throw karega ❌

---

# 🔗 Relations (Ref / ObjectId)

### Example: Post belongs to User

```js
const postSchema = new mongoose.Schema({
  title: String,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User"
  }
});
```

### Populate (Join jaisa)

```js
const posts = await Post.find().populate("user");
```

---

#  Interview One-Liners

**Q: What is Schema in Mongoose?**
👉 Schema defines structure & rules of MongoDB documents.

**Q: What is Model?**
👉 Model is a wrapper over schema to perform CRUD operations.

**Q: Difference between Schema & Model?**

| Schema                | Model               |
| --------------------- | ------------------- |
| Structure / Blueprint | DB interaction tool |
| Defines fields        | Used to query DB    |
| No DB access          | CRUD possible       |

**Q: Why use Mongoose when MongoDB is schema-less?**
👉 For validation, structure, cleaner code & easy relations.

---

# 🧾 README.md Format (as you asked earlier)

````md
# Mongoose Schema & Models (Basics)

## What is Mongoose?
Mongoose is an ODM(Object Data Modeling) library for Node.js that provides a schema-based solution to model MongoDB data.

## Schema
Schema defines the structure and validation rules for documents.

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, min: 18 }
});
````

## Model

Model is created from schema and used to interact with the database.

```js
const User = mongoose.model("User", userSchema);
```

## CRUD Examples

### Create

```js
await User.create({ name: "Aman", email: "aman@gmail.com", age: 20 });
```

### Read

```js
await User.find();
```

### Update

```js
await User.findByIdAndUpdate(id, { age: 25 });
```

### Delete

```js
await User.findByIdAndDelete(id);
```

```
