# Pessimistic vs Optimistc Locking

When dealing with concurrent access to shared resources, two primary locking strategies can be employed: pessimistic locking and optimistic locking.

**Pessimistic locking**: assumes that conflicts will occur and therefore locks the resource for exclusive access when a transaction begins. This approach is suitable for environments with high contention, as it prevents other transactions from modifying the resource until the lock is released. However, it can lead to reduced concurrency and potential deadlocks.

**Optimistic locking**: assumes that conflicts are rare and allows multiple transactions to access the resource concurrently. Instead of locking the resource, it checks for conflicts only when a transaction attempts to commit changes. If a conflict is detected, the transaction is rolled back and can be retried. This approach is more efficient in low-contention environments, as it maximizes concurrency and minimizes locking overhead.

Optimistic locking is often implemented using versioning, where each resource has a version number that is checked and updated during transactions. If the version number has changed since the transaction began, it indicates a conflict.
