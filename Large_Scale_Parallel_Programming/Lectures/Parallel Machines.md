---
tags:
  - 💾
---

# [[Topology]]
[[Mesh]]
[[Binary Tree]]
Fat tree is an engineering solution and they will limit the amount of connections higher-up in the tree. In theory unlimited connections can occur  
[[Hypercube]]

## Advantages and disadvantages

| Topology      | *Number of nodes*        | *Diameter* | *Bisection* *width* | *Edges per node*    | *Implementation* |
| ------------- | ------------------------ | ---------- | ------------------- | ------------------- | ---------------- |
| **Mesh**      | $k^2 \quad (k=\sqrt{p})$ | $2(k-1)$   | $k$                 | $\textasciitilde 4$ |                  |
| **Torus**     |                          |            |                     |                     |                  |
| **Tree**      | $2^{h} -1$               | $2*h$      | $2^{h+1}$           | $\textasciitilde 6$ |                  |
| **Hypercube** |                          |            | $2^{k-1}$           |                     |                  |
|               |                          |            |                     |                     |                  |

# Use of multiple topologies
When building a cluster computing of cheap commodities you are not going to use different topologies. But for super computers different topologies are used for different applications. 
Tree is better for broadcasting

# DAS-5
Fat tree with separated hard-ware for the switches to link all the nodes
Has a root switch at the top level with 12 links to the other switches

# Types of parallel machines
## Vector computing
Good at processing arrays / vectors. This is highly specialised but not used that much any more
## [[Shared-memory multiprocessors]]
Using a global shared-memory as programming modelling.
Doesn't scale with large systems, like when you add a bus the bus will become the bottleneck.

Current super computers are collections of shared-memory that are connected by a network
This makes the system a Distributed system

## Distributed-memory multiprocessors
Routers make diameter less important because of the current hardware

## Distributed Shared Memory
Hybrid form
The memory is physically distributed with a shared memory programming model.
The modern computer uses a distributed shared memory

# CPU
Must be good at everything
This makes that only a small part is used for extual computationals with a large of overhead.
Large of the chip space is taken by things 

No longer sequantial
# GPU
GPU's are undoing all te architectical upgrades of the last decade on the CPU
They use multiple small cores instead
Useful when you need to do massive amount of small computations without ..

# Flynn's Taxonomy

|                          | Single Data | Multiple Data |
| ------------------------ | ----------- | ------------- |
| **Single Instruction**   | SISD        | SIMD          |
| **Multiple Instruction** | MISD        | MIMD          |

# Routing technology
All the information from node A to node B is transferd via all the nodes in between
## Circuit-switched
Uses routers to transport all the information from node A to node B.
It has  kind of setup cost for connecting between the nodes

## Wormhole routing
The first bits look around to find the optimal rout, whereby the rest of the information follows after. 

# Homework questions
```
Describe the similarities and one key difference between NUMA multiprocessors and Distributed Shared Memory
```
Similarities:
2. Memory is physical distributed
3. One memory model
4. Memory access times Non-uniform
Difference:
1. NUMA is hardware DSM is software

# Real machines
VU was one of the first associatons to build cluster computers in 1984. At this moment