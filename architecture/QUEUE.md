# Queue

Queue it's a data structure that follows the First In First Out (FIFO) principle. Elements are added to the back of the queue and removed from the front.

In distributed systems, queues are often used to manage tasks, messages, or data that need to be processed asynchronously. They help in decoupling different parts of a system, allowing for better scalability and fault tolerance.

Queues use cases are:

* **Asynchronous processing**: Tasks can be added to a queue and processed by worker nodes at their own pace.
* **Load balancing**: Distributing tasks across multiple workers to ensure no single worker is overwhelmed.
* **Message brokering**: Facilitating communication between different services or components in a system.
* **Rate limiting**: Controlling the rate at which tasks are processed to avoid overwhelming downstream systems.
* **Event sourcing**: Storing a sequence of events that can be processed later to reconstruct the state of a system.

Common Issues with Queues:
* **Message loss**: If a queue is not properly configured, messages may be lost during transmission or processing.
* **Duplicate messages**: In some cases, messages may be delivered more than once, leading to redundant processing.
* **Latency**: Queues can introduce delays in processing, especially if they become overloaded.
* **Backpressure**: If the rate of incoming messages exceeds the processing capacity, it can lead to increased latency and potential message loss.