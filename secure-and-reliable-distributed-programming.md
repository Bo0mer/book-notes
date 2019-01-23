## Chapter 1

Distributed programming abstractions



*   System model
    *   Processes
    *   Links
*   Agreement problem

Inherent distribution



*   Information dissemination
    *   Publish-subscribe
    *   Best-effort broadcast
    *   Reliable broadcast
*   Process control
    *   Consensus
*   Cooperative work
*   Distributed databases
    *   Atomic commit
*   Distributed storage


## Chapter 2: Basic abstractions



*   Safety - the algorithm does not do anything bad
*   Liveness - Eventually something good happens

Combining safety and liveness is the challenge. If a specification lacks either, it is doing it wrong



*   Process failures
    *   **Crash-stop** - a process stops executing steps. It might recover for later computation, but the algorithm does not depend on that.
    *   **Omissions** - a process does not send (or receive) a message that it is supposed to according to the algorithm
    *   **Crash-recovery** - a process stops executing steps, and might recover later. In such model, if a process crashes and recovers a finite number of times it is considered correct. Processes should know whether they have crashed, therefor a stable memory is required. Other solution is to assume that some of the processes never crash (during the lifetime of the algorithm execution).
    *   **Arbitrary faults**
*   Cryptographic primitives
    *   Hash functions
    *   MACs - authenticates data between two entities
    *   Digital signatures - provides data authentication in systems with multiple entities that need not share any information beforehand
*   Abstracting communication
    *   Link - every pair of process is connected by a bi-directional link
    *   **Fair-loss links** - message is _guaranteed_ to be delivered by its target only if it is sent infinitely often. Properties:
        *   _Fair-loss: _if a correct process _p_ infinitely often sends a message _m _to a correct process _q_, then _q_ delivers _m _an infinite number of times.
        *   _Finite duplication_: if a correct process _p_ sends a message _m_ a finite number of times to process _q_, then _m_ can not be delivered an infinite times by _q_.
        *   _No creation_: if some process _q_ delivers a message _m_ with sender _p_, then _m_ was previously sent to _q_ by process _p_.
    *   **Stubborn links** - implemented atop fair-loss link and adds message retransmission
        *   _Stubborn delivery_ - if a correct process _p_ send a message _m_ once to a correct process _q_, then _q_ delivers _m_ an infinite number of times
        *   _No creation_: if some process _q_ delivers a message _m_ with sender _p_, then _m_ was previously sent to _q_ by process _p_.
    *   **Perfect links** - also called reliable links. Adds mechanism for detecting and suppressing message duplicates. Works with **crash-stop** processes.
        *   Reliable delivery - If a correct process _p_ sends a message _m_ to a correct process _q_, then, then _q_ eventually delivers _m_.
        *   No duplication - No message is delivered by a process more than once.
        *   No creation - If some process _q_ delivers a message _m_ with sender _p_, then _m_ was previously sent to _q_ by process _p_.
    *   **Logged perfect links** - operates with **crash-recovery** processes - keeps state in non-volatile memory, so it is preserved upon recovery
        *   Reliable delivery - If a correct process _p_ sends a message _m_ to a correct process _q_, then, then _q_ eventually log-delivers _m_.
        *   No duplication - No message is delivered by a process more than once.
        *   No creation - If some process _q_ delivers a message _m_ with sender _p_, then _m_ was previously sent to _q_ by process _p_.
    *   **Authenticated perfect links**
*   Timing assumptions
    *   **_Asynchronous system_** - no assumptions about processes and links. Processes do not have access to a physical clock nor there is an upper bound on processing or communication delay.
    *   **Synchronous system** - has the following properties
        *   Synchronous computation - There is a known upper bound on processing delays. Each step execution is time-bounded. Step execution consists of message delivery, computation and sending a message to other process.
        *   Synchronous communication - There is a known upper bound on message transmission delays.
        *   Synchronous physical clocks - Every process is equipped with a local physical clock, which has bounded deviation from a global real-time clock.

		In a synchronous system, several useful services could be implemented:



        *   Timed failure detection - every crash of process may be detected within bounded time
        *   Measure of transit delays
        *   Coordination based on time
        *   Worst-time performance - it is possible to derive the worst-time response times
        *   Synchronized clocks - it is possible to synchronize the clocks of multiple processes in such a way that they're never apart more than some known constant
    *   **Partial synchrony** - timing assumptions only hold eventually, without stating when exactly - this means there is a time after which these assumptions holds forever, but this time is not known a priori.
