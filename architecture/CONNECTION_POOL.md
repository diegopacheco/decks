# Connection Pool

A connection pool is a cache of database connections maintained so that the connections can be reused when future requests to the database are required. Connection pools are used to enhance the performance of executing commands on a database. Opening and maintaining a database connection for each user, especially requests made to a dynamic database-driven website application, is costly and wastes resources.

It's expensive to open and close connections to the database all the time. That's why this objects are created as the application bootup and they are borrowed and returned to the pool as needed.

Java has a great connection pooling libraries like HikariCP.