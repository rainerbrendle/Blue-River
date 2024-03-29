# Blue River

Peace River Version 2

After experiments using Javascript and SQL or Java, it showed up that using GO and JSON with JSONB on Postgresql may be the better option for a headless database cluster. 

# Active Database Cluster

> Using Go, Golang-based data models with a collectiion of Postgres Database Shards and Schema-Free Database Schemas implementing an Anti-Entropy eventual consistency database cluster may be the best option. Especially if you can add CitusDB's column store for PostgreSQL or Apache Druid for Analytics as materialized views, which you can, if you have an event-driven Event-Condition-Action model of an Active Database cluster.  

###### &copy; 2023 R. Brendle, Port Charlotte, FL

## Loose Coupling
We have ayynchronous messsages for transactions and APIs for reading, which are basically JSON or JSON-like structures, but form a network protocol between UIs and components.

## Actors, Messages and Query Language
We make the assumption that we have a collection of logical "actor" classes, which are represented by object IDs in a timely and spatial distribution. We are sending messages to actors for Insert and Cancel operations. and can retrieve data from actors as stable query results on matrialized views of the actor message records. "Actors' can be anything including any business document structures like orders, notes, but also master and configuration data. 

### Messages
Messages are the preliminary data. They represent just wishes of information to be created and eventually  recorded. We have an eventual-conistency architecture. 

Besides being neccessary to be transported there is no necessity to keep message states for very along time. Instead they can be persisted also just as an in-memory state in an application server - as long as we can give it a REST interface. Which we do, when we have the application server holding the state in a docker container and we can address it. A fail-over strategy for preliminary data is to throw them away and restart the aplication from a save point. A container for a GO application server can do sO, We can have peristency, but we do not have to.

### Actors
Actors itself can be defined as objects being maintained in a persistent "Journal" of recorded messages. We are sending messages to Actors and we record the messages there. A Journal is a peristent record of the messages being received. From the "Journal" any kind of projection can be achieved.

### Queries
Real perisitent queries must be operated on stable, immutable data. We generate immutable data out of the Actors' Journal using views on this - basically as "materialized views"

### Sharding
The spatial distribution is given by a sharding category, which can be represented by departments oF organizations. We are sending messages to organizations and we can then represent this as a process flow by "swim lanes", if we want to. Swim Lanes represent work places in organizations, where Actors then belong to. The end points of Swim Lanes can represent "work places" in an organisation. 

Actors receive create-, modify- and cancel-messages of requests and notifcations and then have read-only views in an insert-only, append only database pattern. Everything is distributed within a cloud of data plane-oriented shards of database instances, which again is managed by a control plane-centric shard as distributed and mutiple  database instance having a full understanding of the meta data of the shards and services. Fail-over on thsi level acn be done via the RAft algorithm. The control plane instnaces must exist in a odd number of replica.

We call these cluster management structures in the control node then "Yellow Pages" and "Blue Pages". They are  to be replicated to all the data nodes, since they are needed anywhere.

In an old-fashioned US phone book layout - where we make this analogy to -  Yellow Pages are for departments and workplaces, while the Blue Pages are for the services of the departments. We will need to add "White Pages" for B2B scenarios and business networks and of course business rules.

"Blue Pages" are there to be defined by GO-based model definition in GO structs. We have messages to be send and received by defining insert-only operations, while we define read operations on stable data using a Go-based query language, which can represent all SQL query operations including host variables and including TOP or LIKE operations and are using materialized views and corresponding HTTP views. Data become stable because they are produced via a timely ordered journal of "records".

It is basically an aggresive Lambda architecture bsed on stateless services without side-effects. We can define actor operations as closures, where the URL of an object instance forms the parameter of the closure functions.

This allows having a modern, event-driven busines process management model based on "Swim Lanes", where messages flow to workplaces from departments to departments, while applications are defined via service APIs and workflows may sit on the side and act for assigning users to tasks.

### Sharding and the Sharding Distribution Code

We make the assumption that we are building a distributed database cluster in a full eventual consistency manner and we are using a general 'Sharding Code' criteria, which we can represent as an integer number. The integer number can be interpreted as a "company code" in some cases, but in general it can mean a department for final data. Preliminary data we can represent by negative numbers instead.

