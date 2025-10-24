# Load Balancer

It's a service that distributes incoming network traffic across multiple servers to ensure no single server becomes overwhelmed, improving application availability and responsiveness.

Load balancers can operate at different layers of the OSI model, such as Layer 4 (Transport Layer) and Layer 7 (Application Layer), providing various features like SSL termination, session persistence, and health monitoring of backend servers. 

## Common Load Balancing Algorithms

1. **Round Robin**: Distributes requests sequentially across the servers.
2. **Least Connections**: Directs traffic to the server with the fewest active connections.
3. **IP Hash**: Uses the client's IP address to determine which server will handle the request.
4. **Weighted Round Robin**: Similar to Round Robin but allows assigning weights to servers based on their capacity.
5. **Random**: Distributes requests randomly across the servers.
6. **Metric-Based**: Uses specific metrics (like response time or server load) to make load balancing decisions.
