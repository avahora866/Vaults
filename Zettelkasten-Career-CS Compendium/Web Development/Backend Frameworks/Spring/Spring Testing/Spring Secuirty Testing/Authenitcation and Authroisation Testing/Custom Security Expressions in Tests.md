---
tags:
  - FORMAT
  - Spring
---
# Custom Security Expressions in Tests

In Spring Security, **custom security expressions** allow you to define and use your own expressions to control access to resources based on more complex security rules. By default, Spring Security provides built-in expressions like `hasRole()`, `hasAuthority()`, `isAuthenticated()`, etc. However, there may be cases where the built-in expressions are not sufficient, and you need more fine-grained control over access to certain resources.

Custom security expressions enable you to define new expressions that can be evaluated in the context of Spring Security's expression-based access control mechanisms, such as `@PreAuthorize` and `@Secured`. These expressions can be particularly useful for more complex authorization logic.

In this explanation, we'll look at how to create and use custom security expressions in tests, leveraging Spring Security's powerful expression-based access control.

## What Are Security Expressions?

Security expressions in Spring Security are part of its authorization mechanism. They are used in annotations like `@PreAuthorize` or `@Secured` to define rules for method access, URL access, or resource access based on conditions like roles, authorities, or any other security-related logic.

For example:

* `@PreAuthorize("hasRole('ADMIN')")` restricts access to a method to users with the `ADMIN` role.

* `@Secured("ROLE_USER")` ensures the method is only accessible to users with the `ROLE_USER` authority.

However, these expressions can be extended to support custom logic based on application-specific needs.

## Creating Custom Security Expressions

To create custom security expressions, you need to define a custom `SecurityExpressionHandler` and implement an `@PreAuthorize` or `@Secured` expression with your custom logic.

## Steps for Implementing Custom Security Expressions:

### 1 Define a Custom Expression Handler

The first step is to define a custom expression handler that integrates your custom security expressions. Spring Security uses the `ExpressionHandler` to evaluate security expressions, and you can extend this handler to support your own custom logic.

For example, you might want to create an expression that checks if a user is the owner of a specific resource.

```java
@Component
public class CustomSecurityExpressionHandler extends DefaultMethodSecurityExpressionHandler {

    private final AuthenticationService authenticationService;

    @Autowired
    public CustomSecurityExpressionHandler(AuthenticationService authenticationService) {
        this.authenticationService = authenticationService;
    }

    @Override
    public ExpressionParser getExpressionParser() {
        ExpressionParser expressionParser = super.getExpressionParser();
        expressionParser.registerFunction("isOwner", new IsOwnerFunction(authenticationService));
        return expressionParser;
    }
}
```

In this example:

* `CustomSecurityExpressionHandler` extends `DefaultMethodSecurityExpressionHandler` and overrides the `getExpressionParser()` method to register a custom function.

* The custom function `isOwner` is implemented by `IsOwnerFunction`, which checks whether the current user is the owner of a resource.

### 2 Define the Custom Expression Function

Next, you create a custom function that will be invoked in the security expression. This function should implement `ExpressionExecutor`.

```java
public class IsOwnerFunction implements ExpressionFunction {

    private final AuthenticationService authenticationService;

    public IsOwnerFunction(AuthenticationService authenticationService) {
        this.authenticationService = authenticationService;
    }

    public boolean isOwner(String resourceId) {
        return authenticationService.isCurrentUserOwnerOf(resourceId);
    }
}
```

In this case:

* `isOwner` checks if the currently authenticated user is the owner of a resource.

* You would use `authenticationService.isCurrentUserOwnerOf(resourceId)` to determine if the user has the correct ownership over the resource.

### 3 Register the Custom Expression Handler

Once you have defined your custom expression handler and function, you need to tell Spring Security to use your custom handler. You can do this by configuring it in the security configuration.

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private CustomSecurityExpressionHandler customSecurityExpressionHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .expressionHandler(customSecurityExpressionHandler) // Register the custom handler
            .antMatchers("/admin/**").hasRole("ADMIN")
           .antMatchers("/owner/").access("@isOwner(#resourceId)") // Custom expression
            .anyRequest().authenticated();
    }
}
```

In this case:

* The custom expression handler is registered in the `HttpSecurity` configuration.

The `/owner/` endpoint uses the custom expression `@isOwner(#resourceId)` to determine access based on whether the user is the owner of the resource.

## Using Custom Security Expressions in Tests

Once your custom security expressions are set up, you can use them in your tests. For testing, Spring Security allows you to use mock authentication contexts to simulate different users with specific roles or authorities. You can also use custom expressions in annotations like `@PreAuthorize`.

### 1 Testing Custom Security Expressions in Tests

o test custom expressions like `@isOwner(#resourceId)`, you can use `@WithMockUser` or `@WithUserDetails` to simulate users and perform tests on endpoints secured by your custom expressions.

```java
@RunWith(SpringRunner.class)
@WebMvcTest(MyController.class)
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(username = "user1", roles = "USER")
    public void testOwnerAccess() throws Exception {
        String resourceId = "123"; // Assume resourceId is passed to the URL or method
        
        mockMvc.perform(get("/owner/" + resourceId))
               .andExpect(status().isOk()) // Assuming user1 is the owner of resourceId 123
               .andExpect(content().string("Access granted to owner"));
    }

    @Test
    @WithMockUser(username = "user2", roles = "USER")
    public void testNonOwnerAccess() throws Exception {
        String resourceId = "123"; // Assuming user2 is not the owner of resourceId 123
        
        mockMvc.perform(get("/owner/" + resourceId))
               .andExpect(status().isForbidden()); // Access should be denied for non-owners
    }
}
```

### 2 Test Scenario Explanation

* **First Test (`testOwnerAccess`)**:

  * The test simulates `user1` accessing the `/owner/{resourceId}` endpoint.

  The custom expression `@isOwner(#resourceId)` should return `true` if `user1` is the owner of the resource with ID `123`, allowing access to the endpoint.

* **Second Test (`testNonOwnerAccess`)**:

  * The test simulates `user2` accessing the same endpoint.

  Since `user2` is not the owner of the resource, the custom expression `@isOwner(#resourceId)` should return `false`, and access to the endpoint should be denied (resulting in a `403 Forbidden` status).

## Integrating Custom Expressions with `@PreAuthorize`

Custom security expressions can also be used in the `@PreAuthorize` annotation, which is often used to restrict access to methods based on security expressions. Here's how you can use custom expressions directly in controller methods.

### Example with `@PreAuthorize`:

```java
@RestController
public class MyController {

    @GetMapping("/owner/{resourceId}")
   @PreAuthorize("@isOwner(#resourceId)") // Using custom security expression
    public String viewOwnerPage(@PathVariable String resourceId) {
        return "Access granted to owner";
    }
}
```

## Conclusion

Custom security expressions in Spring Security provide a powerful mechanism for implementing complex, fine-grained access control. By defining your own expressions, you can tailor access rules to fit your specific application logic.

* **Define custom expressions**: Extend `SecurityExpressionHandler` and implement custom logic for expression evaluation.

* **Register custom expressions**: Configure the expression handler in your Spring Security configuration.

* **Test custom expressions**: Use `@WithMockUser` or `@WithUserDetails` to simulate different users and check how custom expressions behave in your tests.

This approach ensures that your security logic can be as flexible as needed to meet the specific needs of your application, while also providing a clean and testable way to define access control rules.