A Department then conistes as a series of work places, where we can use a data flow-based description as we have it with "swim lanes". 

In business scenarios we may want to consider the sharding criteria as a  "company code", the smalest unit of an organization, which is able to make own decissionsin corporate law, but it can be anything, which allows seperating buckets of data, which define a collectiion of "Actors". We can also add booking periods, if we want to. And we clearly want to add workplaces as subdivisions of departments.

We make the assumption that there is a sharding code "0", which represents the control node of the cluster, and which knows about the meaning of the other shards and also about the objects maintained everywhere as a directory service for the cluster. All other may start with 100. THe control node then connects to a RAFT-based etcd database for fail-over management.

Every Shard is intended to have more than one replica, in production 3 is the minimum for every shard. We will have a roll-forward model, we do not do roll-backs in general regarding records. Only a transaction, which is replicated to a minumum set of at least two shards is considered to be complete.

All data are JSON-based data structures, which can be transformed into Go structs back an forth and and  API calls using Go's JSON Marshaler and the Go net/http interface. The is a service layer and a data access layer, while the service layer is very thin and only is managing the transformation of database data to network data and the routing of requests to the shards.

### Message-Based

Creating content for the active database cluster is achieved by sending messages to objects in the shards. Messages are insert, modify and cancel messages and we have an insert-only/append-only model regarding records. On the creation of the messages itself, we can have a more classical CRUD model with PUT and DELETE operations on the then preliminary data.

Messages transform into immutable Records and Records transform into materialized views for reading. 

Message delivery can be done via Kafka, but in the end any reliable message delivery protocol can be used.  We also can use Go's channels and go-routines. We are implementing here mostly the Actor model that MIT had once described. Since we anyhow need an outbox data structure and a receving a Journal data structure, so the transport method can be any.

Messages are JSON-based data structures and have some specific content as sender, receiver and a timely order which allows forming a queue for every Actor class.

The sharding code and the timely order together creates a Lamport time stamp for every message.

Message delivery is done to a Journal data structure. When the message is received it can create follow-up actions in a stable and consistent manner with snapshot consitency,


### Event-Driven

Every received messages represents an event, which is immutable and recorded. It represents history and has already happenend.

### Journal and Outbox

The Journal table records the events and brings them into a guaranteed timely order per Shard. The outbox records requests and notifications to be send to be send and delivered again in guaranteed timely order. The Journal always runs behint the outbox, we have a roll-forward model for every receiver, which we can represent using Vector-Time. 

### Processing
Every Journal entry can be subscribed to and can trigger further actions using an event-codition-action model. Actions create new entries to the outbox.  actions have been triggered and executed are maintained in a high-water mark table, which just follows the Journal using processing agents.

### High-Water Mark
To remember which follow-up actions have been taken yet, we maintain a high-water mark table, which represents a vector-clock implementation, It is a table which remembers which journal entry was executed already per sender.  It also makes sure, that there is only one vector-time update at a time. Follow up actions are triggered by agent jobs, which scan the database shards for pending actions and which can run as GO routines.

### Event-Condition-Action Rules

An action is a next step to be taken and it has to happen within the same logical transaction as the high-water mark entry is written. Since we have a transactional journal, we can defer actions as long there is no concurrency on the single actor and operations are in sequence.

### Agents, Active Processing and High-Water Mark
The active transformation is done via agent jobs, which wake up, if new messages are to be processed and which sleep again if there is nothing there to be done. We can represent the as go-routines.

### Service-API for Snapshot Consistent Data Acccess for Reading
The service A{I for reading needs to add a where clause included as a temporal cutoff. Temporal cutoffs depend on the object type to be be managed. There either is validity date represented by a ISO date or datatime or there is version number, whicch is represented by a major-minor-version scheme (like major-verion/minor-version/revision/build numbers as with software versioning.

### Transactional Guarantees, Fail-Fast/Fail-Over
Since we are recording history, there is no roll-back, there only is a roll-forward model. We always recover from replica and we only move forward in time.
Errors must be processded using exception handling and can create new workflows as any action can create further workflows.\

# Colors
## White Pages: Business Network: Companies
## Yellow Pages: Endpoints of Messages and Services, Swim Lanes: Work Places
## Blue Pages : Data Structures, Documents, Paperwork
## Red Tape : Rules for Validation and Decission Making

> 

