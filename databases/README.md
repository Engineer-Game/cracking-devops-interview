# Databases

## CAP Theorem

| Consistency                                           | [Availability](https://en.wikipedia.org/wiki/Availability)                                               | [Partition tolerance](https://en.wikipedia.org/wiki/Network\_partitioning)                                                      |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Every read receives the most recent write or an error | Every request receives a (non-error) response – without guarantee that it contains the most recent write | The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes |

Myths: [https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)

## Read and Write Locks

Read locks on a resource are shared, or mutually nonblocking: many clients may read from a resource at the same time and not interfere with each other. Write locks, on the other hand, are exclusive—i.e., they block both read locks and other write locks—because the only safe policy is to have a single client writing to the resource at given time and to prevent all reads when a client is writing. It depends of the storage engine.

The most basic locking strategy available in MySQL, and the one with the lowest overhead, is table locks. A table lock is analogous to the mailbox locks described earlier: it locks the entire table. When a client wishes to write to a table (insert, delete, update, etc.), it acquires a write lock. This keeps all other read and write operations at bay. When nobody is writing, readers can obtain read locks, which don’t conflict with other read locks.

## MySQL Master / Slave

MySQL master to slave(s) configuration is the most popular setup. In this design One(1) server acts as the master database and all other server(s) act as slaves.Writes can only occur on the master node by the application.

### Pros

\*Analytic applications can read from the slave(s) without impacting the master

\*Backups of the entire database of relatively no impact on the master

\*Slaves can be taken offline and sync back to the master without any downtime

### Cons

\*In the instance of a failure a slave has to be promoted to master to take over its place. No automatic failover

\*Downtime and possibly lost of data when a master fails

\*All writes also have to be made to the master in a master-slave design

\*Each additional slave add some load\* to the master since the binary log have to be read and data copied to each slave

\*Application might have to be restarted

In a master-master configuration each master is configured as a slave to the other. Writes and reads can occur on both servers.

### Pros

\*Applications can read from both masters

\*Distributes write load across both master nodes

\*Simple ,automatic and quick failover

### Cons

\* Loosely consistent

\*Not as simple as master-slave to configure and deploy

The new kid in town based on MySQL cluster design. MySQL cluster was developed with high availability and scalability in mind and is the ideal solution to be used for environments that require no downtime, high avalability and horizontal scalability.

See[MySQL Cluster 101](http://skillachie.com/2014/07/25/mysql-cluster-101/)for architecture diagram and information related to MySQL

Pros

\*(High Avalability)no single point of failure

\*Very high throughput

\*99.99% uptime

\*Auto-Sharding

\*Real-Time Responsiveness

\*On-Line Operations(Schema changes etc)

\*Distributed writes

### Cons

See[known limitations](http://dev.mysql.com/doc/refman/5.6/en/mysql-cluster-limitations.html)

## Why using Cassandra?

#### Decentralized

Every node in the cluster has the same role. There is no single point of failure. Data is distributed across the cluster (so each node contains different data), but there is no master as every node can service any request.

Supports replication and multi data center replication

Replication strategies are configurable.\[17] Cassandra is designed as a distributed system, for deployment of large numbers of nodes across multiple data centers. Key features of Cassandra’s distributed architecture are specifically tailored for multiple-data center deployment, for redundancy, for failover and disaster recovery.

#### Scalability

Read and write throughput both increase linearly as new machines are added, with no downtime or interruption to applications.

#### Fault-tolerant

Data is automatically replicated to multiple nodes for fault-tolerance. Replication across multiple data centers is supported. Failed nodes can be replaced with no downtime.

#### Tunable consistency

Writes and reads offer a tunable level of consistency, all the way from "writes never fail" to "block for all replicas to be readable", with the quorum level in the middle.\[18]

#### MapReduce support

Cassandra has Hadoop integration, with MapReduce support. There is support also for Apache Pig and Apache Hive.\[19]

#### Query language

Cassandra introduced the Cassandra Query Language (CQL). CQL is a simple interface for accessing Cassandra, as an alternative to the traditional Structured Query Language (SQL). CQL adds an abstraction layer that hides implementation details of this structure and provides native syntaxes for collections and other common encodings.\[20] Language drivers are available for Java (JDBC), Python (DBAPI2), Node.JS (Helenus), Go (gocql) and C++.\[21]

## Mongodb

[Sharding](https://docs.mongodb.com/manual/reference/glossary/#term-sharding)is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations.

Database systems with large data sets or high throughput applications can challenge the capacity of a single server. For example, high query rates can exhaust the CPU capacity of the server. Working set sizes larger than the system’s RAM stress the I/O capacity of disk drives.

There are two methods for addressing system growth: vertical and horizontal scaling.

\_Vertical Scaling\_involves increasing the capacity of a single server, such as using a more powerful CPU, adding more RAM, or increasing the amount of storage space. Limitations in available technology may restrict a single machine from being sufficiently powerful for a given workload. Additionally, Cloud-based providers have hard ceilings based on available hardware configurations. As a result, there is a practical maximum for vertical scaling.

\_Horizontal Scaling\_involves dividing the system dataset and load over multiple servers, adding additional servers to increase capacity as required. While the overall speed or capacity of a single machine may not be high, each machine handles a subset of the overall workload, potentially providing better efficiency than a single high-speed high-capacity server. Expanding the capacity of the deployment only requires adding additional servers as needed, which can be a lower overall cost than high-end hardware for a single machine. The trade off is increased complexity in infrastructure and maintenance for the deployment.

MongoDB supports\_horizontal scaling\_through[sharding](https://docs.mongodb.com/manual/reference/glossary/#term-sharding).

A MongoDB [sharded cluster](https://docs.mongodb.com/manual/reference/glossary/#term-sharded-cluster) consists of the following components:

* [shard](https://docs.mongodb.com/manual/core/sharded-cluster-shards/): Each shard contains a subset of the sharded data. Each shard can be deployed as a [replica set](https://docs.mongodb.com/manual/reference/glossary/#term-replica-set)
* [mongos](https://docs.mongodb.com/manual/core/sharded-cluster-query-router/): The`mongos`acts as a query router, providing an interface between client applications and the sharded cluster.
* [config servers](https://docs.mongodb.com/manual/core/sharded-cluster-config-servers/): Config servers store metadata and configuration settings for the cluster. As of MongoDB 3.4, config servers must be deployed as a replica set (CSRS).

**Sharding**

* partitions the data-set into discrete parts.
*   **Replication**

    duplicates the data-set.

These two things can stack since they're different. Using both means you will shard your data-set across multiple groups of replicas. Put another way, you Replicate shards; a data-set with no shards is a single 'shard'.

A Mongo cluster with three shards and 3 replicas would have 9 nodes.

* 3 sets of 3-node replicas.
* Each replica-set holds a single shard.

## Kafka

Steps:

Download Kafka and Zookeeper

Run both services:

```
Kafka uses ZooKeeper so you need to first start a ZooKeeper server if you don't already have one. You can use the convenience script packaged with kafka to get a quick-and-dirty single-node ZooKeeper instance.

> bin/zookeeper-server-start.sh config/zookeeper.properties
[2013-04-22 15:01:37,495] INFO Reading configuration from: config/zookeeper.properties (org.apache.zookeeper.server.quorum.QuorumPeerConfig)
...
Now start the Kafka server:

> bin/kafka-server-start.sh config/server.properties
[2013-04-22 15:01:47,028] INFO Verifying properties (kafka.utils.VerifiableProperties)
[2013-04-22 15:01:47,051] INFO Property socket.send.buffer.bytes is overridden to 1048576 (kafka.utils.VerifiableProperties)
...
```

Create a Topic

```
> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic tes
```

Send Messages

```
> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
This is a message
This is another message
```

Consume Messages

```
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
This is a message
This is another message
```

Multibroker

```
cp config/server.properties config/server-1.properties
cp config/server.properties config/server-2.properties
```

```
config/server-1.properties:
    broker.id=1
    listeners=PLAINTEXT://:9093
    log.dir=/tmp/kafka-logs-1

config/server-2.properties:
    broker.id=2
    listeners=PLAINTEXT://:9094
    log.dir=/tmp/kafka-logs-2
```

```
> bin/kafka-server-start.sh config/server-1.properties &
...
> bin/kafka-server-start.sh config/server-2.properties &
...
```

[https://kafka.apache.org/quickstart](https://kafka.apache.org/quickstart)
