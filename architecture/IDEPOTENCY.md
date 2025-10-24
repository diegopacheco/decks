# IDEPOTENCY

It's a interesting property that allow the same operation to be performed multiple times without changing the result beyond the initial application.

In REST APIs, idempotency is an important concept, especially for HTTP methods. Here are some common HTTP methods like GET, HEAD, OPTIONS, TRACE, PUT.

- **GET**: The most famous and common Idempotent. Retrieving a resource multiple times does not change its state.

Idepotency it's important because if the same request arrives twice(there is not side effects), the server can safely ignore the second request or return the same result as the first one without any unintended consequences or just re-process the samething again without issues.

Such principle keep us from writing complex software. For instance if a GET operation also does inserts and deletes, we would have to handle the case where the same GET request is sent multiple times, which could lead to data inconsistency and unexpected behavior.

So we want honor the idempotency principle to keep our software simple and predictable.