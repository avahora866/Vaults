---
tags:
  - WebDevelopment
---
A webhook is an automated mechanism that allows one application to send real-time data to another application. It's a "push" model of communication, in contrast to an API, which typically uses a "pull" model.

Think of it this way:
 * API (Polling): You call a restaurant every five minutes to ask, "Is my order ready?" This is inefficient and repetitive.
 * Webhook: You give the restaurant your phone number, and they call you exactly when your order is ready. This is efficient and event-driven.
 
# How Webhooks Work
The process involves a few simple steps:
 * Register a URL: You provide a URL from your application (the "consumer") to another service (the "producer," e.g., GitHub, Stripe). This URL is your webhook endpoint, which is designed to accept incoming data.
 * An Event Occurs: A specific event happens in the producer application. For example, a code commit is pushed to a repository on GitHub, or a payment is successfully processed by Stripe.
 * HTTP POST Request: The producer automatically sends an HTTP POST request to your registered URL.
 * Payload Delivery: The body of this POST request contains information about the event, typically formatted as JSON. This data is called the "payload."
 * Action: Your application receives the POST request, parses the payload, and performs a predefined action, such as starting a CI/CD build, updating a database record, or sending a notification.

# Why Use Webhooks?
 * Real-time: You receive data as soon as an event occurs, eliminating the delay of polling.
 * Efficiency: Webhooks reduce the workload on both the consumer and producer systems. Your application isn't making constant, often empty, API calls, and the producer's server isn't handling those unnecessary requests.
 * Automation: They are fundamental to building automated workflows between different services.

# Common Use Cases
 * CI/CD Pipelines: GitHub or GitLab sends a webhook to a service like Jenkins when you push new code. Jenkins receives this and automatically triggers a build and deployment process.
 * Payment Processing: Stripe sends a webhook to your e-commerce server to confirm a successful payment (payment.succeeded), a failed payment, or a refund. Your application can then update the order status and notify the customer.
 * Notifications: A monitoring service sends a webhook to Slack or PagerDuty when a server goes down.
 * CRM Updates: When a user fills out a form on your website, a webhook can send their information directly to a CRM like Salesforce or HubSpot, creating a new lead.

# Practical Considerations
When implementing webhooks, you must address security and reliability.
Security
Since your webhook endpoint is a public URL, you must verify that incoming requests are legitimate. The standard method is signature verification.
 * The producer provides you with a secret key.
 * For each request, the producer creates a hash-based message authentication code (HMAC) signature using the secret key and the request payload. This signature is included in the request headers (e.g., X-Hub-Signature-256 for GitHub).
 * Your application computes its own signature using the same secret key and the received payload.
 * You compare your computed signature with the one in the header. If they match, the request is authentic and its payload is intact. If not, you reject the request.
Always use HTTPS for your endpoint to encrypt data in transit.
Reliability
 * Respond Quickly: Acknowledge receipt of a webhook immediately by returning a 2xx status code (e.g., 200 OK). Perform any complex or time-consuming logic asynchronously using a message queue (like RabbitMQ or SQS). If you take too long to respond, the producer may assume the delivery failed and retry, leading to duplicate events.
 * Handle Retries: Producers often retry sending a webhook if they don't receive a 2xx response. Your system should be idempotent, meaning it can handle receiving the same event multiple times without causing duplicate actions (e.g., charging a customer twice).
 * Log Payloads: Store the raw payloads of incoming webhooks. This is useful for debugging if an error occurs during processing.
 
# Webhook vs. API (Polling)

| Feature | Webhook (Push) | API (Polling) |
|---|---|---|
| Data Flow | The producer pushes data to the consumer when an event happens. | The consumer pulls data from the producer at set intervals. |
| Efficiency | High. Communication only occurs when there is new data. | Low. Many requests return no new information, wasting resources. |
| Timeliness | Real-time. Data is received almost instantly. | Delayed. Data is only as fresh as the last polling interval. |
| Setup | Consumer creates an endpoint and registers it with the producer. | Consumer writes logic to make repeated requests to the producer's API. |
