# [Traveling sales person](Traveling%20Sales%20Person)
Search overhead


"Given  a list of cities and the distance between each pair of cities, what is the shortest possible route that visits each city exactly once"

It's challenging to solve because of all the possible routes. You can't really solve it brute force. Branch and bound is a strategy for this, but it is sequential. 
For this you represent all possible routes 

[[Dept-First Search]]
[[Breadth-First Search]]


Visit all the cities and keep track of the minimal distance

It's not a greedy algorithm, because then you would only find an approximating of a solution.
Within this algorithm you will find the exact solution.

## Parallelization 

**Option 1:**
Give each branch it's own worker

**Option 2:**
Give an worker multiple branches
You want to include pruning, but that makes the option not the efficient, this is because of the [load balance](Load Balance).
You don't know how many branches each worker will take.

Because there is only a local minimum the parallel algorithm can be slower than the normal one if the minimum is at the beginning.

Option 3:
The master branch uses the top depth of the graph based on maxhops. The workers will search in parallel the subtree below.
A job is an incomplete root.