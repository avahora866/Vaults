---
tags:
  - SoftwareDesign
---
HATEOAS (Hypermedia As The Engine Of Application State) is a principle of REST API design.

# Key Idea:

Clients interact with a REST API entirely through hypermedia provided dynamically by server responses. In other words, instead of hardcoding endpoint URLs, the client discovers available actions via links in the responses.

# Breakdown:

- **Hypermedia**: Information like links or buttons in a response (like in HTML).
- **Engine of Application State**: These links guide the client through the app's state transitions.

# Example:

**Without HATEOAS** (traditional REST):

```http
GET /orders/123
Response:
{
  "orderId": 123,
  "status": "shipped"
}
```

Client must know how to cancel or update an order (e.g., `POST /orders/123/cancel`).

**With HATEOAS**:

```http
GET /orders/123
Response:
{
  "orderId": 123,
  "status": "shipped",
  "_links": {
    "self": { "href": "/orders/123" },
    "cancel": { "href": "/orders/123/cancel", "method": "POST" }
  }
}
```

Client reads the `_links` and knows what it can do next, without needing to hardcode endpoint logic.

# Benefits:

- Decouples client from server structure.
- Server can evolve independently.
- Helps build more dynamic, discoverable APIs.

# Drawbacks:

- Harder to implement and test.
- Few clients/libraries fully support it in practice.