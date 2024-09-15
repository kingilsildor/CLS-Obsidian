---
tags:
  - 💾
---

# [[Topology]]

| Topology               | *Number of nodes*               | *Diameter* | *Bisection* *width* | *Edges per node*    | *Implementation*                  |
| ---------------------- | ------------------------------- | ---------- | ------------------- | ------------------- | --------------------------------- |
| **Mesh**               | $k^2 \quad (k=\sqrt{p})$        | $2(k-1)$   | $k$                 | $\textasciitilde 4$ | Easy                              |
| **Torus**              |                                 |            |                     |                     |                                   |
| **Tree**               | $2^{h} - 1 \quad (h=\log^2(p))$ | $2h$       | $2^{h+1}$           | $\textasciitilde 6$ | Easy                              |
| **Hypertree** **(4D)** | $2^{h}\;(2^{h+1}-1)$            | $2h$       | $2^{h+1}$           | $\textasciitilde 6$ | Only in theory                    |
| **Hypercube**          | $2^{n}$                         | $n$        | $2^{k-1}$           | $n$                 | Complexer for multiple dimensions |
[[Mesh]]
[[Torus]]
[[Binary Tree]]
[[Hyper Tree]]
[[Fat Tree]]
[[Hypercube]]

# Use of multiple topologies
When building a cluster computing of cheap commodities you are not going to use different topologies. But for super computers different topologies are used for different applications. 
Tree is better for broadcasting

# DAS-5
Fat tree with separated hard-ware for the switches to link all the nodes
Has a root switch at the top level with 12 links to the other switches

# Types of parallel machines
## [Processor arrays / Vector computing](Vector Computing)
A [CPU](Central Processing Unit) whereby the instructions are designed to operate efficiently and effectively on large 1D-arrays / [vectors](Linear Algebra). This makes that these CPU's are highly specialized in a certain task, but don't have much uses besides that.

In the front-end the CPU is just a sequential machine that executes programs and broadcast the vector operations to the Processing Elements (PE).
The PE's perform operations on its part of the vector, for example the end.
The ideas of vector computing are now applied in CPU-extensions and [GPU's](Graphics Processing Unit)

Within [Flynn's Taxonomy](Flynn's taxonomy) vector computers are SIMD

Adding two groups of 10 numbers together on a normal CPU:
```armasm
; Hypothetical RISC machine
; assume a, b, and c are memory locations in their respective registers
; add 10 numbers in a to 10 numbers in b, store results in c
  move  $10, count   ; count := 10
loop:
  load  r1, a
  load  r2, b
  add   r3, r1, r2   ; r3 := r1 + r2
  store r3, c
  add   a, a, $4     ; move on
  add   b, b, $4
  add   c, c, $4
  dec   count        ; decrement
  jnez  count, loop  ; loop back if count is not yet 0
  ret
```
And on a vector processor:
```armasm
; assume we have vector registers v1-v3
; with size equal or larger than 10
  move   $10, count    ; count = 10
  vload  v1, a, count
  vload  v2, b, count
  vadd   v3, v1, v2
  vstore v3, c, count
  ret
```
## CPU vs GPU
Within standard computers the CPU's are the most important processor in a given computer because it handles and processes all the given instructions. It uses registers for temporary data storage. Because of this it must be good at everything.
Current day CPU's have a large cache designed to store frequently accessed data and instructions, because of all the optimization over time. This makes that only a small part is used for actual computing with a large of overhead of control and pre-programmed instructions. Besides the current CPU's are no longer sequential.
![[Pasted image 20240911115055.png]]

In contrast the CPU the GPU are undoing all the architecture upgrades that happened the last decade to the CPU. Instead of being good at everything it has a large amount of smaller chips that can do a massive amount of small computations. These computations aren't complex but you can parallelized it easier. 
![[Pasted image 20240911115707.png]]
## [[Shared-Memory Multiprocessors]]
Multiple processors accessing a common global shared-memory space connected via a bus, to work on tasks. Each processor can read from and write to this shared memory.

Within this architecture the bus can become the bottle neck of the system because all the data needs to flow through it.  A cache can be added to decrease the traffic of the bus so that frequently accessed data is stored locally.

Another problem within shared-memory multiprocessors are to maintain cache coherency.
1. **Snooping Cache:**  This technique involves caches monitoring (or "snooping" on) the bus to detect when other caches write to shared data. If a cache sees that another processor has modified a copy of the data it also holds, it will invalidate its own copy to maintain consistency.
2. **Write-through:** In this cache write policy, any changes made to the cache are immediately written to the main memory as well. This ensures that memory always has the most up-to-date data but can create more bus traffic.
3. **Copy-back:** In this policy, changes are initially only made to the cache. The updated data is only written to the main memory when the cache line is replaced. This reduces bus traffic but requires mechanisms to ensure consistency.

Current super computers are collections of shared-memory that are connected by a network
This makes the system a Distributed system
![[Pasted image 20240911123225.png]]

# Non-Uniform Memory Access (NUMA) multiprocessors
An hardware architecture where the physical memory is distributed within a system and were all the processor units can access it. While processors can access all memory, the performance is optimized when they use their own local memory. Because of this the memory access time is not-uniform.

The idea of this is now applied within multi-core systems.

![[Pasted image 20240911124645.png]]
## Distributed-memory multiprocessors
Routers make diameter less important because of the current hardware

## Distributed Shared Memory
Hybrid form
The memory is physically distributed with a shared memory programming model.
The modern computer uses a distributed shared memory

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