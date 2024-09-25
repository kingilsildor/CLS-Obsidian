# [Jacobi method](Jacobi%20Iteration%20Method)
## 1D
[Laplace's equation](Laplace's Equation)

The time complexity of the algorithm is $O(n)$

When parallelizing a block partition is used. This results that you need some communication.

The communication is not taking most of the time

You need to update the ghost cell for the new values

## 2D
You can use the method as a convolution

# [The Gauss-Seidel method](The%20Gauss-Seidel%20Method)
More efficiently in memory, because you save one vector 
Inner loop can't be parallelized, because we are changing and consuming $u$ at the same time.

When using a red-black gauss-seidel you can run it parallel because when you update 1 colour you are not modifying the other colour. 

view -> update value directly 