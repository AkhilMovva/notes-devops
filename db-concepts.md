# Database Concepts

## MySQL Proxy

- The MySQL Proxy is an application that communicates over the network using the MySQL Network Protocol and provides `communication` between one or more MySQL servers and one or more MySQL clients. In the most basic configuration, MySQL Proxy simply passes on `queries` from the `client to the MySQL Server` and returns the `responses` from the `MySQL Server to the client`.

- Because MySQL Proxy uses the `MySQL network protocol`, any MySQL compatible client (include the command line client, any clients using the MySQL client libraries, and any connector that supports the MySQL network protocol) can connect to the proxy without modification.

- In addition to the basic pass-through configuration, the MySQL Proxy is also capable of `monitoring and altering` the communication between the client and the server. This interception of the queries enables you to add profiling, and the interception of the exchanges is scriptable using the Lua scripting language.

- By `intercepting` the queries from the client, the proxy can `insert additional queries` into the list of queries sent to the server, and `remove the additional results` when they are returned by the server. Using this functionality you can add informational statements to each query, for example to monitor their execution time or progress, and separately log the results, while still returning the results from the original query to the client.

- The proxy allows you to perform `additional monitoring, filtering or manipulation` on queries without you having to make any modifications to the client and without the client even being aware that it is communicating with anything but a genuine MySQL server.

## Partitions - MySQL

- `Rage Partition` - AGE, DATE, EMP ID, SERIAL NO. - partitions based on column values falling within a given range
- `List Partition` - LOCATION, - selected based on columns matching one of a set of discrete values
- `Hash Partition` - selected based on the value returned by a user-defined expression that operates on column values in rows to be inserted into the table
- `Key Partition`  - similar to partitioning by HASH, except that only one or more columns to be evaluated are supplied, and the MySQL server provides its own hashing function

## Table partitioning keys

A table partitioning key is an `ordered set of one or more columns` in a table. The values in the table partitioning key columns are used to determine in which `data partition each table row belongs`.

To define the table partitioning key on a table use the `CREATE TABLE statement with the PARTITION BY` clause.

- Choosing an effective table partitioning key column is essential to taking full advantage of the benefits of table partitioning. The following guidelines can help you to choose the most effective table partitioning key columns for your partitioned table.
- Define `range granularity to match data roll-out`. It is most common to use week, month, or quarter.
- Define `ranges to match the data roll-in size`. It is most common to partition data on a date or time column.
- Partition on a column that provides advantages in `partition elimination`.

## Partition vs Sharding

Sharding and partitioning are both about `breaking up a large data set into smaller subsets`. The difference is that `sharding implies the data is spread across multiple computers` while partitioning does not. `Partitioning is about grouping subsets of data within a single database instance`. In many cases, the terms sharding and partitioning are even used synonymously, especially when preceded by the terms “horizontal” and “vertical.” Thus, “horizontal sharding” and “horizontal partitioning” can mean the same thing.

- Horizontal partitioning just divides a table on the basis of rows.
- But Sharding divides the table on the basis of rows in separate schemas that are on different servers.
  - When we replicate the schema across (typically) multiple instances or servers, using some kind of `logic or identifier` to know which instance or server to look for the data. An identifier of this kind is often called a `Shard Key`.

## Optimization Techniques

Options

- Scaling up the Hardware
  - Limit
- Add replicas
  - Eventual Consistency
- Sharding
  - complexity
    - partition mapping
    - routing layer
    - non-uniformity
      - Re-sharding
  - Analytical type queries

## Distributed systems - Data consistency

- CAP & ACID

- `Single DB Server`
  - Single point of failure
  - cost vertical scaling - High (Impossible)
  - Latency (High)

- `Multi Master`
  - Sync & Async
  - split-brain
    - distributed consensus
      - 2pc,3pc,mvcc(postgress),saga

## Cassandra

The ideal Cassandra application has the following characteristics:

- `Writes exceed reads` by a large margin.
- Data is `rarely updated` and when updates are made they are `idempotent`.
- `Read Access` is by a known `primary key`.
- Data can be `partitioned via a key` that allows the database to be spread evenly across `multiple nodes`.
- There is no need for joins or aggregates.

Some of examples of good use cases for Cassandra are:

- `Transaction logging`: Purchases, test scores, movies watched and movie latest location.
- Storing time `series data` (as long as you do your own aggregates).
- `Tracking` pretty much anything including order status, packages etc.
- Storing `health tracker` data.
- `Weather` service history.
- `Internet of things` status and event history.
- `Telematics`: IOT for cars and trucks.
- `Email envelopes`—not the contents.

Fetch huge amounts of data

Cassandra is `heavily optimized` for access to data by `primary key` - `full, partial, or at least partition key`. Other access patterns require additional work. Theoretically you can use secondary index on the corresponding column, but it's only recommended if you're searching the data in addition to having at least partition key - if you just use that `column alone`, it will still reach all nodes and fetch all data, so it will be `much slower`. And you'll need to keep in mind other limitations, such as, cardinality of the column, etc.

Programmatically, you can do the full scan of data as well, but it shouldn't be simple `select * from table` as it will `overload` coordinating node, lead to `timeouts`, etc. Instead it should be more sophisticated solution - it's better to perform `scan by reading data from individual token ranges`, sending the queries to the nodes that are keeping the `corresponding ranges`, and it's possible to do in `parallel` - this is how Spark Cassandra Connector and DSBulk are working.

## Kafka

Use cases of Kafka

- `Metrics` − Apache Kafka is often used for operational monitoring data. This involves aggregating statistics from distributed applications to produce centralized feeds of operational data.
- `Log Aggregation Solution` − Apache Kafka can be used across an organization to collect logs from multiple services and make them available in a standard format to multiple consumers.
- `Stream Processing` − Popular frameworks such as Storm and Spark Streaming read data from a topic, process it, and write processed data to a new topic where it becomes available for users and applications. Apache Kafka’s strong durability is also very useful in the context of stream processing.

## LINKS

- [AWS Sharding blog](https://aws.amazon.com/blogs/database/sharding-with-amazon-relational-database-service/)
- [baeldung cassandra-keys](https://www.baeldung.com/cassandra-keys)
- [Netflix Kafka](https://www.meritdata-tech.com/resources/blog/digital-engineering-solutions/netflix-apache-kafka-business-intelligence/)
- [MySQL Proxy](https://www.cmi.ac.in/~madhavan/courses/databases10/mysql-5.0-reference-manual/mysql-proxy.html)
- [Cassandra mistakes](https://blog.softwaremill.com/7-mistakes-when-using-apache-cassandra-51d2cf6df519)
- [Apache Kafka](https://www.mygreatlearning.com/blog/apache-kafka/)
- [Kafka Real-world usecases](https://dzone.com/articles/real-world-examples-and-use-cases-for-apache-kafka)
