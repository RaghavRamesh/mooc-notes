
# Cassandra for Developers - Pluralsight course by Paul O'Fallon

### Module 1 - What is Cassandra?

##### Brief history of Cassandra
- Distributed NoSQL database
- Originally developed in Facebook for storing messages in Inbox
- Based on two seminal papers - Google’s Bigtable and Amazon’s Dynamo
    - One of the authors of Dynamo paper is also the author of the Fb’s Cassandra paper
- Cassandra combines the distributed nature of Dynamo and the data model of Bigtable.
- Open-sourced in 2008. 2010 - became a top-level Apache project.
- Netflix famously migrated from Oracle to Cassandra
- Apple too - run 75000 nodes

##### Topology of a Cassandra cluster
- True master-less peer-to-peer system. All nodes are capable of performing all of Cassandra’s functions. 

Storing data in Cassandra
- All data stored in Cassandra is associated with a token and there are an astronomical number of valid values for tokens. 2^-63 to 2^63
- As new nodes are added, each node takes a contiguous range of token values and stores the data associated with it
- Challenges - what happens when a new node is added?
- This led to the birth of virtual nodes or v nodes. Each physical node splits its token range into its v nodes so that when a new node is added to the cluster, the tokens associated with one or more of the v nodes assigned to the new physical node. But the nodes no longer take ownership of a contiguous range of values. However, each v node does at a random spot in the total range of values.
- This allows Cassandra to run in heterogenous hardware.
- How do we know where the data is stored?
    - Partitioner!
    - Cassandra is a partitioned row store, which means that the data is read or written with a partition key.
    - When data is inserted, this partition key is hashed into a token. The token value determines which v node and actual physical node is responsible for the data. 

##### Demo
- Installed a Cassandra node in Docker for Mac and used `nodetool`
- Token - may refer to range of token values or individual token
- `nodetool ring`. Each row in the output refers to a v node. Last column refers to end of token range.
- When running a second node, need to pass in a parameter `-seeds` so that the node know how to bootstrap itself when joining a cluster - need to tell the new node the existing nodes in the cluster. This doesn’t mean Cassandra is not masterless. Any number of nodes can act as seed nodes. These nodes act as any other nodes in all other ways but also help in bootstrapping new nodes when they join the cluster.
- Run `nodetool status` again to check the status. This can be run on any of the nodes and will return the same result.
- Run `nodetool ring` and notice that there are 2 IPs on the left-most column. The token range is assigned randomly to the 2 nodes amongst 512 v nodes (256 each)
- Go into the second node and look for `seed_provider` in the `/data/conf/cassandra.yaml` file and notice the node info of the seed node used to bootstrap this node
- `nodetool` packs a punch. —help shows just how much there is to learn about Cassandra.

##### Snitch
- To gain an understanding of the environment, physical or virtual.
    - Default: SimpleSnitch - suitable for development for single DC
    - GossipingPropertyFileSnitch - uses a property file deployed with each node to describe that node’s location and then gossip this info to each other.
        - Tells when nodes are in different racks. 
        - Distinguish how DCs are different from each other.
    - PropertyFileSnitch - entire topology is described in a single property file
    - EC2Snitch - cloud providers have snitches of their own, mapping DCs and racks to their own proprietary constructs
    - EC2MultiRegionSnitch - equates DCs to AWS regions and racks to availability zones
    - GoogleCloudSnitch
    - CloudstackSnitch

##### Revisit Demo
- Run with dc and rack flags enabled and you’ll notice the tokens are distributed among the nodes placed in different racks
- Check `endpoint_snitch` in cassandra.yaml and you’ll notice the GossipingPropertyFileSnitch used
- `cassandra-rackdc.properties` = here is where the docker dc and rack cmdline properties are stored and made available to Cassandra
- Token range allocations are broken down by DCs

* * *

### Module 2 - Replication and Consistency

##### Replication Strategies and Tunable Consistency

Data in C is replicated across many nodes 

Cassandra terminology
- Keyspace
    - At the highest level, data in Cassandra is organised into keyspaces.
    - Closest analogy is an Oracle or MySQL table space
- Table
    - Within a keyspace, there are one or more tables
    - Conceptually a close match to its relational counterpart
- Partition
    - All data in C is associated with a partition key and that determines where the data is located in the cluster
    - All data in a partition are stored together
    - Primary interaction point when reading or writing data into Cassandra
- Row
    - Data within a partition may be depicted in one or more rows

