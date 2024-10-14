# History of Parallel Machines
**1967:**
- First parallel computer (ILLIAC IV)
**1970s:**
- Programming methods
- Experimental machines
- Vector computers
**1980s:**
- Parallel languages (SR, Linda, Orca) 
- Commercial supercomputers
**1990s:**
- Software standardization (MPI) 
- Large-scale machines (Blue Gene)
- Massively Parallel Processors (MPPs)
- Standard microprocessors, special network and I/O
**2000s:**
- Cluster computing (using standard PCs)
- Grid computing
- Advanced architectures (Blue Gene)
- Comeback of vector computing
- IBM Cell/BE
**2010s:**
- Multi-cores
- GPUs
- Intel Phi
- Cloud data centers

## Clusters
A computer cluster is a set of computers that work together so that they can be viewed as a single system. Unlike grid computers, computer clusters have each node set to perform the same task, controlled and scheduled by software. The newest manifestation of cluster computing is cloud computing. 

They use a good price/performance ratio and can be made by standard PCs.
Cluster computers processing power was similar to the supercomputers, because they are both based on microprocessors.
But the communication performance was the key difference.

![[beowulf-892673119.png]]
## Grid computing
Grid computing is the use of widely distributed computer resources to reach a common goal. A computing grid can be thought of as a distributed system with non-interactive workloads that involve many files. Grid computing is distinguished from conventional high-performance computing systems such as cluster computing in that grid computers have each node set to perform a different task/application. Grid computers also tend to be more heterogeneous and geographically dispersed (thus not physically coupled) than cluster computers. Although a single grid can be dedicated to a particular application, commonly a grid is used for a variety of purposes. Grids are often constructed with general-purpose grid middleware software libraries. Grid sizes can be quite large.
![[unnamed.png]]
## Performance Development
Performance of supercomputers in the Top500 list between 1992 and 2023. The different lines present the accumulated performance of all computers in the list (green), and the performance of the top (#1, brown) and last (#500, blue) in each list.
![[Performance-of-supercomputers-in-the-Top500-list-between-1992-and-2023-The-different.jpg]]
## Architecture Development

![[SuperComputing_reference.png]]
# Why do we need parallel processing?
In the current day and age many applications require a huge amount of computer power.
Besides the sequential cores have reached their speed limits.
## Moore's Law
Moore made a prediction in 1975 that circuit complexity doubles every 18 months. Whereby the complexity was calculated as the amount of transistors.
The exponential transistor growth, resulted in exponential growth of processor speeds.
But currently we have hit a  "power wall" whereby the speed isn't increasing as fast.

But Moore's law isn't dead. This is because the law isn't about speed increase but about number of transistors. In the multicore era, the number of transistors kept growing, but they were used to build multiple cores instead of making single cores faster.
# Processor Organization & [[Topology]]
The topology of networks are constructed as a graph, whereby the nodes are processors and the edges are the communication path.
For this the following evaluation criteria's are used:
- *Diameter* Shortest longest path
- *Bisection width* Minimum number of edges that should be removed to split the graph into 2 almost equal halves
- *Number of edges per node*
## Mesh
A mesh topology is a network setup where each computer and network device is interconnected with one another. This topology setup allows most transmissions to be distributed even if one of the connections goes down. It is a topology commonly used for wireless networks.
A mesh is a q-dimensional lattice.
![[mesh.png]]
## Torus
In geometry, a torus is created by revolving a circle about an axis coplanar to the circle. While this is a general definition in geometry, the topological properties of this type of shape describes the network topology in its essence.
A torus adds an extra edge to the nodes that have less than 4 edges.
Resulting in that the boundaries are connected to one another.
![[A-44-Torus-network-topology-2195623876.png]]
## Binary Tree
Tree Topology is a topology which is having a tree structure in which all the computers are connected like the branches which are connected with the tree. In Computer Network, tree topology is called a combination of a Bus and Star network topology. The main advantages of this topology are that is very flexible and also has better scalability. Tree network topology is considered to be the simplest topology in all the topologies which is having only one route between any two nodes on the network.

The nodes are connected via a tree.
The diameter is better than a mesh for large values of P.
$$
\log^2{(P)} < \sqrt{P}
$$
![[tree_height.png]]
## Hyper Tree
A hypertree network is a network topology that shares some traits with the binary tree network. It is a variation of the fat tree architecture.

A hypertree of degree $k$ depth d may be visualized as a 3-dimensional object whose front view is the top-down complete k-ary tree of depth $d$ and the side view is the bottom-up complete binary tree of depth  $d$. 

Because of this structure it gives a better bisection width
![[hypertree-computer-network-with-sixteen-processing-nodes[1].jpg]]
## Fat Tree
The fat tree network is a universal network for provably efficient communication. $k$-ary $n$-trees, the type of fat-trees commonly used in most high-performance networks.

In a tree data structure, every branch has the same thickness (bandwidth), regardless of their place in the hierarchy—they are all "skinny" (skinny in this context means low-bandwidth). In a fat tree, branches nearer the top of the hierarchy are "fatter" (thicker) than branches further down the hierarchy. In a telecommunications network, the branches are data links; the varied thickness (bandwidth) of the data links allows for more efficient and technology-specific use.

Fat trees are used in a lot of places in real life.
![[Fat_tree_network.svg.png]]
## Hyper Cube
In computer networking, hypercube networks are a type of network topology used to connect and route data between multiple processing units or computers. Hypercube networks consist of $2^m$ nodes, which form the vertices of squares to create an internetwork connection. A hypercube is basically a multidimensional mesh network with two nodes in each dimension. Due to similarity, such topologies are usually grouped into a $k$-ary $d$-dimensional mesh topology family, where $d$ represents the number of dimensions and k represents the number of nodes in each dimension.

![[A-Four-dimensional-hypercube-System-52-Hypercubic-P2P-Grid-Topology-The-Hypercubic-P2P.png]]
## Comparison
| Topology               | *Number of nodes*               | *Diameter* | *Bisection* *width* | *Edges per node*    | *Implementation*                  |
| ---------------------- | ------------------------------- | ---------- | ------------------- | ------------------- | --------------------------------- |
| **Mesh**               | $k^2 \quad (k=\sqrt{p})$        | $2(k-1)$   | $k$                 | $\textasciitilde 4$ | Easy                              |
| **Torus**              | $k^2 \quad (k=\sqrt{p})$        | $k$        | $2k$                | $4$                 | Moderate                          |
| **Tree**               | $2^{h} - 1 \quad (h=\log^2(p))$ | $2h$       | $2^{h+1}$           | $\textasciitilde 6$ | Easy                              |
| **Hypertree** **(4D)** | $2^{h}\;(2^{h+1}-1)$            | $2h$       | $2^{h+1}$           | $\textasciitilde 6$ | Only in theory                    |
| **Hypercube**          | $2^{n}$                         | $n$        | $2^{k-1}$           | $n$                 | Complexer for multiple dimensions |
When building a cluster computing of cheap commodities you are not going to use different topologies. But for super computers different topologies are used for different applications. 
Tree is better for broadcasting
# Types of Parallel Machines
## Processor Arrays
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

## GPU's
Within standard computers the CPU's are the most important processor in a given computer because it handles and processes all the given instructions. It uses registers for temporary data storage. Because of this it must be good at everything.
Current day CPU's have a large cache designed to store frequently accessed data and instructions, because of all the optimization over time. This makes that only a small part is used for actual computing with a large of overhead of control and pre-programmed instructions. Besides the current CPU's are no longer sequential.
![[Pasted image 20240911115055.png]]

In contrast the CPU the GPU are undoing all the architecture upgrades that happened the last decade to the CPU. Instead of being good at everything it has a large amount of smaller chips that can do a massive amount of small computations. These computations aren't complex but you can parallelized it easier. 
![[Pasted image 20240911115707.png]]
## Shared-Memory Multiprocessors
Multiple processors accessing a common global shared-memory space connected via a bus, to work on tasks. Each processor can read from and write to this shared memory.

Within this architecture the bus can become the bottle neck of the system because all the data needs to flow through it.  A cache can be added to decrease the traffic of the bus so that frequently accessed data is stored locally.

Another problem within shared-memory multiprocessors are to maintain cache coherency.
1. **Snooping Cache:**  This technique involves caches monitoring (or "snooping" on) the bus to detect when other caches write to shared data. If a cache sees that another processor has modified a copy of the data it also holds, it will invalidate its own copy to maintain consistency.
2. **Write-through:** In this cache write policy, any changes made to the cache are immediately written to the main memory as well. This ensures that memory always has the most up-to-date data but can create more bus traffic.
3. **Copy-back:** In this policy, changes are initially only made to the cache. The updated data is only written to the main memory when the cache line is replaced. This reduces bus traffic but requires mechanisms to ensure consistency.

Current super computers are collections of shared-memory that are connected by a network
This makes the system a Distributed system
![[Pasted image 20240911123225.png]]

## Non-Uniform Memory Access Multiprocessors
(NUMA)
An hardware architecture where the physical memory is distributed within a system and were all the processor units can access it. While processors can access all memory, the performance is optimized when they use their own local memory. Because of this the memory access time is not-uniform.

The idea of this is now applied within multi-core systems.

![[Pasted image 20240911124645.png]]
## Distributed-Memory Multiprocessors
Within a distributed-memory multiprocessors each processors has only it's local memory.
The processors communicate with each other by sending messages over a network. It uses no-remote-memory-access (NORMA) machines. 
Computational tasks can only operate on local data, and if remote data are required, the computational task must communicate with one or more remote processors. In contrast, a shared memory multiprocessor offers a single memory space used by all processors. Processors do not have to be aware where data resides, except that there may be performance penalties, and that race conditions are to be avoided.
![[A-distributed-memory-multiprocessor-The-key-distinction-between-this-and-the-shared-bus_Q320.jpg]]
For the connection routing of message's is important such as packet or circuit-switched message routing.
## Distributed Shared Memory
Memory architecture where physically separated memories can be addressed as a single shared address space. The term "shared" does not mean that there is a single centralized memory, but that the address space is shared—i.e., the same physical address on two processors refers to the same location in memory.

It combines the easy programming of the shared memory with the scalability of the distributed memory.
## Routing
All the information from node A to node B is transferd via all the nodes in between
### Packet-switched
Packet Switching in computer networks is a method of transferring data to a network in the form of packets. In order to transfer the file fast and efficiently over the network and minimize the transmission latency, the data is broken into small pieces of variable length, called Packet.
At the destination, all these small parts (packets) have to be reassembled, belonging to the same file. A packet is composed of a payload and various control information. No pre-setup or reservation of resources is needed.
![[4514-660.png]]
### Circuit-switched
Circuit Switching is a type of switching, in which a connection is established between the source and destination beforehand. This connection receives the complete bandwidth of the network until the data is transferred completely. However, circuit switching can be inefficient and costly due to its requirement for dedicated resources, making it less suitable for high-traffic or large-scale networks. In this article, we will discuss every point about Circuit Switching.
![[Circuit-Switching.webp]]

### Wormhole routing
Wormhole routing is a unique case of virtual cut-through, where the buffers at the intermediate nodes are the size of a flit. The network latency is equal to in virtual cut-through and thus autonomous of the communication distance. The first bits look around to find the optimal rout, whereby the rest of the information follows after. This gives the advantage that not all data has to been send to the next processor.

The advantage of wormhole routing over circuit switching and virtual cut-through appears in networks where contention is not negligible. In circuit switching, once a channel is assigned to a message, it cannot be used by other messages until the channels become free. In virtual cut-through, blocked messages should be saved in the buffer of intermediate nodes.
![[Flit-and-Wormhole-Routing-of-Packet-3795681904.png]]

## Flynn's Taxonomy
|                          | Single Data | Multiple Data |
| ------------------------ | ----------- | ------------- |
| **Single Instruction**   | SISD        | SIMD          |
| **Multiple Instruction** | MISD        | MIMD          |
**SISD:** 
- Single Instruction stream / Single Data stream  
- Traditional uniprocessors  
**SIMD:** 
- Single Instruction Multiple Data  
- Processor arrays  
**MISD:** 
- Multiple Instruction Single Data  
- Nonexistent? Dataflow machines?  
**MIMD:** 
- Multiple Instruction Multiple Data  
- Multiprocessors and multicomputers

# Blue Gene/L
![[Pasted image 20241014173630.png]]Blue Gene uses the following network:

**3 Dimensional Torus**
- Interconnects all compute nodes (65,536)
-  Virtual cut-through hardware routing
- 1.4Gb/s on all 12 node links (2.1 GB/s per node)
- 1 μs latency between nearest neighbors, 5 μs to the farthest
-  Communications backbone for computations
- 0.7/1.4 TB/s bisection bandwidth, 68TB/s total bandwidth
**Global Collective**
- One-to-all broadcast functionality
- Reduction operations functionality
- 2.8 Gb/s of bandwidth per link
- Latency of one way traversal 2.5 μs
- Interconnects all compute and I/O nodes (1024)
**Low Latency Global Barrier and Interrupt**
- Latency of round trip 1.3 μs
**Ethernet**
- Incorporated into every node ASIC
- Active in the I/O nodes (1:8-64)
 - All external comm. (file I/O, control, user interaction, etc.)
**Control Network**

![[Pasted image 20241014173754.png]]