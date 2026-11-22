---
tags:
  - FORMAT
  - NodeJs
---
Sure\! Streams in Node.js are a way to **handle data piece by piece**, rather than all at once. This is especially useful for large files or data flows like reading from files, network operations, or even processing incoming HTTP requests.

There are four types of streams in Node.js:

---

# Readable Streams

Used to **read** data from a source.

## Examples:

* Reading a file

* Receiving data over HTTP

## Key methods/events:

* `.on('data', chunk)` – triggered when data is available

* `.on('end')` – triggered when all data is read

## Example:

```javascript
const fs = require('fs');
const readable = fs.createReadStream('file.txt', 'utf8');

readable.on('data', chunk => {
  console.log('Received:', chunk);
});

readable.on('end', () => {
  console.log('No more data.');
});
```

---

# Writable Streams

Used to **write** data to a destination.

## Examples:

* Writing to a file

* Sending data over a network

## Key methods:

* `.write(chunk)` – write a chunk of data

* `.end()` – signal that no more data will be written

## Example:

```javascript
const fs = require('fs');
const writable = fs.createWriteStream('output.txt');

writable.write('Hello, ');
writable.write('world!');
writable.end(); // done writing
```

---

# Duplex Streams

These are both **readable and writable**.

## Examples:

* A TCP socket

* WebSocket connections

You can read and write data on the same stream.

## Example:

```javascript
const { Duplex } = require('stream');

const duplex = new Duplex({
  read(size) {
    this.push('Data from read side');
    this.push(null); // no more data
  },
  write(chunk, encoding, callback) {
    console.log('Written:', chunk.toString());
    callback();
  }
});

duplex.write('Writing to duplex');
duplex.on('data', chunk => console.log('Read from duplex:', chunk.toString()));
```

---

# Transform Streams

A special kind of Duplex stream that **modifies the data** as it is written and read.

## Example:

* Compressing data

* Encrypting/decrypting

* Converting to uppercase

## Example:

```javascript
const { Transform } = require('stream');

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```

This example reads input from the terminal, transforms it to uppercase, and outputs it.

---

# Summary

| Stream Type | Can Read | Can Write | Purpose |
| ----- | ----- | ----- | ----- |
| Readable | Yes | No | Read data from a source |
| Writable | No | Yes | Write data to a destination |
| Duplex | Yes | Yes | Read and write (no transform) |
| Transform | Yes | Yes | Modify data as it passes through |

Let me know if you want an example involving streaming files with piping or creating custom transform logic.

