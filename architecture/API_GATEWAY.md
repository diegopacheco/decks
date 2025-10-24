# API GATEWAY

It's a architecture pattern that acts as a single entry point for a set of microservices, handling requests by routing them to the appropriate service, aggregating responses, and performing cross-cutting tasks such as authentication, logging, and rate limiting.

## API Gateway vs Load Balancer

An API Gateway and a Load Balancer serve different purposes in a microservices architecture:
- **API Gateway**: Primarily focuses on managing and routing API requests. It handles tasks such as request transformation, response aggregation, authentication, and rate limiting. It operates at the application layer (Layer 7) of the OSI model.

- **Load Balancer**: Primarily focuses on distributing incoming network traffic across multiple servers to ensure high availability and reliability. It operates at both the transport layer (Layer 4) and application layer (Layer 7) of the OSI model, depending on the type of load balancer used.

## Key Features

- **Request Routing**: Directs incoming requests to the appropriate microservice based on the request path, method, or other criteria.
- **Response Aggregation**: Combines responses from multiple microservices into a single response to the client.
- **Cross-Cutting Concerns**: Manages common functionalities such as authentication, logging, rate limiting, and caching.
- **Protocol Translation**: Converts requests and responses between different protocols (e.g., HTTP to WebSocket).
- **Load Balancing**: Distributes incoming requests across multiple instances of a microservice to ensure high availability and reliability.