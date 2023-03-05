# PR2

Peace River Version 2

# Active Database Cluster

> Using Go, Golang-based data models with a collectiion of Postgres Database Shards and Schema-Free Database Schemas implementing an Anti-Entropy eventual consistency database cluster

###### &copy; 2023 R. Brendle, Port Charlotte, FL
  
### Sharding and the Sharding Distribution Code
We make the assumption that a distributed database is sharded and we are using a general sharding code criteria, which we can represent as an integer number. 

(In business scenarios we may want consider it as "company code" representing a department. but it can be anything, which allows seperating buckets of data, which define a collectiion of "Actors".

We make the assumption that there is a sharding code 0, which represents the control node of the cluster, and which knows about the meanign of the other shards and also about the objects maintained everywhere serving as a directory service for the cluster.

Every Shard is intended to have more than one replica.

All data are JSON-based data structures, which can be transformed into Go structs and and then API calls using Go's JSON Marshaler and the Go net/http interface. 

### Message-Based

Creating content for the active database cluster is achieved by sending messages to objects in the shards. Messages are insert, modify and cancel messages, we have an insert-only/append-only model.

Messages transform into Records and Records transform into materialized views for reading. 

Message delivery can be done via Kafka, but in the end any message reliable message delivery protocol can be used.  We are implementing here the famous Actor model. Messages are JSON-based data structures and have some specific content as sender, receiver and a timely order which allows forming a queue for every Actor.

The sharding code and the timely order together create a Lamport time stamp for every messages.

Message delivery is done to a Journal data structure.


### Event-Driven

Every received messages represents an event, which immutable and recorded. It represents history and has already happenend.

### Journal

The Journal table records the events and brings them into a guarnteed timely order per Shard. 

### Processing
Every Journal entry can be subscribed to and can trigger further actions using an event-codition-action model. Which actions have be triggered and executed jet are maintained in a high-water mark table, which just follows the Journal using processing agents.

### High-Water Mark
To rember which follow-up actions have been taken yet, we maintain a high-water mark table, which represents a vector-clock implementation 

### Event-Condition-Action Rules

An action is a next step to be taken and it has to happen within the same transaction as the high-water marks entry is written.

### Agents, Active Processing and High-Water Mark
### Service-API for Snapshot Consistent Data Acccess for Reading
### Transactional Guarantees, Fail-Fast/Fail-Over

> 

