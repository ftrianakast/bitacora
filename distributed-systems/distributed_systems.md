# Course on Distributed Systems by Martin Kleppmann

## Books

1. van Steen & Tenenbaum
"Distributed Systems"

2. Cachin, Guerraoui & Rodrigues
"Introduction to Reliable and Secure Distributed Programming"

3. Kleppmann
"Designing Data-Intensive Applications"

4. Bacon & Harris
"Operating Systems: Concurrent and Distributed Software Design"

## Why make a Distributed System?

- Because the problem is inherently distributed, like sending messages
- For better reliability
- For better performance
- TO solve bigger problems: huge amounts of data that don't fit in a machine

Disadventages:

1. they rely on network and network is problematic
2. process may crash (and we might not know)
3. All this happens nondeterministically

## Distributed Systems and Computer Networking

We use an oversimplication of a distributed system:

node i -------------message-------------> node j

Two important aspect of distributed systems:

a) Latency: time untile message arrrives
b) Bandwith: data volume per unit time

## RPC (Remote Procedure Call)

Its another way of commmunicate nodes. It makes a call to a remote function
look the same as a local function call.
This obeys to the principle "Location Transparency"

However the philosophy is not quite accurate because as we have said network
os unreliable.
_CORBA_ was an example of this

2017: Google RPC introduced a new RPC framework called GRPC

## System Models of Distributed systems

Examples:

1. The two generals problem

army1 and army2 want to attack a city1
The two armies communicated between each other via messengers
Those messenger are exposed to certain risk: so the message could be lost

army 1               army2                outcome
does not attack      does not attack      nothing happens
attacks              does not attack      army1 is defeated
does not attack      attacks              army2 defeated
attacks              attacks              city captured

Desired: army1 attackas if and only if army2 attacks


Situation 1: general1 sends the messenger, the message is received by general2
             but then messenger does not give a response to general 1

Situation 2: general1 sends the messenger, the message is lost

There is not common knowledge: The only way of knowing something is to communicate it

2. The Byzantine generals problem

Problem: some of the generals might be traitors
But with a difference: messengers mechanism never fail

                  +-----------+
      +-----------|   army3   |------------+
      |           +-----------+            |
      |                 |                  |
 messenger           attack?           messenger
      |                 |                  |
      |           +-----|-----+            |
      |           |           |            |
      |    +-------   city    | ------+    |
      |    |      |           |       |    |
      | attack?   +-----------+    attack? |
      |    |                          |    |
      |    |                          |    |
      |---------+              +------|----|
      |  army1  |<-----mess---->   army2   |
      +---------+              +-----------+

Conditions:

1. Up to f generals might behave malicously
2. Honest generals don't know who the malicious ones are
3. The malicious generals may collude
4. Nevertheless, honest generals must agree on plan

Theorem: need 3f + 1 generals in total to tolerate malicious generals
(i.e. < 1/3 may be malicious)

Cryptography (digital signatures) helps - but problem remains hard

               +-----------+                                    
    +-----------|  customer |--------------+                    
    |           +-----------+              |                    
    |                 |                    |                    
   RPC             agree?               RPC|                    
    |                 |                    |                    
    |           +-----|-----+              |                    
    |           |           |              |                    
    |    +-------   order   | ------+      |                    
    |    |      |           |       |      |                    
    | agree?    +-----------+    agree?    |                    
    |    |                          |      |                    
    |    |                          |      |                    
    |---------+              +------|------|                    
    |  online |<-----mess---->   payments  |                    
    |  shop   |              |   service   |                    
    +---------+              +-------------+                    

How they all agree in the status of an order?

"Byzantine" has long been used for excessively complicated, bureazcratic, devious. "The Byzantine tax law"

## System models

In real systems, both nodes and networks may be faulty.
Capture assumptions in a system model consisting of:

1- Network behaviour (message loss)
2- Node behaviour (crashes)
3- Timing behaviour (latency)

Choice of model for each of these parts

### Networks

- Networks are unreliable
- Network partition

But we can take some scenerarion

a) Reliable (perfect) links
b) Fair-loss links: Messages may be lost, duplicated or reordered
c) Arbitrary links (active adversary): A malicious adversary

We can go from Arbitraty to Fair-Loss with TLS and from Fair-Loss to Reliable
with retry + dedup

### Node behaviour

- Crash-Stop: A node is faulty if it crahes (at any moment). After crashing
- it stops executing forever
- Crash-Recovery: A node may crash at any moment, losing its in-memory state.
- It may resume executing sometime later
- - Byzantine (fail-arbitrary): A node is faulty if it deviates from its behaviour

### Synchrony

- Synchronous: Message latency node greater than a know upper bound
Nodes execute algorithm at a know speed

