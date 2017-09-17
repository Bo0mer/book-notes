## Distributed Systems - principles and paradigms (3rd Edition)
by Andrew S. Tanenbaum, Maarten van Steen. If you like this notes, you can
[buy the book](https://www.distributed-systems.net/index.php/books/distributed-systems-3rd-edition-2017/)

### Chapter 1: Introduction

1. **What is a distributed system?**
> A distributed system is a collection of autonomus computing elements that
> appear to its users as a single coherent system.

2. **Design goals**

A distributed system should
* Support resource sharing - it should be easy for users and applications to
  access and share remote resources.
* Make distribution transperant - it should hide the fact that its processes and
  resources are physically distributed across multiple computers possibly
  separated by large distances. There are different types of distribution
  transperancy:
  * Access - hide differences in data representation and how the object is
    accessed
  * Location - hide where an object is located
  * Relocation - hide that an object may be moved to another location while in
    use
  * Migration - hide that an object may move to another location
  * Replication - hide that an object is replicated
  * Concurrency - hide that an object may be shared by several independent users
  * Failure - hide the failure and recovery of an object
* Be open - the system should offer components that can easily be used by, or
  integrated into other systems
* Be scalable - there are different types of scalability:
  * Size scalability - the ability to add more users and resources to the system
    without noticeable loss of performance
  * Geographical scalability - the users and resources may lie far apart, but
    the fact that communication delays may be significant is hardly noticed
  * Administrative scalability - can still be easily managed even after it spans
    multiple independent administrative organizations
