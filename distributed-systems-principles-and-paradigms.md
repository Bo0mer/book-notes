## Distributed Systems - principles and paradigms
by Andrew S. Tanenbaum, Maarten van Steen. If you like this notes, you can
[buy the book](https://www.amazon.com/Distributed-Systems-Principles-Andrew-Tanenbaum/dp/153028175X).

### Chapter 2: Architectures

1. Architectural styles
  * Layered architecture
  * Object-based architecture
  * Data-centered architecture
  * Event-based architecture
1. System architectures
  * Centralized - multi-tier apps. Said to be vertically distributed (logically different components reside on different tiers).
  * Decentralized - said to be horizontally distributed (logically equivalent components reside on different machines).
  * Self-managing - organized as feedback control loops. Merge ideas from system and software architectures.

### Chapter 3: Processes
1. Clients
  * Network user interfaces
  * Client storing data (stateful)
  * Thin clients (not storing data, stateless)
2. Servers

### Chapter 4: Network communication
* Protocols, OSI layers
  1. Physical - transport data onto the wire
  1. Data link - handle error checking
  1. Network layer - handle routing
  1. Transport layer - connection/less
  1. Network middleware
    * Authentication
    * Authorization
    * Commit protocols
    * Distributed locking (of resources)
  1. Application protocol
* RPC communication
* Message oriented communication
  * Message passing interfaces
  * Message oriented middleware (message queues)
* Stream oriented communication
* Multicast communication
  * Gossip-based data dissemination
  * Epidemic dissemination
  * Combined dissemination

### Chapter 5: Naming

1. Types of naming
  * Names - human friendly (e.g. github.com)
  * Identifiers - machine only
  * Addresses - machine only (e.g. http://192.30.253.112:443)
1. Flat naming - an entity is assigned a single address (e.g. ethernet addresses). Locating an entity could be done via:
  * Broadcast/multicast (e.g. ARP) - does not scale well.
  * Forwarding pointers - when an entity moves, it leaves behind a reference to its new location. Also does not scale well. Other problem is broken links.
  * Home-based approaches (like in mobile IP) - Problems arise if the party moves to completely different site, but the home address stays the same.
  * Distributed hash tables
  * Hierarchical approaches. (e.g. DNS)
1. Structured naming
  * Name spaces - labeled directed graph. Edge labels being a parts of the whole name (e.g. file systems, domain names). Sample implementation - DNS. Organized into 3 layers - global, administrative, managerial. Different requirements for different levels. Name resolution algorithms - iterative or recursive.
1. Attribute-based naming (e.g. LDAP)

### Chapter 6: Synchronization
* Clock synchronization
  1. Hardware clocks
    * GPS system provides means for HW clock synchronization
    * NTP - either poll a time server or receive updates from such (pull or push). The synchronized time may not be real, but it would be the same for all machines. Useful when multihop communication is easily achievable.
    * Reference broadcast synchronization - used for wireless networks consisting of sensors or devices that are constrained in terms of power supply or communication abilities.
  1. Logical clocks
    * Lamport clocks
    * Vector clocks - catch causality.
* Mutual exclusion
  * Centralized approach - one manager of permissions
  * Decentralized approach - relies mostly on probability
  * Distributed approach
  * Token ring
* Election algorithms
  * The bully algorithm - the biggest wins (e.g. the process with the highest process ID number is selected as the leader)
  * Ring algorithm

### Chapter 7: Consistency and replication
* There are two main reasons for replication
  * Reliability - e.g. to be able to survive a power outage.
  * Performance - e.g. for clients that are far away.
* Leads to consistency problems - how to keep data across replicas consistent? There are several consistency models.
* Data-centric consistency models - a contract between the software (processes) and memory implementation (data store). This model guarantees that if the software follows certain rules, the memory works correctly.
  * Continuous consistency - models that bound staleness deviations and/or numerical deviations. E.g. using conits, define some continuous measurement for the deviation between replicas and some rules that tell the nodes when to update based on that metric.
  * Consistent Ordering of Operations - models that deal with the order of operations on shared replicated data in order to provide consistency. In this models, all replicas must agree on a consistent global ordering of updates.
    * Sequential consistency - The result of any execution is the same as if the (read and write) operations by all processes on the data store were executed in some sequential order and the operations of each individual process appear in this sequence in the order specified by its program.
    * Causal consistency - Writes that are potentially causally related must be seen by all processes in the same order. Concurrent writes may be seen in a different order on different machines.
    * Grouping operations -
* Client-centric Consistency Models - In essence, client-centric
consistency provides guarantees for a single client concerning the consistency of accesses to a data store by that client. No guarantees are given concerning concurrent accesses by different clients.
  * Eventual consistency -  if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value.
  * Monotonic read consistency - If a process reads a value of a data item _x_, any successive read operations on _x_ by that process will always return that same value or a more recent value.
  * Monotonic write consistency - A write operation by a process on a data item _x_ is completed before any successive write operations on _x_ by the same process.
  * Read your writes consistency - The effect of a write operation by a process on data item _x_ will always be seen by a successive read operation on _x_ by the same process.
  * Writes follow reads consistency - A write operation by a process on a data item _x_ following a previous read operation on _x_ by the same process is guaranteed to take place on the same or a more recent value of _x_ that was read.
* Replica management
  * Replica-server placement
  * Replication content placement - three different types of replicas - permanent replicas, server-initiated replicas, and client-initiated replicas
    * Permanent replicas - the initial set of replicas, often small in number
    * Server-initiated replicas - exist to enhance performance and/or reliability. They are created at the initiative of the data store.
    * Client-initiated replicas (or client caches) - local storage facility that is used to temporary store a copy of the data fetched from a server.
  * Content distribution - managing propagation of updated content.
    * State vs Operations - whether to propagate only the state or the update operations itself. There are three main concepts:
      1. Propagate only a notification of update (invalidation protocol)
      1. Propagate data from one copy to another
      1. Propagate the update operations to other copies
    * Pull vs Push
      1. Pull - clients pulls updates for replicas
      1. Push - server pushes updates to replicas
* Consistency protocols
  * Primary based protocols - each data item _x_ in the data store has an associated primary server which is responsible for writes on _x_.
  * Replicated write protocols - write operations are carried on multiple replicas. There are two categories - if write operations are forwarded to all replicas, or the protocol is based on majority voting.
    * Active replication - write operations are sent to each replica. Does not scale well.
    * Quorum-based replication - a replica must agree with majority of other replicas before doing an operation.
  * Cache-coherence protocols
