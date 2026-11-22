---
tags:
  - FORMAT
  - HTML
---
In HTML, forms rely on specific attributes to determine how they are handled when submitted. The **`action`**, **`method`**, and **`name`** attributes are essential for defining how form data is sent and processed. Let’s break down each of these attributes:

# `action` Attribute

The **`action`** attribute of the `<form>` element specifies the **URL** to which the form data will be sent when the form is submitted. Essentially, it defines where the form’s data will go for processing after submission.

## Key Points:

* **URL or file path**: The value of the `action` attribute is usually a URL (either relative or absolute) or the name of a file on the server that will process the submitted form data.

* **If omitted**: If the `action` attribute is not specified, the form data will be submitted to the same URL as the current page.

* **Server-side script**: Often, the `action` will point to a server-side script (e.g., PHP, Python, Node.js) that processes the data, stores it in a database, sends an email, or performs some other action.

## Example:

```
<form action="submit_form.php" method="POST">
  <!-- form elements go here -->
  <button type="submit">Submit</button>
</form>
```

In this example, when the form is submitted, the data will be sent to `submit_form.php` for processing.

If the `action` is omitted:

```
<form method="POST">
  <!-- form elements go here -->
  <button type="submit">Submit</button>
</form>
```

The form data will be submitted to the current page (i.e., the page where the form is located).

# `method` Attribute

The **`method`** attribute of the `<form>` element defines the HTTP method used to send the form data to the server. There are two primary methods:

* **`GET`**: Appends the form data to the URL as a query string. This method is typically used for **non-sensitive data** and when you want to allow users to bookmark or share the form results.

* **`POST`**: Sends the form data in the body of the HTTP request. This method is more secure and is used for **sensitive data** like passwords or large data sets.

## Key Points:

* **`GET` Method**:

  * The data is appended to the URL in a query string (e.g., `?name=John&email=john@example.com`).

  * Suitable for search forms, filters, or any data that is not sensitive.

  * Has a size limit (around 2048 characters for most browsers).

* **`POST` Method**:

  * The data is sent in the body of the request, so it’s **not visible in the URL**.

  * Ideal for handling sensitive data, such as login credentials or large forms.

  * No data size limit.

## Example:

```
<form action="submit_form.php" method="GET">
  <!-- form elements go here -->
  <button type="submit">Submit</button>
</form>
```

In this example, when the form is submitted, the form data will be sent to `submit_form.php` as a query string (e.g., `submit_form.php?name=John&email=john@example.com`).

```
<form action="submit_form.php" method="POST">
  <!-- form elements go here -->
  <button type="submit">Submit</button>
</form>
```

In this case, the form data will be sent in the body of the HTTP request, so it won’t be visible in the URL.

# `name` Attribute (on Form Controls)

The **`name`** attribute is used on form controls (like `<input>`, `<select>`, `<textarea>`, etc.) to **identify the data** being sent when the form is submitted. The name you assign to each form control is the key that the server uses to retrieve the value of that field.

## Key Points:

* The **`name`** attribute is used to specify a unique identifier for each form element.

* When the form is submitted, the **name-value pairs** are sent to the server.

* **Required for form submission**: If a form control does not have a `name` attribute, its data will not be sent when the form is submitted.

## Example:

```
<form action="submit_form.php" method="POST">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" required>

  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required>

  <button type="submit">Submit</button>
</form>
```

In this example:

* When the form is submitted, the form data sent to the server will include:

  * `username=JohnDoe`

  * `password=secret123`

The **`name`** attribute is what the server-side script (e.g., `submit_form.php`) uses to access the form data. In this case, `$_POST['username']` and `$_POST['password']` would be used in PHP to retrieve the data.

# Summary of Form Attributes

| Attribute | Description |
| ----- | ----- |
| **`action`** | Specifies the URL or file to which the form data is sent when the form is submitted. |
| **`method`** | Defines the HTTP method used to submit the form (`GET` or `POST`). |
| **`name`** (on form controls) | Identifies form elements so their data can be sent to the server. |

# Example of Form with All Three Attributes:

```
<form action="process_form.php" method="POST">
  <label for="first_name">First Name:</label>
  <input type="text" id="first_name" name="first_name" required>
  
  <label for="last_name">Last Name:</label>
  <input type="text" id="last_name" name="last_name" required>
  
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>
  
  <button type="submit">Submit</button>
</form>
```

*   
  **`action="process_form.php"`**: The form data will be sent to the `process_form.php` file.

* **`method="POST"`**: The form data will be sent using the POST method, meaning it will be sent in the body of the request (not visible in the URL).

* **`name="first_name"`, `name="last_name"`, `name="email"`**: These are the keys that will be used to access the form data in the server-side script (e.g., `$_POST['first_name']`, `$_POST['last_name']`, `$_POST['email']` in PHP).

Understanding these attributes allows you to create forms that properly send data to a server for processing and ensure the data is accessible in a structured way.

