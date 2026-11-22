---
tags:
  - Python
  - FORMAT
---
# Working with JSON & APIs

## Parsing JSON Data

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write. In Python, you can use the `json` module to parse and work with JSON data.

### Example: Parsing JSON Data

Suppose you have a JSON string that represents a dictionary.

```py
import json

# Sample JSON string
json_string = '{"name": "Alice", "age": 25, "city": "New York"}'

# Parse JSON string into a Python dictionary
data = json.loads(json_string)

# Accessing the values
print(data["name"])  # Output: Alice
print(data["age"])   # Output: 25
print(data["city"])  # Output: New York
```

### Converting Python Objects to JSON

You can also convert Python dictionaries to JSON format using `json.dumps()`.

```py
# Python dictionary
person = {"name": "Bob", "age": 30, "city": "Los Angeles"}

# Convert to JSON string
json_data = json.dumps(person)

print(json_data)
# Output: {"name": "Bob", "age": 30, "city": "Los Angeles"}
```

### Formatting JSON Output (Pretty Print)

You can use the `indent` parameter to pretty-print the JSON data.

```py
formatted_json = json.dumps(person, indent=4)
print(formatted_json)
```

Output:

```
{
    "name": "Bob",
    "age": 30,
    "city": "Los Angeles"
}
```

---

## Fetching Data from APIs with `requests`

The `requests` library in Python is used for making HTTP requests to interact with APIs.

### Installing the `requests` Library

If you don't have the `requests` library installed, you can install it via pip:

```
pip install requests
```

### Example: Fetching Data from an API

To interact with an API, you can send GET, POST, and other types of HTTP requests. Here’s an example of using `GET` to fetch data from an API.

```py
import requests

# Example API endpoint (JSONPlaceholder is a free API for testing)
url = "https://jsonplaceholder.typicode.com/users"

# Send GET request to fetch data from the API
response = requests.get(url)

# Check if the request was successful
if response.status_code == 200:
    data = response.json()  # Parse the JSON response into a Python dictionary
    print(data)
else:
    print(f"Failed to retrieve data. Status code: {response.status_code}")
```

### Example: Using Parameters in API Requests

Sometimes, you need to pass parameters in your API request (e.g., filters or search queries).

```py
# Example API endpoint with query parameters
url = "https://jsonplaceholder.typicode.com/posts"

# Parameters to be sent with the GET request
params = {"userId": 1}

# Send GET request with parameters
response = requests.get(url, params=params)

if response.status_code == 200:
    data = response.json()
    print(data)  # Posts for user with userId = 1
else:
    print(f"Failed to retrieve data. Status code: {response.status_code}")
```

### Example: Sending Data with a POST Request

In addition to `GET`, you can also use `POST` to send data to an API.

```py
url = "https://jsonplaceholder.typicode.com/posts"

# Data to be sent to the API
data = {
    "title": "foo",
    "body": "bar",
    "userId": 1
}

# Send POST request
response = requests.post(url, json=data)

if response.status_code == 201:
    print(f"Data successfully created. Response: {response.json()}")
else:
    print(f"Failed to send data. Status code: {response.status_code}")
```

---

## Handling Errors and Timeouts

You should handle potential errors (such as network issues) and timeouts in your requests.

```py
try:
    response = requests.get("https://jsonplaceholder.typicode.com/users", timeout=5)
    response.raise_for_status()  # Raise HTTPError for bad responses (4xx or 5xx)
    data = response.json()
    print(data)
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

---

## Summary

| Concept | Example |
| ----- | ----- |
| **Parsing JSON** | `json.loads(json_string)` |
| **Converting to JSON** | `json.dumps(dict)` |
| **Pretty Printing JSON** | `json.dumps(dict, indent=4)` |
| **Fetching Data from API (GET)** | `response = requests.get(url)` |
| **Passing Parameters in GET** | `requests.get(url, params={"key": "value"})` |
| **Sending Data with POST** | `requests.post(url, json=data)` |
| **Error Handling** | `response.raise_for_status()` and exception handling |

