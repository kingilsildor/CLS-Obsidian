# N-body methods
## The basics
The $N$-body problem is the problem of predicting the individual motions of a group of objects.
Most of the time these are celestial objects interacting with each other gravitationally, but can also be done at a molecular level. 

Based on this idea simulations can be made to follow the behavior of a system over time.
```c
for each timestep do
	Compute forces between all bodies
	Compute new position and velocities
od
```
This pseudo code can be parrelized the following way:
- Distribute the $N$ bodies equally among all machines
- $O(N)$ communication and $O(N^2)$ compute time per timestep
- No load balancing problems #TODO
But for astrophysics, $N$ can be many billions, meaning that $O(N^2)$ solutions is still a lot

For this an approximation is used whereby a single body with same mass and center-of-mass.
This is because forces fall very rapidly with distance between bodies.
![[Pasted image 20241008125549.png]]
## Data structures
One can hierarchical represent the physical space of the nodes.
Within this application two data structures can be used:

| Data structure | Dimensions |
| -------------- | ---------- |
| Quadtree       | 2D         |
| Octree         | 3D         |
Quadtrees are the two-dimensional analog of octrees and are most often used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions.

You start with one cell with all the bodies and recursively split the cells with multiple bodies into sub-cells. Whereby each node contains the center of mass data for it's cell.

The smaller sections will have a higher center of mass

## Barnes-Hut algorithm
```c
for each timestep do
	Build tree
	Compute center-of-mass for each cell
	Compute forces between all bodies
	Compute new positions and velocities
od
```
For many practical problems this algorithm will have an time complexity of $O(N \log N)$
1. The tree is build up using a recursive algorithm that can be parallelized
2. The center-of-mass is calculated for each cell upwards through the tree
3. The forces between the bodies are calculated, the most time complex stem
4. The positions and velocities of each node are updated

The force is calculated the following way:
```c
for each body B do
	B.force := ComputeForce(tree.root, B)
od

function ComputeForce(cell B): float;
	if distance(B, cell.CenterOfMass) > threshold
	then
		return DirectForce(B.position, B.mass, cell.CenterOfMass, cell.Mass)
	else
		sum := 0.0
		for each subcell C in cell do
			sum +:= ComputeForce(C, B)
		return sum
```

## Parallelizing Barnes-Hut
# Transposition Driven Scheduling
## Search