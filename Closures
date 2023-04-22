# Basic Api

to be able to use GO closures effectively and without generating code, we make the assumption,
that a basic URL for an Actor object has several parameters, which result in different return values for the insert/read/cancel funtion on every opbject.

We have a set of APIs and obejects to operate on.

## Workitems

## Requests and Notifications

## Records and Journal

## Master Data Versions and Ledgers

## Inventory

## Statements

## Planing


We make a difference in the API if we rad meta data about the structure, access an Actor object instance and have differen operations - all in one API definition.

Every Actor object instance is given by a spatial and temporal coordinate - a Lamport time.

The Spatial coordinate is gven by the database shard and may have subdivisions.
Shards may mean departments, subdivisions my mean workfplace in departments.

The temporal coordinate is gven by a sequence number per an pbject instance. The sequence number defines a version number is is guaranteed to be increasing over time.

We add an epoche number, which allows failover of the sequence to an new sequence - either with fail-over or with sequence number overflow.

Sequence number of "0" means retreival of meta data of the class, not the instance instead.

Exammple on the basis of an URL of an Actor


or a Go data structure as a mixin struct



Both are consiered to be equivalent.
