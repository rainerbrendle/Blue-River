# PR2

Peace River Version 2

After experiments using Javascript and SQL or Java, it showed up that using GO and JSON with JSONB on Postgresql may be the better option for a headless database cluster. 

# Active Database Cluster

> Using Go, Golang-based data models with a collectiion of Postgres Database Shards and Schema-Free Database Schemas implementing an Anti-Entropy eventual consistency database cluster may be the best option.

###### &copy; 2023 R. Brendle, Port Charlotte, FL
  
## Actors, Messages and Query Language
We make the assumption that we have a collection of logical "actor" classes, which are represented by object IDs in a timely and spatial distribution. We are sending messages to actors. and can retrieve data from actors as query results on matrialized views of the actor message records.

The spatial distribution is given by a sharding categorry, which can be represented by departments or organizations. We are sending messages to organizations and we can then represent this as a process flow by "swim lanes", if we want to.

Actors receive create, modify and cancel messages and have read-only views in an insert-only, append only database pattern. Everything is distributed in a cloud of data plane shards, which again is managed by a control plane database instance having a full understanding of shards and services. We call this "Yellow Pages" and "Blue Pages" - taken from a more old-fashioned US phone book layout. where we make the analogy, that Yellow Pages are for departments, while the Blue Pages are for the services of the departments. We will need to add "White Pages" for a B2B scenarios and buiness networks)

"Blue Pages" are there to define by GO-based model definition in GO structs. We have messages to be send and recieved defining insert[only oeprations, while we define read operation using a Go-based query language, which can represent all SQL query operations inclusing host variables including TOP or LIKE operations and are using materialized views and corresponding HTTP views.

It is basically a Lambda architecture.

This allows to have a modern, event-driven busines process management model based on "Swim Lanes", where messages flow from detartments to departments, while applications are defined via service APIs and workflows may sit on the side and act for assigning users to tasks.

### Sharding and the Sharding Distribution Code
We make the assumption that we are building a distributed database cluster in a full eventual consistency manner and we are using a general 'ShardingCode' criteria, which we can represent as an integer number. The integer number can be interpreted as a "company code" in some cases.

In business scenarios we may want to consider the sharding criteria as "company code" representing a department. but it can be anything, which allows seperating buckets of data, which define a collectiion of "Actors".We can alo add booking periods, if we want to.

We make the assumption that there is a sharding code 0, which represents the control node of the cluster, and which knows about the meaning of the other shards and also about the objects maintained everywhere serving as a directory service for the cluster. All other may start with 100.

Every Shard is intended to have more than one replica, 3 at minimum. We will have a roll-forward model, we do not do roll-backs in general.

All data are JSON-based data structures, which can be transformed into Go structs and and then API calls using Go's JSON Marshaler and the Go net/http interface. 

### Message-Based

Creating content for the active database cluster is achieved by sending messages to objects in the shards. Messages are insert, modify and cancel messages and we have an insert-only/append-only model. On the creation of the messages itself, we can have a more classical CRUD model with PUT and DELETE operations

Messages transform into immutable Records and Records transform into materialized views for reading. 

Message delivery can be done via Kafka, but in the end any reliable message delivery protocol can be used.  We are implementing here the Actor model that MIT was once describing. Messages are JSON-based data structures and have some specific content as sender, receiver and a timely order which allows forming a queue for every Actor.

The sharding code and the timely order together creates a Lamport time stamp for every messages.

Message delivery is done to a Journal data structure. When the message is received it can create follow-up action in a stable and consistent manner with snapshot consitency,


### Event-Driven

Every received messages represents an event, which is immutable and recorded. It represents history and has already happenend.

### Journal

The Journal table records the events and brings them into a guaranteed timely order per Shard. 

### Processing
Every Journal entry can be subscribed to and can trigger further actions using an event-codition-action model. Which actions have been triggered and executed  are maintained in a high-water mark table, which just follows the Journal using processing agents.

### High-Water Mark
To remember which follow-up actions have been taken yet, we maintain a high-water mark table, which represents a vector-clock implementation  Follow up actions are triggered by agent jobs, which scan the datasae shards for pending actions and which can run as GO routines.

### Event-Condition-Action Rules

An action is a next step to be taken and it has to happen within the same logical transaction as the high-water marks entry is written. Since we have a transactional journal, we can defer actions as long there is no concurrency on the single actor and oepratiosn are in sequence.

### Agents, Active Processing and High-Water Mark
The active transformation is done via agent jobs, which wake up, if new messages are to be processed and which sleep again if there is nothing there to be done..

### Service-API for Snapshot Consistent Data Acccess for Reading
The service A{I for reading need to add a where clause included as a temporal cutoff.

### Transactional Guarantees, Fail-Fast/Fail-Over
Since we are recording history, there is no roll-back, there only is a roll-forward model. We always recover from replica and we only move forward in time.
Errors must be processded using exception handling and can create new workflows as any action can create further workflows.\

# Colors
## White Pages, Business Nwtwork
## Yellow Pages: End Points of Messages, Swim Lanes
## Blue Pages : Paperwork
## Red Tape : Rules

> 