*   Abstracting time
    *   Failure detection
        *   Arbitrary-faulty processes - possible in theory, but inherently hard to implement
        *   **Perfect failure detector** - assuming **crash-stop** process abstraction, crashes could be accurately detected using timeouts.
            *   Strong completeness - eventually, every process that crashes is permanently detected by every correct process.
            *   Strong accuracy - if a process _p_ is detected by any process, then _p_ has crashed.
        *   **Leader election -** assuming **crash-stop** process abstraction; it can not be formulated for crash-recovery and arbitrary-fault process abstractions.
            *   Eventual detection - either there is no correct process, or some correct process is eventually elected as the leader.
            *   Accuracy - if a process is leader, then all previously elected leaders have crashed.
        *   **Eventually perfect failure detector** - detects crashes accurately after some a priori unknown point in time, but may make mistakes before that time. In such case we take about failure _suspicion_, not failure detection.
            *   Strong completeness - eventually, every process that crashes is permanently _suspected_ by every correct process.
            *   Eventual strong accuracy - eventually, no correct process is suspected by any correct process.
        *   **Eventual leader election - **ensures the uniqueness of the leader only eventually. Can be implemented with a **crash-stop** or a **crash-recovery** process abstractions.
            *   Eventual accuracy - there is a time after which every correct process trusts some correct process.
            *   Eventual agreement - there is a time after which no two correct processes trust different correct processes.
        *   **Byzantine leader election**
            *   Eventual succession - if more than _f_ correct processes that trust some process _p_ complain about _p_, then every correct process eventually trusts a different process
            *   Putsch resistance - a correct process does not trust a new leader unless at least one correct process has complained against the previous leader
            *   Eventual agreement - there is a time after which no two correct processes trust different processes
*   **Distributed-system model** - combination of a process abstraction, a link abstraction and possibly a failure-detector abstraction
    *   **Fail-stop** - crash-stop processes, perfect links and perfect failure detector
    *   **Fail-noisy** - crash-stop processes, perfect links and eventually perfect failure detector of eventual leader detector
    *   **Fail-silent **- crash-stop processes, perfect links, and no failure detector
    *   **Fail-recovery** - crash-recovery processes, stubborn links, and eventual leader detector
    *   **Fail-arbitrary **- fail-arbitrary processes, authenticated perfect links
    *   **Randomized** - orthogonal to all other models, algorithms are not necessarily deterministic

<table>
  <tr>
   <td>
Model Name
   </td>
   <td>Process Abstraction
   </td>
   <td>Link Abstraction
   </td>
   <td>Failure/Leader detector
   </td>
  </tr>
  <tr>
   <td>Fail-stop
   </td>
   <td>Crash-stop
   </td>
   <td>Perfect links
   </td>
   <td>Perfect FD
   </td>
  </tr>
  <tr>
   <td>Fail-noisy
   </td>
   <td>Crash-stop
   </td>
   <td>Perfect links
   </td>
   <td>Eventually perfect FD
   </td>
  </tr>
  <tr>
   <td>Fail-silent
   </td>
   <td>Crash-stop
   </td>
   <td>Perfect links
   </td>
   <td>n/a
   </td>
  </tr>
  <tr>
   <td>Fail-recovery
   </td>
   <td>Crash-recovery
   </td>
   <td>Stubborn links
   </td>
   <td>Eventual LD
   </td>
  </tr>
  <tr>
   <td>Fail-arbitrary
   </td>
   <td>Fail-arbitrary
   </td>
   <td>Authenticated perfect links
   </td>
   <td>depends
   </td>
  </tr>
</table>



## Chapter 3: Reliable Broadcast

A broadcast abstraction enables a process to send a message, in a one-shot operation,

to all processes in a system, including itself.



*   **Best-effort broadcast** - ensures the delivery of messages as long as the sender does not fail
    *   Validity - if a correct process broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - no message is delivered more than once
    *   No creation - if a process delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
*   **Regular reliable broadcast** - ensures agreement even when the sender fails
    *   Validity - if a correct process broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - no message is delivered more than once
    *   No creation - if a process delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
    *   **Agreement** - if a message _m_ is delivered by some correct process, then _m_ is eventually delivered by every correct process
*   **Uniform reliable broadcast** - guarantees that the set of messages delivered by faulty processes is always a subset of the messages delivered by correct processes (**intuition**: if a failed process delivers, everyone must deliver)
    *   Validity - if a correct process broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - no message is delivered more than once
    *   No creation - if a process delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
    *   **Uniform agreement** - if a message _m_ is delivered by some process (whether correct or faulty), then _m_ is eventually delivered by every correct process.

    Algorithms implementing Uniform Reliable broadcast

*   All-ack uniform reliable broadcast - implements URB in the fail-stop model. A process delivers a message only when it knows that the message has been BEB-delivered and thereby seen by all correct processes
*   Majority-ack uniform reliable broadcast - implements URB in the fail-silent model. Similar to all-ack, except that processes do not wait until all correct processes have seen a message, but only until a majority quorum has seen and retransmitted the message
*   **Stubborn broadcast **- delivers every message that is broadcast by a correct process an infinite number of times
    *   Best-effort validity - if a process that never crashes broadcasts a message _m_, then every correct process delivers _m_ an infinite number of times
    *   No creation - if a process delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
*   **Logged best-effort broadcast **- operates in the fail-recovery system model and relies on logging
    *   Validity - if a process that never crashes broadcasts a message _m_, then every correct process eventually log-delivers _m_
    *   No duplication - no message is log-delivered more than once
    *   No creation -  if a process log-delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
