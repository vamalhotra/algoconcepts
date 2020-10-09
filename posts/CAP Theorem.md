---
layout: post
---
# CAP Theorem

CAP theorem states that no distributed storage system can guarantee (high) **C**onsistency, (high) **A**vailability and (low) **L**atency. where's P? **P** is partition tolerance which impacts latency in serving request when either partition goes down or data partitioning is needed to handle increased size. 

## Consistency

Consistency stands for freshness of data - say a write happened 5 ms ago followed by a read. Then read should get the latest value and not the stale value. 

## Availability

Availability is the ability of system to serve requests when any node in the cluster goes down.

## Latency

Latency is the response time. You can make the reader/writers wait infinitely while your system recovers. You can claim high availability but due to high latency, it's a fail. That's why availability is mostly measured with some latency threshold.

## Partition Tolerance

When communication between two nodes in the cluster goes down, system is still able to function. Note that if this happens, then we have to sacrifice one of the other two - you can either serve stale reads by compromising consistency or you can sacrifice availability and do not send response until the communication is established. If you have both consistency and availability, then in events of change in primary replica, your requests will wait until switch is completed.

## Always write to primary

In a distributed storage system, data is stored on multiple machines and each copy of data has multiple replicas kept on different machine. When writing, primary/Master replica is updated first. All writes are committed to memory and flushed to disk. Disk can be RAM itself or SSD or HDD. Usually, writing to primary is not a choice but in-built into system.

## Make a choice to read either from primary or any replica

When reading, you can either choose to read from any replica which will guarantee you low latency and high availability but it will compromise on consistency by providing stale data because it may not necessarily read from primary replica and sync has not yet happened between primary and secondary replica. Or you can choose to read from primary replica which will guarantee you high consistency but in the event of failure of replica or during data partitioning, all the writes and read-from-primary will be queued which will impact availability and latency.

### References:

1. [StackOverflow answer on CAP](https://stackoverflow.com/questions/12346326/cap-theorem-availability-and-partition-tolerance)
2. [ACID property](https://en.wikipedia.org/wiki/ACID) for Sql databases