##### Replication strategy
- The specifics of a replication strategy are picked up at the keyspace level
- Partition key is used to locate the first copy of the data stored in the cluster, from there the config present in the keyspace is used to determine where else the data is replicated in the cluster
- Replication Strategies - snitches and replication strategies work hand-in-hand
    - SimpleStrategy - for dev env or single DC clusters
    - NetworkTopologyStrategy
        - Not only specifying how many copies but also specifying which DCs should contain how many copies
        - Ideal for production environments with multiple DCs
        - Also uses its knowledge of racks to smartly distribute replicas of data within a DC as well

##### Demo: Replication Strategies
- Cassandra docker image comes with `cqlsh` command installed
- `nodetool describering &lt;keyspace-name&gt;`
    - Shows how data is stored in the nodes

##### Reads and Writes in Cassandra
- Although each node is capable of performing all the tasks, within the scope of a single read or write, certain node will perform certain functions
- Coordinator node: the node the client connects to when performing a write operation.
    - Using the data available to it, it will then determine which nodes should actually process the operation and communicate it to them. Those nodes are called the Replica nodes.
    - There will be cases where Coordinator node is also a Replica node.
- Tunable Consistency - Writes
    - One of the attributes that makes Cassandra unique in the world of distributed NoSQL databases is this notion of tunable consistency.
    - In a Cassandra cluster, nodes can become unresponsive for any number of reason - node may be down or just responding slowly to requests. The TC notion allows you to determine from how many nodes the Coordinator node will wait for ack of the write until responding to the client that the write was successful. Rather than setting this setting at a cluster, keyspace or a table level, Cassandra allows you to set this at each statement level.
    - Consistency levels
        - ONE =&gt; Coordinator responds to client after a successful write to one of the replica nodes. 
        - TWO
        - THREE
        - QUORUM =&gt; majority of the nodes have responded with ack of the write
        - ALL =&gt; all nodes have responded with ack of the write
        - ANY =&gt; if the write can make it to the cluster at all, even to the coordinator node, it’s considered a success
    - Hinted Handoff
        - If consistency level is QUORUM and one of the nodes was down, the write will appear as a success to the client. But Cassandra has to write to all the nodes responsible for storing the data regardless of the consistency level of the statement. 
        - Cassandra uses the strategy called Hinted Handoff - the data is written down on the Coordinator node and then the C node tries repeatedly to write to the replica node which previously unavailable. Eventually, the data is removed from the Coordinator node.
        - However, what will happen when one of the replica nodes is down, the Coordinator node is / goes down as well? Cassandra has a strategy for this at read time.
- Tunable Consistency - Reads
    - Consistency in reads works similar to writes.
    - The number of nodes the Coordinator will consult in an attempt to return the most current data to the client. It does this by getting the data from one node and a “digest” of the data from the other nodes. The exact number depends on the consistency level.
    - Tunable consistency allows the ability to specify how important it is to obtain the latest data in response to a query
    - Consistency levels
        - ONE - just the data alone is sufficient
        - TWO - a digest from another node along with the data is necessary to respond to the client
        - THREE 
        - QUORUM - data and digest from a majority of the nodes
        - ALL
    - Read Repair
        - Coordinator node died and a Hinted Handoff never processed. 
        - If consistency level is all, and C had died before writing the data to all the nodes, when read operation is received to the C node from the client, it reads digest from the node that was not having the latest data, then the digest will not match. Then the C node will read data from all the other nodes and writes that data to the node whose digest did not match. This is known as Read repair.
        - For quorum consistency level, only a percentage of the repairs are done during repair because of the overhead.
        - So it is recommended to periodically run the `nodetool repair` command on the nodes in the cluster to remove these inconsistencies if any without waiting for the data to be read
- Achieve strong consistency
    - Cassandra has a reputation of an eventually consistent database - which it achieves using the above notions of read repair, hinted handoff and unable consistency.
    - However, it is possible to achieve strong consistency with Cassandra - there is the following magic formula
        - (Write consistency + read consistency) > Replication factor
        - If above, then we have strong consistency
- Consistency with multiple data centers
    - 3 additional levels of consistency that can be used
        - EACH_QUORUM - quorum of writes succeed in each DC prior to returning success
        - LOCAL_QUORUM - returns success once quorum has been achieved in the DC in which the Coordinator is located. These are important distinctions from a QUORUM consistency where there is no rule on which DC the nodes are located. Which may mean that your caller may be waiting for a write to be completed without realising it
        - LOCAL_ONE - same as ONE but only attempts to read from a node where the Coordinator is located

* * *