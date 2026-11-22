---
tags:
  - FORMAT
  - NodeJs
---
Connecting **Node.js** with **SQL databases** (such as **MySQL**, **PostgreSQL**, or **SQLite**) is a common task when building web applications that need to interact with a relational database. You can use Node.js packages (also known as modules) to make it easier to perform database operations like querying, inserting, updating, and deleting data.

Let’s break down how to connect **Node.js** to each of these databases:

---

# Connecting Node.js to MySQL

## Step 1: Install MySQL Node.js Module

To connect to MySQL, you can use the `mysql2` package, which is a MySQL client for Node.js. You can install it using **npm**:

```
npm install mysql2
```

## Step 2: Create a Connection

Once `mysql2` is installed, you can create a connection to the MySQL database.

```javascript
const mysql = require('mysql2');

// Create a connection to the database
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',  // Replace with your MySQL username
  password: 'password',  // Replace with your MySQL password
  database: 'test_db'  // Replace with your database name
});

// Connect to the database
connection.connect((err) => {
  if (err) {
    console.error('Error connecting to the database: ' + err.stack);
    return;
  }
  console.log('Connected to the MySQL database as id ' + connection.threadId);
});

// Example Query
connection.query('SELECT * FROM users', (err, results) => {
  if (err) {
    console.error('Error executing query: ' + err.stack);
  } else {
    console.log(results);
  }
});

// Close the connection
connection.end();
```

## Important Concepts:

* `mysql.createConnection()`: This creates a connection to your MySQL database.

* `.connect()`: Establishes the connection.

* `.query()`: Executes SQL queries.

* `.end()`: Closes the connection.

# Connecting Node.js to PostgreSQL

## Step 1: Install PostgreSQL Node.js Module

To connect to PostgreSQL, you can use the `pg` package. Install it with **npm**:

```
npm install pg
```

## Step 2: Create a Connection

Once `pg` is installed, you can create a connection to the PostgreSQL database.

```javascript
const { Client } = require('pg');

// Create a new PostgreSQL client
const client = new Client({
  host: 'localhost',
  port: 5432,  // Default port for PostgreSQL
  user: 'postgres',  // Replace with your PostgreSQL username
  password: 'password',  // Replace with your PostgreSQL password
  database: 'test_db'  // Replace with your database name
});

// Connect to the database
client.connect((err) => {
  if (err) {
    console.error('Error connecting to PostgreSQL:', err.stack);
  } else {
    console.log('Connected to the PostgreSQL database');
  }
});

// Example Query
client.query('SELECT * FROM users', (err, res) => {
  if (err) {
    console.error('Error executing query:', err.stack);
  } else {
    console.log(res.rows);  // `res.rows` contains the query result
  }

  // Close the connection
  client.end();
});
```

## Important Concepts:

* `pg.Client`: This is used to establish a connection to the PostgreSQL database.

* `.connect()`: Establishes the connection.

* `.query()`: Executes SQL queries.

* `.end()`: Closes the connection.

# Connecting Node.js to SQLite

## Step 1: Install SQLite Node.js Module

To connect to SQLite, you can use the `sqlite3` package, which is a simple SQLite client for Node.js. Install it using **npm**:

```
npm install sqlite3
```

## Step 2: Create a Connection

Once `sqlite3` is installed, you can create a connection to an SQLite database.

```javascript
const sqlite3 = require('sqlite3').verbose();

// Open a database (it will create the file if it doesn't exist)
const db = new sqlite3.Database('./test_db.sqlite', (err) => {
  if (err) {
    console.error('Error opening SQLite database:', err.message);
  } else {
    console.log('Connected to SQLite database');
  }
});

// Example Query
db.all('SELECT * FROM users', (err, rows) => {
  if (err) {
    console.error('Error executing query:', err.message);
  } else {
    console.log(rows);  // `rows` contains the result of the query
  }
});

// Close the database
db.close((err) => {
  if (err) {
    console.error('Error closing database:', err.message);
  } else {
    console.log('Closed SQLite database');
  }
});
```

