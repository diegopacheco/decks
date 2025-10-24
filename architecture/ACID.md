# ACID

it's a property of database transactions intended to guarantee validity even in the event of errors, power failures. It's a RELATIONAL database concept.

## A == Atomicity

Atomicity ensures that a transaction is treated as a single unit, which either completely succeeds or completely fails. 

If any part of the transaction fails, the entire transaction is rolled back, and the database remains unchanged.

## C == Consistency

Consistency ensures that a transaction brings the database from one valid state to another valid state, maintaining all predefined rules, such as constraints, cascades, and triggers.

## I == Isolation

Isolation ensures that concurrent transactions do not interfere with each other. The intermediate state of a transaction is invisible to other transactions until the transaction is committed.

This prevents transactions from reading uncommitted data from other transactions, which could lead to inconsistencies.

## D == Durability

Durability guarantees that once a transaction has been committed, it will remain so, even in the event of a system failure. Committed data is saved to non-volatile storage, ensuring that it is not lost. Usualy using a WAL (Write Ahead Log) to achieve this.
