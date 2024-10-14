# Parallelization steps
To develop and study the parallel implementation, we will follow these steps:
- Identify the parts of the sequential algorithm that can be parallelized
	- (For example inner for loops)
	- If the iterations are independent of each other, they are trivial to parallelize
		- If you change to order of the loops and the end result changes, the iterations are not independent.
- Consider different parallelization strategies
- Discuss the (theoretical) performance of these implementations
	- (Data dependencies, communication overhead, complexity)
- Measure the actual performance of these implementations

# Speedup
The parallel speedup $S_p$ on $P$ processes is defined as:
$$S_P = \frac{T_1}{T_P}$$
where $T_1$ denotes the runtime of the sequential algorithm and $T_P$ denotes the runtime of the parallel algorithm on $P$ processes.
We expect an parallel algorithm to run $p$ times faster than the sequential one. Because of this the optimal speedup of a parallel algorithm on $p$ processes is equal to $P$
$$S_{p}^* = P$$
The ratio of the actual speedup over the optimal one is called the parallel efficiency (the closer to one the better)
$$E_P = \frac{S_p}{S_{p}^*} = \frac{T_1 / T_P}{P} $$
# [[Matrix Multiplication]]
Given $A$ and $B$ two $N$-by-$N$ matrices, compute the matrix-matrix product $C=AB$

![[Pasted image 20241002095745.png]]
![[Pasted image 20241002102435.png]]

For this parallel algorithm the following assumptions are made:
- All matrices $A$, $B$, and $C$ are initially stored in the master process
- The result will be overwritten in matrix $C$, in the master process


A (naive) sequential algorithm follows the mathematical definition of the matrix-matrix product $$C_{ij} = \sum_k A_{ik} B_{kj}$$
This results in 3 for loops:
```julia
@everywhere function matmul_seq!(C,A,B)
	# Create variable for the different dimensions of the matrix
    m = size(C,1)
    n = size(C,2)
    l = size(A,2)
    # Check if the size match
    @assert size(A,1) == m
    @assert size(B,2) == n
    @assert size(B,1) == l
    z = zero(eltype(C))
    for j in 1:n
        for i in 1:m
	        # Create a variable to fill the coordinate
            Cij = z
            for k in 1:l
	            # Calculate the matrix product of the given cell
                @inbounds Cij += A[i,k]*B[k,j]
            end
            C[i,j] = Cij
        end
    end
    C
end
```

Because of the 3 loops the [complexity](Big%20O%20Notation) of the algorithm is $O(N^3)$

## Parallelism
The loop over `j` and `i` ban be done the other way around as well, but the loop $k$ can't because this will change the result in `Cij`.
The addition of floating point numbers doesn't always return the same results due to rounding errors.
```julia
for j in 1:n
    for i in 1:m
        Cij = z
        for k in 1:l
            @inbounds Cij +=  A[i,k]*B[k,j]
        end
        C[i,j] = Cij
    end
end
```

There are different ways to parallelize this algorithm
![[Pasted image 20241002102413.png]]

1. Each worker computes a single entry of C
2. Each worker computes a single row of C
3. Each worker computes $N/P$ rows of C

## Algorithm 1
**Each worker computes a single entry of C**
### Data Dependencies
![[Pasted image 20241002102507.png]]

Each entry of `C[i,j]` needs to know column `i` in $A$ and row `j` in $B$

### Implementation
1. The worker receives the data dependencies from the master process
2. The worker computes the dot product of `A[i,:]` and `B[:,j]` locally
3. The worker sends back the result of `C[i,j]` to the master process
```julia
function matmul_dist_1!(C, A, B)
    m = size(C,1)
    n = size(C,2)
    l = size(A,2)
    @assert size(A,1) == m
    @assert size(B,2) == n
    @assert size(B,1) == l
    # Create a worker of each index
    @assert nworkers() == m*n
    iw = 0    
    # Wait for all the workers to be finished
    @sync for j in 1:n
        for i in 1:m
            Ai = A[i,:]
            Bj = B[:,j]
            iw += 1
            w = workers()[iw]
            # Create a worker process
            ftr = @spawnat w begin
                Cij = z
                for k in 1:l
                    @inbounds Cij += Ai[k]*Bj[k]
                end
                Cij
            end
            # Fill in the value based on the worker output
            @async C[i,j] = fetch(ftr)
        end
    end
    C
end
```
### Complexity
**Communication complexity: $O(N)$**
The $i$-th row has $N$ elements and the $j$-column has also $N$ elements resulting in $O(2N)$.
Because constants can be removed from the notation it results in $O(N)$.

**Computation complexity:** $O(N)$
Same reason as for the communication complexity.

