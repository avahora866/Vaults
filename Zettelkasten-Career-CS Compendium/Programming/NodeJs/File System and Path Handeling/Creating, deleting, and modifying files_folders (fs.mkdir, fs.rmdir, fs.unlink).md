---
tags:
  - FORMAT
  - NodeJs
---


Sure\! In Node.js, the `fs` module also provides methods to **create**, **delete**, and **modify** files and directories (folders). Here's how the commonly used ones work:

---

# 📁 Creating a Folder – `fs.mkdir`

Creates a **new directory** (folder).

## Syntax:

```javascript
const fs = require('fs');

fs.mkdir('folderName', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('Folder created');
});
```

*   
  If the folder already exists, you'll get an error.

* To create nested folders, use `{ recursive: true }`.

## Example:

```javascript
fs.mkdir('myFolder', { recursive: true }, (err) => {
  if (err) throw err;
  console.log('Folder created');
});
```

---

# 🗑️ Deleting a Folder – `fs.rmdir` (Deprecated)

`fs.rmdir` is deprecated. Use `fs.rm` instead with `{ recursive: true }` if you want to delete a folder that has contents.

## Using `fs.rm`:

```javascript
fs.rm('myFolder', { recursive: true, force: true }, (err) => {
  if (err) throw err;
  console.log('Folder deleted');
});
```

---

# 🗑️ Deleting a File – `fs.unlink`

Deletes a file (not a folder).

## Syntax:

```javascript
fs.unlink('file.txt', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File deleted');
});
```

---

# ✏️ Modifying Files

You can modify a file's content using:

## `fs.writeFile` (overwrites):

```javascript
fs.writeFile('file.txt', 'New content', (err) => {
  if (err) throw err;
  console.log('File modified (overwritten)');
});
```

## `fs.appendFile` (adds to end):

```javascript
fs.appendFile('file.txt', '\nMore content', (err) => {
  if (err) throw err;
  console.log('Content appended');
});
```

---

# Summary Table:

| Operation | Method | Notes |
| ----- | ----- | ----- |
| Create folder | `fs.mkdir` | Use `{ recursive: true }` for nested |
| Delete folder | `fs.rm` | Use `{ recursive: true }` |
| Delete file | `fs.unlink` | Deletes a single file |
| Modify file (overwrite) | `fs.writeFile` | Replaces existing content |
| Modify file (append) | `fs.appendFile` | Adds content to the end |

Would you like async/await versions of these too?

