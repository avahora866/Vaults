---
tags:
  - Architecture
---
Client-server architecture is a computing model that separates an application into two main components:

1. **Clients**: End-user devices or applications that request services or resources
2. **Servers**: Computers or programs that provide those services or resources

# How It Works

In a client-server model:

- The client initiates requests to the server
- The server processes these requests and returns appropriate responses
- Communication happens over a network using specific protocols (like HTTP, FTP, etc.)

# Key Characteristics

- **Separation of Concerns**: Clients handle user interface and interaction, while servers manage data processing, storage, and business logic
- **Centralized Control**: Resources are centrally managed on servers, allowing for easier updates and maintenance
- **Scalability**: Servers can be scaled independently to handle increased loads
- **Resource Sharing**: Multiple clients can access the same server resources

# Common Examples

- Web browsers (clients) connecting to web servers
- Email clients connecting to mail servers
- Mobile apps connecting to backend API servers
- Database clients connecting to database servers

# Advantages

- Better security by keeping sensitive data on secured servers
- Centralized data management
- Easier maintenance and updates
- Efficient resource utilization

# Disadvantages

- Dependency on network connectivity
- Server can become a bottleneck if overloaded
- More complex to implement than standalone applications