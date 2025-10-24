# BASE 

A concept from NOSQL databases.
BASE systems prioritize availability instead of immediate consistency.

## BASE Properties

BASE is an acronym that describes the consistency model used by many NoSQL systems. It stands for:

BA == Basically Available

The system guarantees availability - it will always respond to requests, even if the response contains stale or inconsistent data. The database remains operational even when parts of the system fail.

S == Soft state
The state of the system may change over time, even without new input, due to eventual consistency. Data doesn't have to be immediately consistent across all nodes.

E == Eventual consistency
Given enough time without new updates, all replicas will eventually converge to the same value. The system doesn't guarantee immediate consistency but promises that consistency will be achieved eventually.