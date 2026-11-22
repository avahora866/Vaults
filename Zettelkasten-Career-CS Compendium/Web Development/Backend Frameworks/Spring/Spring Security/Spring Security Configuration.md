---
tags:
  - NOTES
  - Spring
---
# `@Configuration` class
This is a class marked by the `@Configuration` where the main security config will go
## Spring Security Password Storage
In spring security passwords are stored using a specifc format:
`{id}encodedPassword`
`{id}`: is where the password algorithm being used will be. Lets spring know what encoding algorithm to use
### Types of password algoirhtms
`{noop}`: Plain text passwords
`{bcrypt}`: Passwords hashed using the bcrypt algorithm
## InMemory Authentication and Authorisation
```java
@Configuration
public class DemoSecurityConfig {
    
    @Bean
    public InMemoryUserDetailsManager userDetailsManager() {
        UserDetails jhon = User.builder()
                .username("jhon")
                .password("{noop}test123!")
                .roles("EMPLOYEE")
                .build();

        UserDetails mary = User.builder()
                .username("mary")
                .password("{noop}test123!")
                .roles("EMPLOYEE", "MANAGER")
                .build();

        UserDetails susan = User.builder()
                .username("susan")
                .password("{noop}test123!")
                .roles("EMPLOYEE", "MANAGER", "ADMIN")
                .build();
    }
}
```

### Restricting access to endpoints to certain roles
```java
requestMatchers(HTTPMethod, "/api/employees/**")
	.hasRole("MANAGER") | .hasAnyRole("MANAGER", "ADMIN")
```
In out current configuration class add a new method
```java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.auhtoriseHttpRequests(configurer -> 
            configurer
                    .requestMarchers(HttpMethod.GET, "/api/employees").hasRole("EMPLOYEE")
                    ...
        );
        //Use HTTP basic auhtentication
        http.httpBasic(Customser.withDefaults());
		
				// disable CSRF protection
				http.csrf(csrf -> csrf.disable());	
        
       	return http.build();
    }
```
### Cross-Site Request Forgery (CSRF)
What is a CSRF attack
Spring security embeds additional data/token into all HTML forms. On subsequent requests, web app will verify token before processing.
Spring security team recommends to CSRF protection for all normal browser web requests but if you application is a [[Rest|REST API]] for non-browser client you may want to disable said protection
### Spring Data REST
When using Spring Data Rest your PUT requests requires a wildcard at the end as it puts the query params in the URL 
`"/api/employees/**"`
## JDBC Authentication and Authorisation
### Spring harcoded schema
By default Spring has a schema defined that will do all the JDBC queries to find the users and their roles for you
```
tableName: users {
	columns: {
		username: VARCHAR(50)
		password: VARCHAR(50)
		enabled: TINYINT(1)
	}
}

tablename: auhtorities {
	columns: {
		username: VARCHAR(50)
		auhtority: VARCHAR(50)
	}
	example: {
		'jhon',
		'ROLE_EMPLOYEE'
	}
}
```
Have to use these exact table names and columns
Replace the userDetailsMnaager method in your configuration class with:
```java
public UserDetailsManager userDetailsManager (DataSource dataSource) {
	return new JdbcUserDetailMnager(dataSource)
}
```
### Bcrypt Encryption
- Performs one-way encrypted hashing
- Adds a random salt to the password for additional protection
- Inc;udes support to defeat brute force attacks
All you need to do is when registering a new user to make sure to store ther password as a becrypt encoded password with the prefix of `{bcrypt}`
Another thing to change is the column length of your password in your database. It must be 68 (`{bcrypt}:8 characters, encodedPassword: 60 characters`)
#### Behind the scenes login process
1. Retrieve pass from db for the user
2. Read the encoding algorithm id `{bcypt}`
3. For the case of bcrypt, will encrypt the plaintext password givent from the login form (using lsalt form the db password)
4. Compare encrypted password from login form WITH encrypted password from db
5. Match = Login Successfule
6. No Match = Login unssuccessfull
**Password from db is never decrypted**
### Custom DB Scehema
We need to:
- tell spring how to query your custom tables
	- provide query to find user by usernname
	- provide query to find authorities / roles by username
```java
public UserDetailsManager userDetailsManager (DataSource dataSource) {
	JdbcUserDetailsManager theUserDetailsManagaer = new JdbcUserDetailsManager(dataSource);
	
	theUserDetailsManager
			.setUserByUsernameQuery("select user_id, pw. active from members where user_id=?");
	
		theUserDetailsManager
			.setAuthoritiesByUsernameQuery(query);
	
	return theUserDetailsManager
}
```