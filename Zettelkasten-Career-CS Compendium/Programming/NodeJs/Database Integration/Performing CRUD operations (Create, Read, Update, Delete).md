---
tags:
  - FORMAT
  - NodeJs
---
# Performing CRUD Operations in Node.js

**CRUD** stands for **Create, Read, Update, and Delete**. These are the four basic operations you can perform on data in a database. Let’s walk through each of these operations and how they are implemented using **Node.js** with both **SQL** (using Sequelize) and **NoSQL** (using Mongoose for MongoDB) databases.

---

# Create Operation (Insert Data)

The **Create** operation is used to insert new data into a database.

## In SQL (Sequelize)

Using Sequelize, you can create a new record in a relational database like **MySQL** or **PostgreSQL**.

```javascript
const { Sequelize, DataTypes } = require('sequelize');
const sequelize = new Sequelize('mysql://username:password@localhost:3306/mydb');

// Define a User model
const User = sequelize.define('User', {
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  age: {
    type: DataTypes.INTEGER,
    allowNull: false
  },
  email: {
    type: DataTypes.STRING,
    unique: true,
    allowNull: false
  }
});

// Create a new user
User.create({
  name: 'John Doe',
  age: 30,
  email: 'john.doe@example.com'
})
  .then(user => console.log('User created:', user))
  .catch(err => console.error('Error:', err));
```

## In NoSQL (Mongoose for MongoDB)

In MongoDB, using Mongoose, you can create a new document in a collection.

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost/mydb', { useNewUrlParser: true, useUnifiedTopology: true });

// Define a User schema and model
const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
  email: String
});

const User = mongoose.model('User', userSchema);

// Create a new user
const newUser = new User({
  name: 'Alice',
  age: 25,
  email: 'alice@example.com'
});

newUser.save()
  .then(user => console.log('User created:', user))
  .catch(err => console.error('Error:', err));
```

---

# Read Operation (Retrieve Data)

The **Read** operation allows you to query the database and retrieve data.

## In SQL (Sequelize)

To read data, you can use the `findAll` or `findOne` methods.

```javascript
// Find all users
User.findAll()
  .then(users => console.log('Users:', users))
  .catch(err => console.error('Error:', err));

// Find a single user by name
User.findOne({ where: { name: 'John Doe' } })
  .then(user => console.log('User found:', user))
  .catch(err => console.error('Error:', err));
```

## In NoSQL (Mongoose for MongoDB)

With Mongoose, you can retrieve documents using the `find` or `findOne` methods.

```javascript
// Find all users
User.find()
  .then(users => console.log('Users:', users))
  .catch(err => console.error('Error:', err));

// Find a single user by name
User.findOne({ name: 'Alice' })
  .then(user => console.log('User found:', user))
  .catch(err => console.error('Error:', err));
```

---

# Update Operation (Modify Data)

The **Update** operation is used to modify existing data in the database.

## In SQL (Sequelize)

To update a record in Sequelize, you can use `update` or `updateOne` methods.

```javascript
// Update a user's age
User.update({ age: 31 }, { where: { name: 'John Doe' } })
  .then(result => console.log('User updated:', result))
  .catch(err => console.error('Error:', err));
```

## In NoSQL (Mongoose for MongoDB)

To update a document in MongoDB, you can use the `updateOne`, `updateMany`, or `findOneAndUpdate` methods.

```javascript
// Update a user's age
User.updateOne({ name: 'Alice' }, { $set: { age: 26 } })
  .then(result => console.log('User updated:', result))
  .catch(err => console.error('Error:', err));

// Or using findOneAndUpdate
User.findOneAndUpdate({ name: 'Alice' }, { $set: { age: 26 } })
  .then(user => console.log('User updated:', user))
  .catch(err => console.error('Error:', err));
```

---

# Delete Operation (Remove Data)

The **Delete** operation removes a record from the database.

## In SQL (Sequelize)

To delete a record in Sequelize, you can use the `destroy` method.

```javascript
// Delete a user by name
User.destroy({ where: { name: 'John Doe' } })
  .then(result => console.log('User deleted:', result))
  .catch(err => console.error('Error:', err));
```

## In NoSQL (Mongoose for MongoDB)

To delete a document in MongoDB, you can use the `deleteOne`, `deleteMany`, or `findOneAndDelete` methods.

```javascript
// Delete a user by name
User.deleteOne({ name: 'Alice' })
  .then(result => console.log('User deleted:', result))
  .catch(err => console.error('Error:', err));

// Or using findOneAndDelete
User.findOneAndDelete({ name: 'Alice' })
  .then(user => console.log('User deleted:', user))
  .catch(err => console.error('Error:', err));
```

---

# Summary of CRUD Operations:

| Operation | SQL (Sequelize) | NoSQL (Mongoose) |
| ----- | ----- | ----- |
| **Create** | `Model.create()` | `new Model().save()` |
| **Read** | `Model.findAll()`, `Model.findOne()` | `Model.find()`, `Model.findOne()` |
| **Update** | `Model.update()` | `Model.updateOne()`, `Model.findOneAndUpdate()` |
| **Delete** | `Model.destroy()` | `Model.deleteOne()`, `Model.findOneAndDelete()` |

---

# Advanced Notes:

* **Transactions**: In SQL, **transactions** ensure that a series of operations (e.g., multiple `Create` or `Update`) either all succeed or all fail. Sequelize supports transactions. MongoDB doesn’t have the concept of transactions in the same way, but you can use **atomic operations** in MongoDB to ensure data consistency.

* **Validation**: Both **Sequelize** and **Mongoose** provide built-in validation for data before performing CRUD operations. For example, Sequelize uses data types and validation constraints, while Mongoose uses schema validation with rules like `required`, `unique`, and `min`.

---

# Conclusion

Understanding CRUD operations is fundamental for interacting with databases. In Node.js, libraries like **Sequelize** and **Mongoose** make performing these operations easier by abstracting away the low-level database interactions. Whether you're working with a **relational database** (SQL) like MySQL or PostgreSQL using Sequelize or a **NoSQL database** like MongoDB using Mongoose, performing **Create**, **Read**, **Update**, and **Delete** operations follows a consistent pattern that helps you manage your application’s data efficiently.

