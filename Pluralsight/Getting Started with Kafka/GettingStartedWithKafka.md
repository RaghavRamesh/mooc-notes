Producers and Consumers are decoupled by the topic construct. They don’t need to be aware of each other, they only need to know the topic to produce to and consume from. 

Broker

- Software process / executable / daemon that runs on a machine. 

- Synonym is "server"

- Broker has access to resources on the machine like file system to store messages categorised by topics

- Like any .exe you can run more than one on a machine, but each should have a unique configuration so that they don’t conflict with one another

- Here’s where the difference between Kafka and other messaging systems become apparent

- How Kafka distributes the messages is what helps in supporting high throughput. 

    - Can scale out brokers horizontally as much as needed without modifying producers or consumers. 

Cluster

- Grouping of brokers

- Can have single broker or multiple brokers in a single machine

- 1 broker 1 machine =&gt; cluster of 1

- 2 brokers 1 machine =&gt; cluster of 2. Also same for 2 brokers in 2 machines

- In the end, it doesn’t matter where brokers are running. What matters is how independent brokers are grouped to form a cluster. The grouping mechanism that determines a cluster's membership of Brokers is an important part of Kafka’s architecture which is what enables it to scale in a fault-tolerant way. This is where Apache Zookeeper comes into play.

Distributed systems

- System: collection of resources that are instructed to achieve a specific goal or function

- DS: consists of multiple workers or nodes that have a specific goal 

    - But they need to coordinate to ensure consistency and progress towards a common goal with most optimal use of resources

- In Kafka these worker nodes are the Brokers

- Within a DS, there are different roles and responsibilities. There’s generally a hierarchy that starts with a supervisor (worker node elected by its peers). Usually the supervisor is the one that has been around the longest. 

- Controller responsibilities

    - Maintain attendance / inventory of workers available for work

    - Maintain a list of work items

    - Maintain active status of workers and status of tasks

    - In Kafka, this team is the Cluster and its members Brokers. There’s a C within the cluster. 

- Reliable work distribution

    - Worker availability and health

    - Task redundancy - what level of redundancy to apply to ensure reliability

    - For an assignment, if a controller determines redundancy is required, it will promote a worker into a leader which will take direct ownership fo the task assigned. It will be the leader’s job to recruit 2 of its workers to take part in the replication.

    - In Kafka, the risk factor to protect against loss is known as the replication factor

    - Once peers have committed to the leader, a quorum is formed and the committed peers take on a new role in relation to a leader - follower. If leader cannot get quorum, controller will reassign to another leader that can.

    - In Kafka, work refers to receiving messages, categorising them into topics and reliably persisting them for eventual retrieval

- Communication and consensus

    - Gossip protocol. 

    - Worker node membership and naming

    - Configuration management

    - Leader election

    - Health status updates

    - Meet Apache Zookeeper

        - Centralised service for maintaining metadata about a cluster of distributed nodes
            - Configuration info

            - Health status

            - Group membership

        - Hadoop, HBase, Redis, etc. also depend on Zookeeper for scaling

        - ZK is also a distributed system that consists of multiple nodes called an “ensemble"

Installation setup

- Depends on

    - Linux

    - Java 

    - Scala

Topic

- Topics can scale an entire set of brokers for scalability

- Consumers want to consume from a topic regardless of where it is

- Once events enter a topic, they cannot be changed

    - Only way to correct a wrong event is to follow up with another event that corrects it and it’s up to the consumer to reconcile how to ignore the wrong event and use the correct event - event sourcing

- Message. Each message has a

    - Timestamp

    - Referenceable identifier (can be referenced by consumers)

    - Payload (binary)

- Consumers should have a clear operational boundary with other consumers so that something that affects one consumer doesn’t affect the rest - for e.g., erroneous processing in one consumer or a consumer crash shouldn’t affect the other consumers

Message offset (bookmark / placeholder)

- This is how consumers can read messages at their own pace

- Last read position

- Entirely established and maintained by the consumer. It needs to keep track of what has been read and what hasn't

- Corresponds to a message id

- Can stay connected when no more messages are there in topic and then when more messages arrive, it can continue to process. All of this possible by making use of the message offset

Message retention policy

- Kafka is immune to slow consumption because it has the means to retain messages over  a long period of time - Message retention policy. Configurable. Default: 7 days = 168 hours. Beyond that message start falling off. 

- All messages are retained by the Kafka clusters regardless of whether a single consumer has consumed the message

- Retention period is set per-topic

- Physical storage can constrain retention

Underlying basis

- Commit log / event sourcing

    - Append-only

    - Ordered sequence (by time)

    - Immutable

- Similar to db commit log which has the following properties:

    - Source of truth

    - Physically stored and maintained

    - Higher-order data structures derive from the log

        - Tables, indexes, views, etc. which are just projections from the authoritative source

    - Point of recovering (restoring by replaying events)

    - Basis for replication and distribution

- Apache Kafka is publish-subscribe messaging rethought as a distributed commit log

Partitions - how topics are implemented in the physical world and how within this physical realm, Kafka is able to operate and deliver upon its amazing promise

- Each topic has one or more partitions - entirely configurable

    - Scale

    - Become fault-tolerant

    - Achieve higher levels of overall throughput

- Each partition is maintained in at least one or more brokers

- At minimum each topic has to have 1 partition. 

    - Has a corresponding folder in file system

    - There are legit reasons to use a single partition for a topic. 

- Cannot split a single partition across multiple machines

    - Which means if only one partition for a topic, you’ll be limited by the broker’s node’s ability to fit all the messages within it not to mention the I/O operations that it has to perform

- Multiple partitions

    - =&gt; 1 topic split into 3 log files ideally managed by different broker nodes

    - =&gt; burden shared among partitions. But the manner in which they are managed is the same i.e., time ordered sequence

    - How is it split amongst the partitions? Based on partitioning scheme established by the producer. 

Consumers are responsible for reconciling order of messages consumed from different partitions of topic

Partitioning trade-offs

- More partitions =&gt; more entries for ZK to keep track. Limited by memory that ZK can operate

    - Ensure proper ZK capacity for large partition numbers

- Message ordering can become complex

    - Consider single partition for global ordering. =&gt; limited in terms of scalability

    - Alternatively, consumer-handling for ordering

- The more partitions the longer the leader fail-over time. Usually low ms but for larger clusters with large partitions this can quickly add up

Fault-tolerance

- Broker failure

- Network issue

- Disk failure

- Solved by configuring replication factor.

    - Reliable work distribution

        - Redundancy of messages

        - Cluster resiliency

        - Fault-tolerance

    - Guarantees

        - N-1 broker failure tolerance

        - 2 or 3 minimum

    - Configured on a per-topic basis

If in-sync replicas (ISRs) and quorum is same =&gt; healthy state