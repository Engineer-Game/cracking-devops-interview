# Architecture

## Design

For a high level system design some things that should be handled well are Speed, Reusability, Scalability, Availability and Security.

Reusability comes from the software architecture, design patterns, classes and data structures. How modular your class methods are ? Usage of proper abstraction (abstract classes and interfaces) ? Correct visibility of classes and methods (private, public, protected wherever necessary) ? Handled class and package dependencies properly ? Any framework used MVC etc. ? and so on...

Scalability comes from handling of large amount of data and handling large number of requests effectively. Multiple web servers serving requests ? How is database designed and managed ? Proper indexing of database tables ? Caching of requests handled properly or technologies used for caching ? Master-slave configuration ? Multiple nodes ? Usage of NoSQL to avoid joining tables ? Hadoop or MapReduce design patterns for models ? Knowledge of what is persistent data and what should be kept in memory ? How to read persistent data effectively ? and so on...

Speed comes with improving application performance, views loading faster. Lowering of server latency using Memcache, Redis etc. ? UI components handling for web page requests (separate css, js files, CDN serving images, lazy loading of images) ? Avoiding costly database queries and using Session variables and cookies wherever necessary ? Caching of web pages on browsers ? and so on...

Availability comes from how fast you can recover from failures. Multiple servers ? Backup databases ? Round robin server request handling ? Proper logging mechanisms ? Efficient log file reading ? Log rotation ? Proper "downtime" messages for users and so on ...

Security comes with strong authentication and encryption maintained. HTTPS ? Prevention of XSS and MySQL injection attacks ? Two step verification of user input data ? Proper cleansing of user input data (is it what is only required) ? Captcha ? Securing api's using HMAC and nonces ? Strong passwords only ? Proper access rules for files defined in .htaccess ? Proper permissions of the code base ? and so on..

#### Scale a system from two engineers working in github and ssh to the server directly

#### Microservices Dependencies

#### Service Discovery

The client makes a request to a service via a load balancer. The load balancer queries the service registry and routes each request to an available service instance. As with client-side discovery, service instances are registered and deregistered with the service registry.

The AWS Elastic Load Balancer (ELB) is an example of a server-side discovery router. An ELB is commonly used to load balance external traffic from the Internet. However, you can also use an ELB to load balance traffic that is internal to a virtual private cloud (VPC). A client makes requests (HTTP or TCP) via the ELB using its DNS name. The ELB load balances the traffic among a set of registered Elastic Compute Cloud (EC2) instances or EC2 Container Service (ECS) containers. There isn’t a separate service registry. Instead, EC2 instances and ECS containers are registered with the ELB itself.

#### Microservices different versions

What these microservice-based systems have lost is monolithic versioning: a single place where valid versions of the system is tracked as if in a monolithic repository. Here are two ways to get monolithic versioning back.

Monolithic repository

Keep a monolithic repository for your polylithic architecture. That repository will have a single tree of versions which are tracked in the same way as a monolithic architecture. If the entire deployed system is on the same version, then its a valid deployment. This approach has its downsides, as its harder to identify whats a service and whats shared code, but it has upsides too. (Etsy takes this view.)

System Version repository

Create a new repository that holds a listing of the versions of all of the services and components in the system. Anytime this repository is updated, a new valid system version is created.

The key to this approach is using this repository as a source of truth for your system. You can add configuration data, or information on how to deploy services. The point is that deployments and development environments should use this repository as key to creating and operating on the system.

For example, consider the docker-compose configuration file. It contains a listing of each image version. If we checked in this file (or something like it, sans host-specific configuration) to a separate repository, it represents a monolithic versioning of the system. This can be used to create deployments and development environments at will.

#### Design a system like S3

[http://stackoverflow.com/questions/39042924/interview-assignment-design-a-system-like-s3](http://stackoverflow.com/questions/39042924/interview-assignment-design-a-system-like-s3)

I listed requirements - CRUD Microservices on objects - Caching layer to improve performance - Deployment on PaaS - resiliency with failover - AAA support ( authorization, auditing, accounting/billing) - Administration microservices (user, project, lifecycle of object, SLA dashboard) - Metrics collection (Ops, Dev) - Security for service endpoints for admin UI

I defined basic APIs.\
[https://api.service.com/services/get](https://api.service.com/services/get)Arugments object id, metadata return binary object[https://api.service.com/services/upload](https://api.service.com/services/upload)Arguments object returns object id[https://api.service.com/services/delete](https://api.service.com/services/delete)Arugments object id returns success/error[http://api.service.com/service/update-meta](http://api.service.com/service/update-meta)Arugments object id, metadata return success/error

![](https://i.stack.imgur.com/jg09S.png)

### Design a scalable DNAT system

[http://www.brianlinkletter.com/how-to-build-a-network-of-linux-routers-using-quagga/](http://www.brianlinkletter.com/how-to-build-a-network-of-linux-routers-using-quagga/)

### Design an auth system

#### Design a URL shortener

## Distributed Systems

#### Centralized vs distributed vs decentralized

Distributed means not all the processing of the transactions is done in the same place. This does not mean that those distributed processors aren't under the control of a single entity. (Think of gas stations, there are Shell stations all over yet all are Shell)

Decentralized means that not one single entity has control over all the processing. By nature, this implies that it is distributed among various parties.

[https://medium.com/@bbc4468/centralized-vs-decentralized-vs-distributed-41d92d463868\\#.idzq7pj17](https://medium.com/@bbc4468/centralized-vs-decentralized-vs-distributed-41d92d463868#.idzq7pj17)

#### Decentralized consensus

Decentralized consensus is the main “invention” of the Bitcoin system. The idea is that users can come to a consensus as to what information is accepted by “voting.” In the case of Bitcoin the public ledger, the Blockchain, is agreed upon by this consensus.

About every 10 minutes on average a new “block” is found by a Bitcoin miner somewhere. This adds a section to the Bitcoin ledger and includes new transactions. Bitcoin miners do essentially useless work and they are rewarded when they “find” a new block in a random process. The rewards are obtained based on how much computer power, or “hashing power.” The blocks are then broadcast among users and the users decide whether to continue to rebroadcast the block. If some users try to circulate a “bad” block that does not meet the majority consensus it may circulate for a short time but will eventually be overtaken by the majority consensus.

This system has a number of implications such as making changes. To “fix a bug” in the system or make a change means breaking consensus. Some people claim that the entire system is in the hands of the software developers. That is true to a certain extent since the developers have tremendous influence over the software changes. However, users have the ultimate say by downloading and running the software. There is much futuristic discussion whether some alternate to Bitcoin will overtake it or whether consensus will be able to be achieved among Bitcoin users.

#### Paxos – How it Works

The basic steps in Paxos are very similar to 2PC:

Elect a node to be a Leader / Proposer

The Leader selects a value and sends it to all nodes (called Acceptors in Paxos) in an accept-request message. Acceptors can reply with reject or accept.

Once a majority of the nodes have accepted, consensus is reached and the coordinator broadcasts a commit message to all nodes.

[https://www.quora.com/Distributed-Systems-What-is-a-simple-explanation-of-the-Paxos-algorithm](https://www.quora.com/Distributed-Systems-What-is-a-simple-explanation-of-the-Paxos-algorithm)