*   **Logged uniform reliable broadcast - **operates in the fail-recovery system model. If a process log-delivers a message, all correct processes should eventually log-deliver that message
    *   Validity - if a process that never crashes broadcasts a message _m_, then every correct process eventually log-delivers _m_
    *   No duplication - no message is log-delivered more than once
    *   No creation -  if a process log-delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
    *   **Uniform agreement **- If a message _m_ is log-delivered by some process (whether correct or faulty), then _m_ is eventually log-delivered by every correct process

**Probabilistic broadcast** -  behavior is partially determined by a controlled random experiment. Often needed because reliable broadcast is does not scale well.



*   **Epidemic dissemination**
    *   Probabilistic validity - there is a possible value ε, such that when a correct process broadcasts a message _m_, the probability that every correct process eventually delivers _m_ is at least 1-ε.
    *   No duplication - no message is delivered more than once
    *   No creation - if a process log-delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_

**FIFO and Causal broadcast** - deliver messages according to first-in first-out (FIFO) order and according to causal order (preserves the potential causality among messages from multiple senders)



*   **FIFO-order specification **- one of the simplest possible orderings and guarantees that messages from the **same sender** are delivered in the same sequence as they were broadcast by the sender. 
    *   Validity - if a correct process broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - no message is delivered more than once
    *   No creation - if a process delivers a message _m_ with sender _s_, then _m_ was previously broadcast by process _s_
    *   Agreement - if a message _m_ is delivered by some correct process, then _m_ is eventually delivered by every correct process
    *   **FIFO delivery** - if some process broadcasts message _m1_ before it broadcasts message _m2_, then no correct process delivers _m2_ unless it has already delivered _m1_
*   **Causal-order specification** - messages are delivered such that they respect all cause–effect relations
    *   RB1-RB4 or URB1-URB4
    *   **Causal delivery** - for any message _m1_ that potentially caused a message _m2_, i.e., _m1_ → _m2_, no process delivers _m2_ unless it has already delivered _m1_.

**Byzantine broadcast **- operates in the fail-arbitrary system model.



*   **Byzantine consistent broadcast** -  **every instance** of consistent broadcast has a  designated sender process _s_, who broadcasts a message _m_ . If the sender _s_  is correct then every correct process should later deliver _m_. If _s_ is faulty then the primitive ensures that every correct process delivers the same message, if it delivers one at all. In other words, with a faulty sender, some correct processes may deliver a message and others may not, but if two correct processes deliver a message, it is unique.
    *   Validity - if a correct process _p_ broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - every correct process delivers at most one message
    *   Integrity - if some correct process delivers a message _m_ with sender _p_ and process _p_ is correct, then _m_ was previously broadcast by _p_
    *   Consistency - if some correct process delivers a message _m_ and another correct process delivers a message _m'_, then _m_ = _m'_ 
*   **Byzantine reliable broadcast** - ensure agreement in the sense that a correct process delivers a message if and only if every other correct process delivers a message
    *   Validity - if a correct process _p_ broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - every correct process delivers at most one message
    *   Integrity - if some correct process delivers a message _m_ with sender _p_ and process _p_ is correct, then _m_ was previously broadcast by _p_
    *   Consistency - if some correct process delivers a message _m_ and another correct process delivers a message _m'_, then _m_ = _m'_ 
    *   **Totality** - if some message is delivered by any correct process, every correct process eventually delivers a message

**Byzantine broadcast channels** - delivers multiple messages in contrast to the byzantine broadcast abstractions. The label is an arbitrary bit string that can be determined by the implementation, under the only condition that the labels of all messages delivered on a channel for a particular sender are unique. One can think of it as a per-sender sequence number.



*   **Byzantine consistent broadcast channel**
    *   Validity - if a correct process _p_ broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - for every process _p_ and label _l, _every correct process delivers at most one message with label _l_ and sender _p_
    *   Integrity - if some correct process delivers a message _m_ with sender _p_ and process _p_ is correct, then _m_ was previously broadcast by _p_.
    *   Consistency - if some correct process delivers a message _m_ with label _l_ and sender _s_, and another correct process delivers a message _m' _with label _l_ and sender _s_, then _m_ = _m'_
*   **Byzantine reliable broadcast channel**
    *   Validity - if a correct process _p_ broadcasts a message _m_, then every correct process eventually delivers _m_
    *   No duplication - for every process _p_ and label _l, _every correct process delivers at most one message with label _l_ and sender _p_
    *   Integrity - if some correct process delivers a message _m_ with sender _p_ and process _p_ is correct, then _m_ was previously broadcast by _p_.
    *   Consistency - if some correct process delivers a message _m_ with label _l_ and sender _s_, and another correct process delivers a message _m' _with label _l_ and sender _s_, then _m_ = _m'_
    *   **Agreement** - if some correct process delivers a message _m_ with label _l_ and sender _s_, then every correct process eventually delivers message _m_ with label _l_ and sender _s_