**Ratio communication over complexity:** $O(1)$
$$ O(1) = \frac{O(N)}{O(N)} $$
## Algorithm 2
**Each worker computes a single row of C**
### Data dependencies
![[Pasted image 20241002105432.png]]
Each entry of `C[i,j]` needs to know column `i` in $A$ and row `j` in $B$.
To calculate the whole row of $C$ all the values of `j` from $B$ needed to be send
### Implementation
```julia
function matmul_dist_2!(C, A, B)
    m = size(C,1)
    n = size(C,2)
    l = size(A,2)
    @assert size(A,1) == m
    @assert size(B,2) == n
    @assert size(B,1) == l
    z = zero(eltype(C))
    @assert nworkers() == m
    iw = 0
    @sync for i in 1:m
        Ai = A[i,:]
        iw += 1
        w = workers()[iw]
        ftr = @spawnat w begin
            Ci = fill(z,n)
            for j in 1:n
                for k in 1:l
                    @inbounds Ci[j] += Ai[k]*B[k,j]
                end
            end
            Ci
        end
        @async C[i,:] = fetch(ftr)
    end
    C
    end
```
No for loop for $B$ needed in the main process
### Complexity
**Communication complexity:** $O(N^2)$
The total communication complexity is matrix $B = O(N^2)$ and `A[i,:]` $= O(N)$.
Given us $O(N^2) + O(N) = O(N^2)$.

**Computation complexity:** $O(N^2)$
Same reason.

**Ratio communication over complexity:** $O(1)$
$$ O(1) = \frac{O(N^2)}{O(N^2)} $$


## Algorithm 3
**Each worker computes $N/P$ rows of C**
### Data dependencies
![[Pasted image 20241002110234.png]]
The worker still needs everything from $B$, but also get multiple rows from $A$.
### Implementation
```julia
function matmul_dist_3!(C,A,B)
    m = size(C,1)
    n = size(C,2)
    l = size(A,2)
    @assert size(A,1) == m
    @assert size(B,2) == n
    @assert size(B,1) == l
    @assert mod(m,nworkers()) == 0
    nrows_w = div(m,nworkers())
    @sync for (iw,w) in enumerate(workers())
        lb = 1 + (iw-1)*nrows_w
        ub = iw*nrows_w
        A_w = A[lb:ub,:]
        ftr = @spawnat w begin
             C_w = similar(A_w)
             matmul_seq!(C_w,A_w,B)
             C_w
        end
        @async C[lb:ub,:] = fetch(ftr)
    end
    C
end

@everywhere function matmul_seq!(C,A,B)
    m = size(C,1)
    n = size(C,2)
    l = size(A,2)
    @assert size(A,1) == m
    @assert size(B,2) == n
    @assert size(B,1) == l
    z = zero(eltype(C))
    for j in 1:n
        for i in 1:m
            Cij = z
            for k in 1:l
                @inbounds Cij = Cij + A[i,k]*B[k,j]
            end
            C[i,j] = Cij
        end
    end
    C
end
```
### Complexity
**Communication complexity:** $O(N^2)$
Each worker has the following complexity:
$$ \frac{N}{P} \cdot N + N^2 = \frac{N^2}{P} + N^2 = N^2(1+\frac{1}{P})$$
Only the larger value will stay resulting in $O(N^2)$
**Computation complexity:** $O(\frac{N^3}{P})$
Each worker has to calculate $\frac{N}{P}$ rows from $A$ multiplied by the whole matrix $B$.
$$\frac{N}{P} \cdot N^2 = O(\frac{N^3}{P})$$

**Ratio communication over complexity:**
In this case, the ratio between communication and computation is $O(P/N)$. If the matrix size $N$ is much larger than the number of workers $P$, then the communication overhead $O(P/N)$ would be negligible. This opens the door to an scalable implementation.

## Summary
| Algorithm | Parallelism <br>(#workers) | Communication <br>per worker | Computation <br>per worker | Ratio communication/<br>computation |
|---|---|---|---|---|
| 1 | N² | O(N) | O(N) | O(1) |
| 2 | N | O(N²) | O(N²) | O(1) |
| 3 | P | O(N²) | O(N³/P) | O(P/N) |

- Matrix-matrix multiplication is trivially parallelizable (all entries in the result matrix can be computed in parallel, at least in theory)
- However, we cannot exploit all the potential parallelism in a distributed system due to communication overhead
- We need a sufficiently large grain size to obtain a near optimal speedup
- We measured the theoretical parallel performance by computing the complexity of communication over computation
- We measured the actual performance using the parallel speedup and parallel efficiency

# Jacobi
# All Pairs of Shortest Paths (ASP)
# LEQ
# Traveling Sales Problem