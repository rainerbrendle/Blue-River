# PR2

Peace River Version 2

# Active Database Cluster

> Using Go, Golang-based data models with a collectiion of Postgres Database Shards and Schema-Free Database Schemas implementing an Anti-Entropy eventual consistency database cluster

###### &copy; 2023 R. Brendle, Port Charlotte, FL
  
### Sharding and the Sharding Code
We make the assumption that a distributed database is sharded and we are using a general sharding code criteria, which we can represent as an integer number. 

(In business scenarios we may want consider it as "company code" representign a department. but it can be anything).
We make the assumption that there is a sharding code 0, which represents the control node of the cluster, and which knows about the meanign of the other shards and also about the objects maintained everywhere serving as a directory service for the cluster.

All data are JSON-based data structures, which can transformed into Go structs and APIs using Go's JSON Marshaler 

### Message-based

Creating content for the active database cluster is achieved by sending messages to objects in shards. Message delivery can be done via Kafka, but in the end any message reliable message delivery protocol can be used.  We are implementing here the famous Actor model. Messages are JSON-based data structures and have some specific content as sender, receiver and a timely order which allows forming a queue for every Actor.

The sharding code and the timely order together create a Lamport time stamp for every messages.

Message delivery is done to a Journal data structure.


### Event-Driven
### Journal
### Event-Condition-Action Rules
### Agents, Active Processing and High-Water Mark
### Service-API for Snapshot Consistent Data Acccess for Reading

> 

