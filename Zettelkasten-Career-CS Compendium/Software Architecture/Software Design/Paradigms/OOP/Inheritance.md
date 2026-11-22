---
tags:
  - OOP
  - "#SECONDARY_BRANCH"
---
Wouldn’t it be nice if we could reuse code or extend it for more specific use cases? That’s what inheritance is about.

To go about code reuse, in TDD, we have this technique called the Rule of Three which specifies that upon seeing duplication three times, we should refactor it into an abstraction.

The classic example I use to demonstrate is one where we rely on several different API endpoints from a front-end application but we duplicate the fetching logic across each API adapter (think `/users`, `/forum`, `/billing`, etc.).

Instead, what we could do is refactor the data-fetching logic into a commonplace. The [[Abstract Classes and Interfaces|abstract class]] is a good tool for this use case.

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.Map;

public abstract class API {

    protected String baseUrl;
    private HttpClient httpClient;

    public API(String baseUrl) {
        this.baseUrl = baseUrl;
        this.httpClient = HttpClient.newHttpClient();
        enableInterceptors();
    }

    private void enableInterceptors() {
        // Here you can define common retry logic or other interceptors if needed
    }

    // Common GET logic
    protected HttpResponse<String> get(String url, Map<String, String> headers, Map<String, String> params) throws IOException, InterruptedException {
        String fullUrl = baseUrl + "/" + url;
        if (params != null && !params.isEmpty()) {
            StringBuilder paramString = new StringBuilder("?");
            for (Map.Entry<String, String> entry : params.entrySet()) {
                paramString.append(entry.getKey()).append("=").append(entry.getValue()).append("&");
            }
            paramString.deleteCharAt(paramString.length() - 1); // remove trailing '&'
            fullUrl += paramString.toString();
        }

        HttpRequest.Builder requestBuilder = HttpRequest.newBuilder()
                .uri(URI.create(fullUrl))
                .GET();

        if (headers != null) {
            headers.forEach(requestBuilder::header);
        }

        HttpRequest request = requestBuilder.build();
        return httpClient.send(request, HttpResponse.BodyHandlers.ofString());
    }

    // Common POST logic
    protected HttpResponse<String> post(String url, String body, Map<String, String> headers) throws IOException, InterruptedException {
        HttpRequest.Builder requestBuilder = HttpRequest.newBuilder()
                .uri(URI.create(baseUrl + "/" + url))
                .POST(HttpRequest.BodyPublishers.ofString(body != null ? body : ""));

        if (headers != null) {
            headers.forEach(requestBuilder::header);
        }

        HttpRequest request = requestBuilder.build();
        return httpClient.send(request, HttpResponse.BodyHandlers.ofString());
    }
}

```

And then, for each API adapter we need, we can merely subclass the base API abstract class to get access to the common functionality (`public` or `protected` methods and properties).

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.IOException;
import java.net.http.HttpResponse;
import java.util.Map;

// Users API
public class UsersAPI extends API {

    private ObjectMapper objectMapper = new ObjectMapper();

    public UsersAPI() {
        super("http://example.com/users");
    }

    public Users getUsers() throws IOException, InterruptedException {
        HttpResponse<String> response = this.get("/", null, null); // GET request using base class
        return objectMapper.readTree(response.body())
                           .path("users")
                           .traverse(objectMapper)
                           .readValueAs(Users.class);
    }
}

// Forum API
public class ForumAPI extends API {

    private ObjectMapper objectMapper = new ObjectMapper();

    public ForumAPI() {
        super("http://example.com/forum");
    }

    public Posts getPosts() throws IOException, InterruptedException {
        HttpResponse<String> response = this.get("/", null, null);
        return objectMapper.readTree(response.body())
                           .path("posts")
                           .traverse(objectMapper)
                           .readValueAs(Posts.class);
    }
}

// Billing API
public class BillingAPI extends API {

    private ObjectMapper objectMapper = new ObjectMapper();

    public BillingAPI() {
        super("http://example.com/billing");
    }

    public Payments getPayments() throws IOException, InterruptedException {
        HttpResponse<String> response = this.get("/", null, null);
        return objectMapper.readTree(response.body())
                           .path("payments")
                           .traverse(objectMapper)
                           .readValueAs(Payments.class);
    }
}
```

Inheritance relies on the principle of abstraction; with it, we gain the ability to abstract away (duplicated) low-level details to the base class (`API`) so that the subclass can focus on the (unique) high-level details (`UsersAPI`, `ForumAPI`, `BillingAPI`).

A good reason to refactor using inheritance is to clean up duplication and make generic functionality easier to use for more specific objects.

# Its about reuse not hierarchies

When most developers learn about inheritance, they’re presented with trivial Animal-Dog or Person-Staff-Teacher examples which map nicely to the real world but manifest real problems with the complex hierarchies they create.

It is very important to note that concepts which exist in the real world often do not translate into useful software objects. For example, it’s probably not important to model the hierarchy of a car’s speed, dimensions, upgrades, parts, and window tint within the context of an online oil change booking form. However, in the design of a multiplayer racing game, they may very well be useful and necessary.

Why do we invent abstractions anyway? What's the point? To help us express what we need to express to meet the requirements, and to do so in a maintainable way — the goal of software design is to write code that:
- Meet the needs of the customer
- Is cost-effectively changed by developers

Inheritance used properly helps to. Therefore, inheritance is a tool we use upon finding opportunities for reuse, not to express real-world hierarchical similarities.

# Composition and delegation

If inheritance is really just about reuse (and not about world-building), then that means we can skip the hierarchy entirely and use the techniques of composition and delegation to implement inheritance.

That means we could also refactor the duplication of the HTTP logic like this.

```java
import java.io.IOException;
import java.net.http.HttpResponse;

public class UsersAPI {

    private final API http;
    private final String baseURL = "https://example.com/users";

    // Constructor with dependency injection
    public UsersAPI(API http) {
        this.http = http;
    }

    // High-level method
    public HttpResponse<String> getUsers() throws IOException, InterruptedException {
        return http.get("/", null, null); // Calls the base API's GET method
    }
}
```

Therefore, if we encounter duplication, we have a lot of refactoring techniques:

Inheritance (Extract superclass): When you have two or more classes with common methods or fields, create a superclass and inherit the shared behaviour.

Composition (Extract class): When one class does work for two or more classes, extract the related methods to its own class and compose your reliant classes with the new one.

Rule of thumb — only inherit from contracts, not concretions: If you’re going to use inheritance the classic OO way, I generally advise that you only inherit or extend from contracts (interfaces, abstract classes, types) and not from concretions (classes). There’s a design method which explains why this rule makes sense (see Responsibility-Driven Design), but as a quick rule of thumb, you’ll know if you’re using inheritance poorly if you find yourself needing to subclass a concrete class.