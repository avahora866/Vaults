---
tags:
  - FORMAT
  - Spring
---
**JDBC Authentication** in Spring Security is a method where user credentials (such as username, password) and roles (authorities) are stored in a relational database. Spring Security manages this authentication process using JDBC and typically requires setting up a specific database schema for storing user data. This method provides a more scalable and persistent solution compared to in-memory authentication and is suitable for production environments where user data needs to be stored in a database.

---

# How JDBC Authentication Works

In JDBC authentication, Spring Security authenticates users by querying a relational database to retrieve user credentials (like passwords) and associated roles or authorities. This is typically done using SQL queries configured in the Spring Security configuration.

## Key Components:

* **JDBC Authentication** relies on the `JdbcUserDetailsService` or a custom `UserDetailsService` that interacts with the database to retrieve user details.  
* The authentication data, such as users and roles, is typically stored in tables (such as `users`, `roles`, or `authorities`) within the database.

---

# Database Schema for JDBC Authentication

Spring Security provides a default schema that you can use for storing user credentials, authorities, and roles. The default schema typically includes three tables:

1. **`users`**: Stores basic user information (username, password).  
2. **`authorities`**: Stores the roles/authorities associated with each user.  
3. **`user_authority`**: Links users and their corresponding roles/authorities.

Here’s an example of how these tables might look:

## Example Schema:

```
-- Users table
CREATE TABLE users (
    username VARCHAR(50) NOT NULL PRIMARY KEY,
    password VARCHAR(255) NOT NULL,
    enabled BOOLEAN NOT NULL
);

-- Authorities table (roles)
CREATE TABLE authorities (
    username VARCHAR(50) NOT NULL,
    authority VARCHAR(50) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username)
);

-- User-Authority table to link users with authorities
-- (optional if you use a single 'authorities' table)
CREATE TABLE user_authority (
    username VARCHAR(50) NOT NULL,
    authority VARCHAR(50) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username)
);
```

* The **`users`** table holds the username, password (usually encoded), and whether the account is enabled.  
* The **`authorities`** table holds roles or permissions that are granted to users (e.g., `ROLE_ADMIN`, `ROLE_USER`).  
* **`user_authority`** can be used to link users with their authorities if they are stored separately.

---

# Configuring JDBC Authentication in Spring Security

## Step-by-Step Setup:

1. **Configure DataSource**: You need a `DataSource` bean that connects to your relational database (e.g., MySQL, PostgreSQL).

2. **Define JdbcUserDetailsService**: This is the class responsible for retrieving user details (username, password, roles) from the database.

3. **Configure Spring Security to Use JDBC Authentication**: Set up the HTTP security and authentication manager to use JDBC for authentication.

## Example Configuration:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import javax.sql.DataSource;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public UserDetailsService userDetailsService(DataSource dataSource) {
        JdbcDaoImpl jdbcDao = new JdbcDaoImpl();
        jdbcDao.setDataSource(dataSource);  // Set the data source
        jdbcDao.setUsersByUsernameQuery("SELECT username, password, enabled FROM users WHERE username = ?");
        jdbcDao.setAuthoritiesByUsernameQuery("SELECT username, authority FROM authorities WHERE username = ?");
        return jdbcDao;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();  // Using BCrypt to encode passwords
    }

    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin()
                .permitAll();
    }
}
```

---

## Key Points:

1. **`JdbcDaoImpl`**: This class is used for implementing JDBC-based authentication. You need to configure SQL queries to retrieve users and their authorities from the database.

2. **SQL Queries**:

   * `setUsersByUsernameQuery()`: This query retrieves user details (username, password, and account status).  
   * `setAuthoritiesByUsernameQuery()`: This query retrieves the roles or authorities associated with the user.  
3. **Password Encoding**: Use a `PasswordEncoder` (e.g., `BCryptPasswordEncoder`) to encode passwords securely when storing and comparing them in the database. **Do not store plain text passwords**.

4. **Database Integration**: The database tables should store the data securely. The user's password must be encrypted, and the role/authority data should be consistent with the application’s security model.

---

# Advantages of JDBC Authentication

## Persistence

* Unlike in-memory authentication, JDBC authentication stores user credentials and roles in a database, meaning the data persists even if the application is restarted.

## Scalable

* Suitable for larger applications where user data needs to be managed across sessions or when you have many users.

## Centralized Authentication

* You can store user credentials in a centralized database, making it easier to manage users across different parts of the system or even different applications.

## Customizable Queries

* You can customize the SQL queries used for retrieving user details and roles. This allows for greater flexibility, such as integrating with existing user management systems.

---

# Limitations of JDBC Authentication

## Database Dependency

* This method introduces a dependency on a relational database. If your database goes down, users won’t be able to authenticate until the database is back online.

## Security Considerations

* Storing user credentials in a database requires careful handling of sensitive data. It's critical to use proper password encoding (e.g., BCrypt) and secure database connections.

## Complexity

* More configuration is required compared to in-memory authentication, especially for managing the database schema, encoding passwords, and ensuring correct queries are used.

---

# Example Use Case

**JDBC Authentication** is ideal when:

* You have a larger application with a persistent user base.  
* User credentials and roles need to be stored in a database (e.g., MySQL, PostgreSQL).  
* You want to integrate Spring Security with an existing relational database and manage user access centrally.

---

# Conclusion

**JDBC Authentication** in Spring Security is a powerful method for storing user credentials and roles in a relational database, making it ideal for scalable and persistent user management. While it requires setting up a database schema and configuring SQL queries, it offers flexibility and scalability compared to simpler authentication methods like in-memory authentication. It is suitable for production environments where security and persistence are important.

