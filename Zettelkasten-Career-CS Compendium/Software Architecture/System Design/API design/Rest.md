---
tags:
  - APIDesign
---
REST (**Representational State Transfer**) is an **architectural style** used for designing web APIs that communicate over HTTP. It provides a structured way for clients (like web browsers or mobile apps) and servers to interact using **standard HTTP methods**:

- **GET** → Retrieve data (e.g., `GET /users/1` to fetch user with ID 1).
- **POST** → Create a new resource (e.g., `POST /users` to create a new user).
- **PUT** → Update an existing resource (e.g., `PUT /users/1` to update user 1).
- **DELETE** → Remove a resource (e.g., `DELETE /users/1` to delete user 1).

Each **resource** (like users, products, or orders) is represented by a URL (`/users`, `/products`) and data is typically exchanged in **JSON** or **XML** format.

# Rules for an API to be RESTful

For an API to be **RESTful**, it must follow six key constraints:

## Client-Server Architecture

- The client (frontend) and server (backend) must be separate.
- This allows them to be developed and scaled independently.

## Statelessness

- Each request from the client must include **all necessary information** for the server to process it.
- The server **does not** store session data between requests.
- Authentication should be handled using **tokens** (e.g., JWT, API keys).

## Cacheability

- Responses should indicate if they **can be cached** (e.g., using `Cache-Control` headers).
- Caching improves performance and reduces server load.

## Uniform Interface

- The API must have a consistent structure with:
    - **Resource-based URLs** (e.g., `/users` instead of `/getUsers`).
    - **Standard HTTP Methods** (GET, POST, PUT, DELETE).
    - **Standardized Responses** (e.g., `200 OK`, `404 Not Found`).
    - **HATEOAS (Hypermedia as the Engine of Application State)** (Optional but recommended) – API responses include links to related resources.

## Layered System

- The API can have multiple layers (e.g., caching, authentication, security), but clients should only interact with the API and not the underlying layers.

## Code on Demand (Optional)

- The server **can send executable code** (e.g., JavaScript) to the client to extend functionality.
- Rarely used in REST APIs.

## Additional Best Practices (Not Mandatory, But Recommended)

- Use **JSON** as the default data format.
- Implement **versioning** (e.g., `/api/v1/users`).
- Support **pagination** for large datasets.
- Use **meaningful error messages** (e.g., `{ "error": "User not found" }`).

---

# Why is REST Stateless?

REST is **stateless**, meaning the server does **not store session data** between requests. Each request must contain all necessary information.

## Example of Stateless Authentication (JWT Token)

1. **User Logs In** and gets a token:
    
    ```
    POST /login  
    Body: { "username": "user1", "password": "mypassword" }
    ```
    
    Response:
    
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
    ```
    
    - The client stores the token (e.g., in local storage).
2. **Client Sends Requests with Token**
    
    ```
    GET /profile  
    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
    ```
    
    - The server validates the token and processes the request.
    - No session data is stored on the server.
3. **If the token is invalid/expired**, the server responds with `401 Unauthorized`.
    

This makes REST APIs **scalable** because any server can handle requests without needing to track user sessions.

---

## How Stateful Authentication Works (Non-RESTful)

In a **stateful** system, the server remembers the user’s session:

1. **User Logs In**
    
    ```
    POST /login  
    Body: { "username": "user1", "password": "mypassword" }
    ```
    
    Response:
    
    ```json
    {
      "sessionId": "abc123xyz"
    }
    ```
    
    - The server stores the session ID in memory or a database.
2. **Client Makes Authenticated Requests**
    
    ```
    GET /profile  
    Cookie: sessionId=abc123xyz
    ```
    
    - The server looks up the session ID and identifies the user.
3. **If the session expires**, the user must log in again.
    

Stateful authentication is common in **monolithic** applications, while **stateless** authentication is preferred for **modern REST APIs and microservices**.

---

# REST vs. RESTful: Is There a Difference?

**REST** is the **architectural style**, while **RESTful** refers to an **API that follows REST principles**. If an API follows the six constraints of REST, it is **RESTful**.