# Distributed systems for fun and profit
by Mikito Takada. If you like this notes, you can [get the
book](http://book.mixu.net/distsys/single-page.html).


### Chapter 1: Distributed systems at a high level
* Scalability - the ability of a system, network, or process to handle a growing
  amount of work in a capable manner or it's ability to be enlarged to
  acommodate that growth. Interesting scalability terms:
  a. Size scalability - e.g. adding more nodes should make the system lienarly
  faster, growing the dataset should not increase latency
  b. Geographic scalability
  c. Administrative scalability

A system can be scalable in two aspects.
* Performance
    * Latency
* Availability
    * Fault tolerance

But, we do not live in a perfect word. Distributed systems are constrained by
two physical factors
* Number of nodes
* Distance between nodes
This is were abstractions and models come into play.

**Design techniques**
* Partitioning - very application specific
    * Improves performance by limiting the amount of work to be done by a single
      node
    * Improves availability by allowing different partitions to fail
      independently
* Replication
    * Improves performance by making additional computing power and bandwidth
      available to a new copy of the data
    * Improves availability by creating additional copies of the data
    * Comes at a price - consistency model

### Chapter 2: Up and down the level of abstraction

Distribution - key property of distributed systems.

* Programs run concurrently on independent nodes
* Programs are connected by a network that may introduce nondeterminism and
  message loss
* Programs do not have shared memory or shared clock

This leads to implications.

**System model** - a set of assumptions about the environment and facilities on
which a distributed system is implemented, e.g.
* what capabilities the nodes have and how they may fail
* how communication links operate and how they may fail
* properties of the overall system, such as assumptions about time and order

**Nodes in a distributed system model**
* They have the ability to execute a program
* They have volatile and stable memory
* They have a clock
Additionally, there is a failure model which describes the ways in which nodes
can fail.

**Communication links in a distributed system model**
* They connect individual nodes to each other
* They allow messages to be sent in both directions
* They can be reliable or not
Failure in a communication link creates a network partition

**Time and ordering in a distributed system model**
* Synchronous model - not realistic
    * Process execute in lock-step
    * There is known upper bound on message transmission delay
    * Each process has an accurate clock
* Asynchronous model
    * No timing assumptions
    * No bound on message transmission delay
    * No useful clocks exist

This leads to **the consensus problem**. Several programs achieve consensus if
they agree on some value Formally:
* Agreement: every correct process must agree on the same value
* Integrity: every correct process decides at most one value, and if it decides
  some value, then the value must have been proposed by some process
* Termination: all processes eventually reach a decision
* Validity: if all correct processes propose the same value V, then all correct
  processes decide the same value V

**FLP** - under the asynchronous system model, where nodes can only fail by
crashing, network is reliable and there is no upper bound on message delay,
there does not exist a deterministic algorithm for the consensus problem.

**CAP theorem** - Of the following three properties
* Consistency - all nodes see the same data at the same time
* Availability - node failures do not prevent survivors to operate
* Partition tolerance - the system continues to operate despite message loss due
  to network and/or node failure
only two can hold simultaneously. This leads to several types of systems.
* CA - e.g. full strict quorum protocols
* CP - e.g. majority quorum protocols in which minority partitions are
  unavailable, such as Paxos
* AP - e.g. conflict resolution

But consistency and availability are not binary choices unless speaking about
strong consistency. Consistency is not a singular, unambiguous property - there
are different consistency models.

**Consistency model** - a contract between programmer and system, wherein the
system guarantees that if the programmer follows some specific rules, the
results of operations on the data store would be predictable.
* Strong consistency
    * Linearizable consistency
    * Sequential consistency
* Weak models
    * Client-centric
    * Causal consistency
    * Eventual consistency

### Chapter 3: Time and order

Time in a distributed system:
* Can define order across system
* Can define boundary conditions for algorithms

**Lamport clocks** - each process maintains a counter using the following rules:
* Whenever a process does work, increment the counter
* Whenever a process sends a message, include the counter
* Whenever a message is received, set the counter to `max(local_counter,
  received_counter) + 1`.

**Vector clocks** - each process maintains an array of N logical clocks using
the following rules:
* Whenever a process does work, increment the logical clock value of the node in
  the vector
* Whenever a process sends a message, include the whole vector
* When a message is received
    * Update each element in the vector to `max(local, received)`
    * Increment the logical clock value of the node in the vector

**Failure detectors** - can be characterized by two properties
* Completeness - easier to achieve
    * Strong completeness - every crashed process is eventually suspected by
      **every** correct process.
    * Weak completeness - every crashed process is eventually suspected by
      **some** correct process.
* Accuracy
    * Strong - no correct process is suspected ever
    * Weak - some correct process is never suspected
