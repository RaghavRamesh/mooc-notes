# Understanding Sharding in Elasticsearch by Brad Quarry, elastic.co

## Agenda
1. Quick - What is ES?
2. What are shards?
3. Why do we need shards?
4. What is in a shard?
5. Optimal shard size
6. Optimal shard number
7. How do you manage shards?

## What is Elastic Stack?

Elasticsearch: highly available document store, search and analyse
Kibana: visualisation engine
Beats and Logstash: to get data into the system

## What are shards?
Equal pieces of the overall data in a single index
If you start with 100 GB of documents, ES will split them equally into data nodes as shards.
### Example
* transactions_index = 3M documents
* When we use Consistent Hashing (assign a hash to each document) to spread data evenly between all shards. This enables horizontal scaling:
    * ES cluster containing 3 data nodes
    * Data Node 1 Shard 1: 1M docs
    * Data Node 2 Shard 2: 1M
    * Data Node 3 Shard 3: 1M

## Why do we need shards?
They enable the use of multiple nodes and horizontal scaling to process data and improve performance

### Example by contrast
1. No shards | Vertical scaling, one big node
    1. Exponentially more expensive as you get bigger.
        1. Data Node 1: 4 CPU 16G RAM -> 8 CPU 32 G RAM -> 16 CPU 64 G -> ...
    2. Eventually you can’t get bigger
2. Shards | Horizontal scaling, many nodes
    1. Cheaper over time as the node type ages
    2. Add up to hundreds of nodes for incremental performance
        1. Data Node 1: 16 CPU 64G RAM
        2. Data Node 2: same
        3. ...

## Can I mix and match node sizes in a cluster?
* Yes you can but it won’t necessarily speed up your cluster
* All nodes must respond before a query completes, therefore you are only as fast as your slowest node. The fast nodes will complete work, but the user will still wait for the slow nodes before the query is 100% complete.
* Even if you hash specific customer data to specific fast nodes, other customers will be on slower nodes.

## What is in a shard?
A shard is an individual instance of Apache Lucene
* A piece of the data in the overall index stored incrementally in segments
* Indexes for each field that speed up querying
* Index and document metadata

## What happens when nodes and shards fail?
Using Shard Replicas to avoid data loss and increase query throughput

_look for image in Evernote_

Shard replicas are never stored in the same node as the primary shard.
* Replicas are an index level setting and the typical value is 1
    * Each replica makes a full copy of the entire index in the replicated shards
* Each replica is a READ replica and will automatically be used for queries
* When a primary fails, the replica is promoted to Primary and a new Replica is regenerated on another node
* Primary and Replica can never live on the same node
* Queries will always be sent to the node that is least utilised

## Is there an optimal shard size?
Shard size best practices:
* Recommended shard size is between 20-60GB based on field experience of consultants
* Shard size is not a setting, it’s total data in GB/shard count and evolves as you load more data
* A search on a field must scan the entire index for that field to return results
    * As a result larger shards take longer to scan
    * Exception to this rules is you sort by a field that is used in your queries, like a timestamp
        * In this case the query will be able to immediately find the data of interest and reduce scanning significantly
    * The more data you have per node the longer recovery times take
* As of ES7.6 the recommended storage per node with 31GB heap is 3-5TB on average with max of 10TB
* As of ES7.7 the recommended storage per node with 31GB heap is 3-5TB on average with max of 50TB

## Is there an optimal shard count?
* Shards use a default amount of HEAP memory even if empty
    * If you have 1000s of shards per node you will be using all of your RAM for shards
        * Oversharding is the most common recurring problem for inexperienced users
* You should have at least 1 shard per data node to maximise performance, including replicas
    * Increase starting shard count only as required as data grows
    * Example: 5 TB of data in an index @30GB/shard  = 166 shards to start
    * If you have 10 nodes that ~ 17 shards per node which is ok.
* More shards can increase indexing throughput, but also slows down searches since there are more parallel tasks to manage for each search
    * It’s a balance

## How do you manage shards?
* Besides the count and size, you generally don't
* Catalog style index - only one index
    * Storing non-time series search data like a product catalog or semi-structured text for document search
    * Option: overshard to start as more nodes are added shards will balance to the new nodes
    * Option: start an optimal number of shard and use the split and shrink apis which will double or halve the shard count every time it’s used
* Time series style index: many indices over time typically weekly
    * Storing immutable infrastructure log data in a set of time series indices
    * Could be infrastructure logs or customer transaction data
    * Can use index lifecycle management and snapshot lifecycle management policies
    * Every time a new weekly index is created the shard count can change
* When you add nodes ES will re-balance existing shards on to the new nodes and improve performance
* Heap usage will reduce as shards are thinned out across the new nodes
* ES will do a good job of merging data segments as needed at runtime
    * However, you can use Force Merge to optimally merge segments and improve heap usage on read-only indices
        * Merges many data segments in an index to one segment reducing segment pointers in memory

---