- Partially synchronous:
- Asynchronous
  Messages can be delayed arbitrarily
  Nodes can pause execution arbitrarily
  No timing guarantees at all

For each of three parts of system modeling, pick one:

- Network: reliable, fair-loss or arbitrary
- Nodes: crash-stop, crash-recovery or byzantine
- Timing: synchronous, partially synchronous or asynchronous

## Fault tolerance

We want availability to 99.999% per year which means only 5.3 minutes/year

SLO(Service Level Objective): 99,9% of users get response in 200ms
SLA(Service Level Agreement): contract specifying some SLO, penalties for violation

### Failure detectors

Failure detectors: algorithm that detects wheter another node is faulty
Perfect failure detector: labels a node as faulty if and only if it has crashed

Problem:
cannot tell the difference between crashed node, temporarily unresponsive node,
lost messagem and delayed message

__Eventually perfect failure Detector__
- May temporarily label a node as crashed, even though it is correct
- May temporarily label a node as correct, even it has crashed
- But eventually, labels a node as crashed if and only if has crashed

## Times, clocks

- time as schedulers, timeouts, failure detectors, retry times
- performance measurements
- cache times
- determine order of events across several nodes

We distinguisg between:

- physical clocks: counts the number of seconds elapsed
- logical clocks: counts events, messages, etc

Computer clocks are made of quartz: which frequency sometimes is affected for high temperatures for example. The clock error is measured in ppm

If quartz is not precise then maybe you want to use an atomic clocks. They are used for satellites

We take atomical time and we make corretions depending on astronomical obsrvations and we define then UTC.

How computers represent timestamps:

Unix time is the number of seconds since 1 January of 1970, not counting leap seconds. Most software ignores leap seconds.

Some solution is to smear the leap second during the course of a day

## Clock synchronization

Because computers use quartz clocks, they could drift, which give us: clock skew: difference between  two clocks at a given point.

How do we deal with this?

With a protocol called _NTP_
or a protocol called _PTP_

### NTP

Stratum 0: atomic clock or GPS receiver
Stratum 1: synced directly with stratum 0 device
Stratum 2: servers that sync with with stratum 1, etc

The protocol reduces clock skew to few milliseconds in good network conditions,
but can be much worse!

NTP Client ------------T1-------------->T2 NTP Server

NTP Client T4<-----------(T1,T2,T3)---------------T3 NTP Server

Roundtrip network delay: d = complete time(T4 - T1) - processing time(T3 - T2)

Estimated server time when client receives response: t3 + d/2

Estimated clock skew: t3 + d/2 -t4 = (t2-t1 + t3-t4) / 2


System.nanoTime() --> Time since arbitrary tume Monotonic clock (when the machine booted up). Good for meaurements
Szstem.currentTimeMillis() --> Time since 1 January 1970

## Ordering of messages

we need to define mathematically:

a __happens before__ b (a -> b) iff:

An __event__ is something happening at one node (sending or receiving a message, or a local execution step)

- a and b occurred at the same node, and a occurred before b in that nodes local execution order
- a is the sending event of some message m, and event b is the receipt of the same message m (assuming sent messages are unique)
- there is  an event c such a -> c and c -> b then a -> b

we have the concept of _causality_

a -> b, then a _might have caused_ b
a || b, we know that a cannot have caused b

This encodes potential causality


# Broadcast protocols and logical time

Why physical time is if there is a message m1 with t1 and a message m2 with t2 it could be that t2 < t1.

Because of this we use logical clocks that captures the causality.

__Physical Clock__: count number of seconds elapsed
__Logical Clock__: count number of events occured. They are designed to capture
causality. (e1 -> e2) => T(e1) < T(e2)

## Logical clocks

- Lamport clocks
- Vector clocks

They capture the property (e1 -> e2) => T(e1) < T(e2)

### Lamport clocks algorithm

__on__ initialisation __do__
  t :=0       _each node has its own local variable t_
__end on__

__on__ any event occuring at the local node __do__
  t := t + 1
__end on__

__on__ request to send message _m_ __do__
  t := t + 1; send (t, m) via the underlying network link
  __end on__

__on__ receiving (t', m) via the underlying network link __do__
  t := max(t', t) + 1
  deliver m to the application
__end on__

_Properties of Lamport clocks_

Assumptions:

0. Each node mantains a counter _t_, incremented on every local event _e_
1. Let L(e) be the value of _t_ after that increment
2. Attach current _t_ to message sent over the network
3. Recipient moves its clock forward to timestamp in the message (if greater than local counter), then increments

Properties:

1. if a -> b then L(a) < L(b)
2. L(a) < L(b) does not imply a -> b
3. It is possible that L(a) = L(b) for a /= b

