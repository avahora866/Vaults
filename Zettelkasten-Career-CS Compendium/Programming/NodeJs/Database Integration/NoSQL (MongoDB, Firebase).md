---
tags:
  - FORMAT
  - NodeJs
---
**NoSQL** databases are a category of database management systems that do not use the traditional **relational model** of tables, rows, and columns. Instead, they store data in a more flexible and scalable way. **NoSQL** stands for **Not Only SQL**, and these databases are designed to handle large amounts of unstructured or semi-structured data, making them a popular choice for modern web applications.

Two well-known NoSQL databases are **MongoDB** and **Firebase**. Let’s dive into how they work and how to connect them to **Node.js** applications.

---

# MongoDB (Document-Oriented NoSQL Database)

**MongoDB** is one of the most popular **document-oriented** NoSQL databases. In MongoDB, data is stored in **documents** (in BSON format, which is similar to JSON), and these documents are grouped into **collections**. It is designed to scale out easily and handle high-velocity, unstructured data.

## Features of MongoDB:

* **Document-Oriented**: Data is stored in JSON-like documents, making it easy to represent complex relationships.

* **Schema-less**: MongoDB is flexible, meaning the structure of documents within a collection can differ.

* **Scalable**: MongoDB supports horizontal scaling (sharding), which makes it great for applications that need to handle large amounts of data.

* **Query Language**: MongoDB uses a query language that is similar to SQL, but it is designed for querying documents.

## Step 1: Install MongoDB Node.js Driver

To interact with MongoDB from a **Node.js** application, you can use the `mongoose` package, which is an Object Data Modeling (ODM) library for MongoDB and Node.js. Mongoose makes it easier to work with MongoDB by providing a schema-based solution for modeling application data.

Install it using npm:

```
npm install mongoose
```

## Step 2: Connect Node.js to MongoDB

```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/mydb', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
  .then(() => console.log('Connected to MongoDB'))
  .catch((err) => console.error('Could not connect to MongoDB:', err));

// Define a schema for the "User" collection
const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
  email: String
});

// Create a model from the schema
const User = mongoose.model('User', userSchema);

// Create a new user
const newUser = new User({
  name: 'John Doe',
  age: 30,
  email: 'johndoe@example.com'
});

// Save the user to the database
newUser.save()
  .then(() => console.log('User saved'))
  .catch((err) => console.error('Error saving user:', err));

// Query the database to find all users
User.find()
  .then((users) => console.log('Users:', users))
  .catch((err) => console.error('Error fetching users:', err));
```

## Key Concepts in MongoDB:

* **Database**: A container for collections of documents.

* **Collection**: A group of MongoDB documents (similar to a table in SQL).

* **Document**: A single record in a collection (similar to a row in SQL).

* **Mongoose Schema**: Defines the structure of the document in MongoDB.

## CRUD Operations in MongoDB:

* **Create**:

```javascript
const newUser = new User({ name: 'Alice', age: 25, email: 'alice@example.com' });
newUser.save();
```

*   
  **Read**:

```javascript
User.find({}, (err, users) => { console.log(users); });
```

*   
  **Update**:

```javascript
User.updateOne({ name: 'Alice' }, { $set: { age: 26 } }, (err, result) => { console.log(result); });
```

*   
  **Delete**:

```javascript
User.deleteOne({ name: 'Alice' }, (err, result) => { console.log(result); });
```

---

# Firebase (Realtime Database and Firestore)

**Firebase** is a Backend-as-a-Service (BaaS) platform provided by Google. It includes multiple services, but one of the core features is its **NoSQL database**. Firebase provides two types of databases: **Firebase Realtime Database** and **Firestore**.

## Firebase Realtime Database:

* **Realtime Database** is a cloud-hosted NoSQL database that supports data syncing in real-time.

* It stores data in JSON format and allows clients to read and write data in real time.

## Cloud Firestore:

* **Firestore** is the newer, more flexible database service by Firebase.

* It is a scalable NoSQL cloud database with a more structured data model than Realtime Database.

* It uses collections and documents, similar to MongoDB, and supports richer queries than the Realtime Database.

## Step 1: Install Firebase SDK for Node.js

To interact with Firebase from a **Node.js** application, you need to install the Firebase Admin SDK:

```
npm install firebase-admin
```

## Step 2: Initialize Firebase Admin SDK

You need to initialize Firebase Admin SDK with your Firebase credentials. You can obtain these credentials by generating a private key file from the Firebase Console.

```javascript
const admin = require('firebase-admin');

// Initialize Firebase
const serviceAccount = require('./path/to/serviceAccountKey.json');
admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: 'https://your-project-id.firebaseio.com'  // Use your Firebase Database URL
});

// Firestore database reference
const db = admin.firestore();
const usersCollection = db.collection('users');

// Add data to Firestore
usersCollection.add({
  name: 'John Doe',
  age: 30,
  email: 'johndoe@example.com'
})
  .then(docRef => {
    console.log('Document written with ID: ', docRef.id);
  })
  .catch(error => {
    console.error('Error adding document: ', error);
  });
```

## Firestore CRUD Operations:

* **Create**:

```javascript
const newUserRef = usersCollection.add({
  name: 'Alice',
  age: 25,
  email: 'alice@example.com'
});
```

*   
  **Read**:

```javascript
usersCollection.get()
  .then(snapshot => {
    snapshot.forEach(doc => {
      console.log(doc.id, '=>', doc.data());
    });
  });
```

*   
  **Update**:

```javascript
const userDocRef = usersCollection.doc('document_id');
userDocRef.update({ age: 26 });
```

*   
  **Delete**:

```javascript
usersCollection.doc('document_id').delete();
```

## Realtime Database:

If you're using Firebase's **Realtime Database**, here's how you can perform basic operations:

```javascript
const db = admin.database();
const ref = db.ref('users');

// Write data
ref.push({
  name: 'John Doe',
  age: 30,
  email: 'johndoe@example.com'
});

// Read data
ref.once('value', (snapshot) => {
  console.log(snapshot.val());
});

// Update data
ref.child('user_id').update({ age: 31 });

// Delete data
ref.child('user_id').remove();
```

---

# Comparing MongoDB and Firebase:

| Feature | MongoDB | Firebase (Firestore, Realtime DB) |
| ----- | ----- | ----- |
| **Data Model** | Document-based (JSON/BSON format) | Document-based (Firestore) or JSON (Realtime DB) |
| **Scalability** | Horizontal scaling (Sharding) | Horizontal scaling (Firestore) |
| **Real-Time** | Not built-in, but can use change streams | Built-in real-time syncing (Realtime DB and Firestore) |
| **Querying** | Powerful queries, rich aggregation | Firestore supports rich queries, but Realtime DB is simpler |
| **Offline Support** | No built-in offline support | Built-in offline support (Firestore) |
| **Authentication** | Not provided by default | Built-in authentication (Firebase Auth) |
| **Hosting** | Requires external hosting | Firebase provides hosting, auth, and more |

---

# Conclusion

* **MongoDB** is a great choice if you need a flexible, scalable document database with powerful query capabilities. It is ideal for applications where you need a schema-less approach with the ability to scale horizontally.

* **Firebase** (Firestore or Realtime Database) is a powerful BaaS platform that offers real-time data syncing, easy-to-use SDKs, and built-in support for authentication, hosting, and more. Firestore is a great choice for applications that need offline capabilities and real-time data syncing.

Both are highly scalable NoSQL databases, but your choice will depend on your specific use case and the features you need.

