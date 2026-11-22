---
tags:
  - FORMAT
  - NodeJs
---
Sure\! Node.js comes with a variety of **built-in modules** that provide core functionality for handling common tasks, such as working with files, HTTP servers, cryptography, and more. These modules are available for use right out of the box without needing to install any external packages.

Here’s an overview of some commonly used built-in modules in Node.js:

---

# `fs` (File System)

The `fs` module provides an API for interacting with the file system. You can use it to read, write, delete, and manipulate files and directories.

## Common Methods:

* **Reading Files:**

```javascript
const fs = require('fs');

// Synchronous
const data = fs.readFileSync('file.txt', 'utf8');
console.log(data);

// Asynchronous
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

*   
  **Writing Files:**

```javascript
// Synchronous
fs.writeFileSync('output.txt', 'Hello, Node.js!');

// Asynchronous
fs.writeFile('output.txt', 'Hello, Node.js!', (err) => {
  if (err) throw err;
  console.log('File saved!');
});
```

*   
  **Creating Directories:**

```javascript
fs.mkdirSync('newDir'); // Synchronous
fs.mkdir('newDir', (err) => { // Asynchronous
  if (err) throw err;
  console.log('Directory created!');
});
```

---

# `path`

The `path` module provides utilities for working with file and directory paths. It can help you handle file system paths in a cross-platform way (i.e., it works on both Windows and Unix-like systems).

## Common Methods:

* **Join paths:**

```javascript
const path = require('path');
const filePath = path.join('folder', 'file.txt');
console.log(filePath); // 'folder/file.txt' (or 'folder\\file.txt' on Windows)
```

*   
  **Get the directory name:**

```javascript
const dir = path.dirname('/folder/file.txt');
console.log(dir); // '/folder'
```

*   
  **Get the file extension:**

```javascript
const ext = path.extname('file.txt');
console.log(ext); // '.txt'
```

*   
  **Normalize a path (resolve '..' and '.')**

```javascript
const normalized = path.normalize('folder/../file.txt');
console.log(normalized); // 'file.txt'
```

---

# `http`

The `http` module allows you to create an HTTP server and handle HTTP requests. It's used for building web servers in Node.js.

## Common Methods:

* **Create a simple HTTP server:**

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, world!');
});

server.listen(3000, () => {
  console.log('Server is listening on port 3000');
});
```

*   
  **Make an HTTP request:**

```javascript
const http = require('http');

http.get('http://example.com', (res) => {
  let data = '';
  res.on('data', (chunk) => {
    data += chunk;
  });
  res.on('end', () => {
    console.log(data);
  });
});
```

---

# `os` (Operating System)

The `os` module provides operating system-related utility methods. You can use it to get information about the operating system, like the system's architecture, platform, and network interfaces.

## Common Methods:

* **Get the system's platform:**

```javascript
const os = require('os');
console.log(os.platform()); // 'linux', 'darwin', 'win32'
```

*   
  **Get the system's CPU architecture:**

```javascript
console.log(os.arch()); // 'x64', 'arm'
```

*   
  **Get the system's free memory:**

```javascript
console.log(os.freemem()); // Returns the free memory in bytes
```

*   
  **Get the system's hostname:**

```javascript
console.log(os.hostname());
```

*   
  **Get the system's uptime:**

```javascript
console.log(os.uptime()); // Returns the uptime in seconds
```

---

# `events`

The `events` module provides an EventEmitter class, which is used for handling events and event-driven programming in Node.js.

## Common Methods:

* **Create an EventEmitter:**

```javascript
const EventEmitter = require('events');
const myEmitter = new EventEmitter();

// Listener
myEmitter.on('event', () => {
  console.log('An event occurred!');
});

// Emit an event
myEmitter.emit('event');
```

*   
  **Emit an event with arguments:**

```javascript
myEmitter.on('greet', (name) => {
  console.log(`Hello, ${name}!`);
});

myEmitter.emit('greet', 'Alice');
```

---

# `crypto` (Cryptography)

The `crypto` module provides cryptographic functionality, such as hashing, encryption, decryption, and more. It's commonly used for securing data.

## Common Methods:

* **Create a hash:**

```javascript
const crypto = require('crypto');
const hash = crypto.createHash('sha256');
hash.update('some data');
console.log(hash.digest('hex')); // Prints the hash as a hexadecimal string
```

*   
  **Create a HMAC (Hash-based Message Authentication Code):**

```javascript
const hmac = crypto.createHmac('sha256', 'secretkey');
hmac.update('message');
console.log(hmac.digest('hex'));
```

*   
  **Encrypt and Decrypt data:**

```javascript
const algorithm = 'aes-256-cbc';
const password = 'password';
const iv = crypto.randomBytes(16);
const cipher = crypto.createCipheriv(algorithm, Buffer.from(password), iv);
let encrypted = cipher.update('data to encrypt', 'utf8', 'hex');
encrypted += cipher.final('hex');
console.log(encrypted);
```

---

# `url`

The `url` module provides utilities for URL resolution and parsing. It's especially useful for working with web servers and handling URLs in web applications.

## Common Methods:

* **Parse a URL:**

```javascript
const url = require('url');
nst myUrl = new URL('https://example.com/path?name=JohnDoe#section');
console.log(myUrl.hostname); // 'example.com'
console.log(myUrl.pathname); // '/path'
console.log(myUrl.searchParams.get('name')); // 'JohnDoe'
```

*   
  **Resolve relative URLs:**

```javascript
const baseUrl = 'https://example.com/';
const relativeUrl = '/path';
const resolvedUrl = new URL(relativeUrl, baseUrl);
console.log(resolvedUrl.href); // 'https://example.com/path'
```

---

# `stream`

The `stream` module provides utilities for working with streams of data, such as reading from or writing to files, HTTP requests, or network connections. Streams are essential for handling large amounts of data efficiently.

## Common Methods:

* **Readable Stream Example (Reading File):**

```javascript
const fs = require('fs');
const readStream = fs.createReadStream('file.txt', 'utf8');

readStream.on('data', (chunk) => {
  console.log(chunk); // Handle the data as it is read in chunks
});
```

*   
  **Writable Stream Example (Writing to File):**

```javascript
const fs = require('fs');
const writeStream = fs.createWriteStream('output.txt');

writeStream.write('Hello, world!\n');
writeStream.end(); // Close the stream after writing
```

---

# Conclusion

Node.js's built-in modules provide a wealth of functionality that simplifies many common tasks in backend development. Whether you're working with files, HTTP requests, or cryptography, Node.js gives you powerful and efficient tools to help you build applications quickly. The modules mentioned above are just the beginning, and Node.js offers many more built-in modules to explore\!

Let me know if you'd like more details or examples of any of these modules\!

