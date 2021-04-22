# Basics of Kafka

## Theory

Covering: 
- Understanding Topics 
- Partitions and 
- Offsets etc

### Topics

- Particular Stream of Data
- Category/feed name to which records are stored and published
- Topic is identified by name

- Topics are split in partitions
    - Each partition is ordered
    - Each message within a partition gets an incremental id, called offset
  
- After data is written to a partition, it can't be changed (immutability)
- Data is kept in a topic for a limited amount of time (default 1 week)
    
Example: 

![Kafka Diagram - Basic](./assests/kafka-1.png)
Credit: [CloudKarafka](https://www.cloudkarafka.com/blog/part1-kafka-for-beginners-what-is-apache-kafka.html)

#### Offsets

- Offset is a position within a partition for the next message to be sent to a consumer
- Two types: Current Offset and Committed Offset
- Current Offset:
  - Simple integer number showing a pointer to the last record that Kafka has sent to a consumer
- Committed Offset:
  - The position that a consumer has confirmed about processing
  - Done so that a particular consumer does not consume the same data that it has already processed beforehand
  - Concept is important to partition re-balancing

#### Topic Replication Factor

We are dealing with disturbed systems. Need a way for topics in one broker to replicate in case that broker goes down.

- Replication factor should be > 1 (usually between 2 and 3)
  - Replication factor of 2 means you have 2 copies of the data (one copy in 2 brokers)
  - For resiliency, need replication factor of 3 (or 3 copies of the same data)
- If a broker goes down, another broker can serve the data

### Broker

- A Kafka cluster is composed of multiple brokers (Servers)
- Each broker is identified with its ID (integer)
- Each broker contains certain topic partitions
- After connecting to any broker (called a bootstrap broker), you will be connected to the entire cluster

### Leader for a Partition

- At any time there is only one leader of a partition
  - If there is a topic replication factor greater than 1, then the leader broker for that partition will be the only one that receives and serve data for that partition in the topic
  - Other brokers will synchronize the data
- Each partition has one leader and multiple ISA (in-sync replica) when the replication factor is greater than one

**What happens when there the lead broker goes down**
- There is an election to decide which broker will be the new leader for that partition
- Once the failed broker comes back online, it will try to assume leadership if possible

