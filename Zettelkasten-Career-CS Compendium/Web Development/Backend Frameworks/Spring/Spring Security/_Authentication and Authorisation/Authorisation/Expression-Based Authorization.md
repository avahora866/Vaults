---
tags:
  - FORMAT
  - Spring
---
**Expression-based authorization** in Spring Security allows for flexible, dynamic access control by using **Spring Expression Language (SpEL)**. This feature lets you define security rules in a declarative way, making it easy to create complex and dynamic authorization logic based on various security attributes such as roles, authorities, or even the state of the application.

With Spring Security, you can apply expressions in several places, such as method-level security (using `@PreAuthorize`, `@Secured`), HTTP security (URL-based authorization), and object-level security.

---

# Key Concepts of Expression-Based Authorization

## Spring Expression Language (SpEL)

* SpEL is a powerful expression language that allows you to query and manipulate application context, variables, and properties.  
* It can access various objects, such as authentication details, user roles, authorities, and more, to make dynamic authorization decisions.

## Security Expressions

Spring Security offers predefined expressions that you can use for authorization. These expressions are evaluated at runtime, and they control whether a user can access a particular resource or method.

---

# Common Security Expressions

## `hasRole()`

Checks if the authenticated user has a particular role. In Spring Security, roles are prefixed with `"ROLE_"`.

### Example:

```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long userId) {
    System.out.println("Deleting user with ID: " + userId);
}
```

* **`hasRole('ADMIN')`**: This expression checks whether the user has `ROLE_ADMIN`.

## `hasAuthority()`

Checks if the authenticated user has a specific authority (i.e., permission). Unlike roles, authorities don't require the `"ROLE_"` prefix.

### Example:

```java
@PreAuthorize("hasAuthority('WRITE_PRIVILEGES')")
public void updateDocument(Long documentId, String content) {
    System.out.println("Updating document with ID: " + documentId);
}
```

* **`hasAuthority('WRITE_PRIVILEGES')`**: This expression checks if the user has the `WRITE_PRIVILEGES` authority.

## `hasAnyRole()`

Checks if the authenticated user has any of the specified roles. This expression allows multiple roles to be evaluated.

### Example:

```java
@PreAuthorize("hasAnyRole('USER', 'ADMIN')")
public void viewContent() {
    System.out.println("Accessing content...");
}
```

* **`hasAnyRole('USER', 'ADMIN')`**: This expression checks if the user has either `ROLE_USER` or `ROLE_ADMIN`.

## `hasAnyAuthority()`

Checks if the authenticated user has any of the specified authorities. Similar to `hasAnyRole()`, but for authorities instead of roles.

### Example:

```java
@PreAuthorize("hasAnyAuthority('READ_PRIVILEGES', 'WRITE_PRIVILEGES')")
public void accessDocument() {
    System.out.println("Accessing document...");
}
```

* **`hasAnyAuthority('READ_PRIVILEGES', 'WRITE_PRIVILEGES')`**: This expression allows users with either `READ_PRIVILEGES` or `WRITE_PRIVILEGES` to access the resource.

## `permitAll()`

Allows access to everyone, including unauthenticated users.

### Example:

```java
http.authorizeHttpRequests()
    .requestMatchers("/public/**").permitAll()
    .anyRequest().authenticated();
```

* **`permitAll()`**: Grants access to the specified URL pattern for all users, regardless of authentication or roles.

## `denyAll()`

Denies access to everyone, including authenticated users.

### Example:

```java
http.authorizeHttpRequests()
    .requestMatchers("/restricted/**").denyAll()
    .anyRequest().authenticated();
```

* **`denyAll()`**: Denies access to the `/restricted/**` endpoint for all users, whether authenticated or not.

## `authenticated()`

Ensures that the user is authenticated before accessing the resource.

### Example:

```java
http.authorizeHttpRequests()
    .requestMatchers("/user/**").authenticated()
    .anyRequest().permitAll();
```

* **`authenticated()`**: Requires that the user be logged in before accessing the `/user/**` resource.

## `isAnonymous()`

Checks if the user is not authenticated (i.e., an anonymous user).

### Example:

```java
http.authorizeHttpRequests()
    .requestMatchers("/login").isAnonymous()
    .anyRequest().authenticated();
```

* **`isAnonymous()`**: This expression ensures that only anonymous users (those not authenticated) can access the `/login` URL.

## `isAuthenticated()`

Checks if the user is authenticated, meaning they have a valid session.

### Example:

```java
@PreAuthorize("isAuthenticated()")
public void changePassword(String newPassword) {
    System.out.println("Changing password...");
}
```

* **`isAuthenticated()`**: Ensures that only authenticated users can execute the `changePassword` method.

---

# Advanced Expression Usage

## Method Arguments in Expressions

ou can use method arguments in your security expressions by referencing them with the `#` symbol.

### Example:

```java
PreAuthorize("hasPermission(#documentId, 'DOCUMENT', 'READ')")
public void readDocument(Long documentId) {
    System.out.println("Reading document with ID: " + documentId);
}
```

`#documentId`: This expression uses the method argument (`documentId`) to check if the user has permission to read the document with the given ID.

## Using `@PreAuthorize` for Complex Logic

You can create more complex authorization logic by combining expressions with logical operators like `and`, `or`, and `not`.

### Example:

```java
@PreAuthorize("hasRole('ADMIN') and hasAuthority('WRITE_PRIVILEGES')")
public void manageData() {
    System.out.println("Managing data...");
}
```

* **`and`**: Both conditions must be true (the user must have `ROLE_ADMIN` and the authority `WRITE_PRIVILEGES`).

```java
@PreAuthorize("hasRole('USER') or hasAuthority('ACCESS_DATA')")
public void viewData() {
    System.out.println("Viewing data...");
}
```

* **`or`**: The user needs to satisfy either of the conditions (having `ROLE_USER` or `ACCESS_DATA` authority).

```java
@PreAuthorize("not(hasRole('GUEST'))")
public void performAction() {
    System.out.println("Action performed.");
}
```

* **`not`**: The user must **not** have the specified role or authority.

---

# Expression-Based Authorization in HTTP Security

You can also use Spring Security's expressions in HTTP request authorization rules.

## Example:

```java
http.authorizeHttpRequests()
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .requestMatchers("/reports/**").hasAuthority("READ_PRIVILEGES")
    .requestMatchers("/user/**").authenticated()
    .anyRequest().permitAll();
```

* **`hasRole('ADMIN')`**: Limits access to the `/admin/**` URL to users with `ROLE_ADMIN`.  
* **`hasAuthority('READ_PRIVILEGES')`**: Allows access to the `/reports/**` URL for users with `READ_PRIVILEGES`.  
* **`authenticated()`**: Ensures that the user is authenticated before accessing `/user/**`.  
* **`permitAll()`**: Grants access to all other URLs without restriction.

---

# Conclusion

**Expression-based authorization** in Spring Security provides flexible and dynamic access control by using **SpEL (Spring Expression Language)**. The key advantages include:

* **Granular Control**: Apply complex authorization logic at both the HTTP request level and method level.  
* **Predefined Expressions**: Use built-in expressions like `hasRole()`, `hasAuthority()`, and `permitAll()`.  
* **Flexible Logic**: Combine expressions with logical operators (`and`, `or`, `not`) to create complex security rules.  
* **Method Argument Access**: Reference method arguments in expressions for fine-grained control over individual method invocations.

By leveraging these expressions, you can define robust security rules that are both easy to maintain and adapt to complex use cases.

