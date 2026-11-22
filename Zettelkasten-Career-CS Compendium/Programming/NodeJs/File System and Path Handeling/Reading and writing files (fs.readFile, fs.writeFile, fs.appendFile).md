---
tags:
  - FORMAT
  - NodeJs
---
Absolutely\! In Node.js, file operations such as reading, writing, and appending data to files are handled using the built-in `fs` (File System) module. Here's a breakdown of how the main functions work:

---

# Reading Files – `fs.readFile`

This function is used to **read the contents of a file asynchronously**.

## Syntax:

```javascript
const fs = require('fs');

fs.readFile('filename.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

*   
  `'filename.txt'`: Path to the file.

* `'utf8'`: Encoding (optional, but recommended to get a readable string).

* `callback`: Function with `err` and `data` as arguments.

## Example:

```javascript
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('File contents:', data);
});
```

---

# Writing Files – `fs.writeFile`

This function **writes data to a file**, replacing the file if it already exists.

## Syntax:

```javascript
fs.writeFile('filename.txt', 'Content to write', 'utf8', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File written successfully');
});
```

*   
  If the file does **not exist**, it will be **created**.

* If it **does exist**, it will be **overwritten**.

## Example:

```javascript
fs.writeFile('output.txt', 'Hello, Node.js!', 'utf8', (err) => {
  if (err) throw err;
  console.log('Content written to file');
});
```

---

# Appending to Files – `fs.appendFile`

This function **adds data to the end of a file**. If the file doesn’t exist, it creates it.

## Syntax:

```javascript
fs.appendFile('filename.txt', 'Data to append', 'utf8', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('Data appended');
});
```

## Example:

```javascript
fs.appendFile('log.txt', 'New log entry\n', 'utf8', (err) => {
  if (err) throw err;
  console.log('Log updated');
});
```

---

# Summary Table:

| Method | Purpose | Overwrites? | Creates File if Missing? |
| ----- | ----- | ----- | ----- |
| `fs.readFile` | Read contents of a file | N/A | No |
| `fs.writeFile` | Write (overwrite) contents | Yes | Yes |
| `fs.appendFile` | Append data to existing file | No | Yes |

Let me know if you’d like the synchronous versions or examples with `async/await` using `fs.promises`.