## Important Concepts:

* `sqlite3.Database()`: This is used to open the SQLite database (or create it if it doesn't exist).

* `.all()`: Fetches all rows for a query.

* `.close()`: Closes the SQLite database connection.

---

# CRUD Operations in SQL Databases

Once connected to a database, you can perform various **CRUD** operations (Create, Read, Update, Delete) using SQL queries.

## Create (INSERT)

**MySQL Example**:

```javascript
connection.query(
  "INSERT INTO users (name, age) VALUES (?, ?)",
  ['John Doe', 30],
  (err, results) => {
    if (err) throw err;
    console.log('User inserted with ID:', results.insertId);
  }
);
```

**PostgreSQL Example**:

```javascript
client.query(
  "INSERT INTO users (name, age) VALUES ($1, $2) RETURNING id",
  ['John Doe', 30],
  (err, res) => {
    if (err) throw err;
    console.log('User inserted with ID:', res.rows[0].id);
  }
);
```

**SQLite Example**:

```javascript
db.run(
  "INSERT INTO users (name, age) VALUES (?, ?)",
  ['John Doe', 30],
  function(err) {
    if (err) {
      console.error(err.message);
    } else {
      console.log('User inserted with ID:', this.lastID);
    }
  }
);
```

## Read (SELECT)

**MySQL Example**:

```javascript
connection.query('SELECT * FROM users', (err, results) => {
  if (err) throw err;
  console.log(results);
});
```

**PostgreSQL Example**:

```javascript
client.query('SELECT * FROM users', (err, res) => {
  if (err) throw err;
  console.log(res.rows);
});
```

**SQLite Example**:

```javascript
db.all('SELECT * FROM users', (err, rows) => {
  if (err) {
    console.error('Error fetching users:', err.message);
  } else {
    console.log(rows);
  }
});
```

## Update (UPDATE)

**MySQL Example**:

```javascript
connection.query(
  'UPDATE users SET age = ? WHERE name = ?',
  [31, 'John Doe'],
  (err, results) => {
    if (err) throw err;
    console.log(`${results.affectedRows} rows updated`);
  }
);
```

**PostgreSQL Example**:

```javascript
client.query(
  'UPDATE users SET age = $1 WHERE name = $2',
  [31, 'John Doe'],
  (err, res) => {
    if (err) throw err;
    console.log(`${res.rowCount} rows updated`);
  }
);
```

**SQLite Example**:

```javascript
db.run(
  'UPDATE users SET age = ? WHERE name = ?',
  [31, 'John Doe'],
  function(err) {
    if (err) {
      console.error(err.message);
    } else {
      console.log(`${this.changes} rows updated`);
    }
  }
);
```

## Delete (DELETE)

**MySQL Example**:

```javascript
connection.query(
  'DELETE FROM users WHERE name = ?',
  ['John Doe'],
  (err, results) => {
    if (err) throw err;
    console.log(`${results.affectedRows} rows deleted`);
  }
);
```

**PostgreSQL Example**:

```javascript
client.query(
  'DELETE FROM users WHERE name = $1',
  ['John Doe'],
  (err, res) => {
    if (err) throw err;
    console.log(`${res.rowCount} rows deleted`);
  }
);
```

**SQLite Example**:

```javascript
db.run(
  'DELETE FROM users WHERE name = ?',
  ['John Doe'],
  function(err) {
    if (err) {
      console.error(err.message);
    } else {
      console.log(`${this.changes} rows deleted`);
    }
  }
);
```

---

# Conclusion

* **MySQL**, **PostgreSQL**, and **SQLite** are widely used relational databases, and connecting them to a **Node.js** application involves using specific packages like `mysql2`, `pg`, and `sqlite3`.

* You can use these libraries to perform CRUD operations (Create, Read, Update, Delete) in your Node.js applications by executing SQL queries.

* It’s also important to handle database connections properly and close them when done to avoid memory leaks.

Each of these databases has its own specificities, but the general process of connecting, querying, and handling results is quite similar in Node.js.

