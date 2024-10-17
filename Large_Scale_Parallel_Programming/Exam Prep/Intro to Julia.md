# What is Julia
[[Julia]] is a general purpose programming language created for scientific computing.
The developers call it the "New Fotran"
Julia uses a [Just-in-time compiler](Compiler) (JIT) to generate specialized native machine code for the [CPU](Central%20Processing%20Unit) or [GPU](Graphics%20Processing%20Unit).

```julia
function f(a,b)
	c = 2 * a
	return b + c
end
```
```bash
julia> @code_native debuginfo=: none f(1,2)
	.text
	.file   "f"
	.global julia_f_125
	.p2align         4, 0x90
	.type   julia_f_125,@function
julia_f_125:
	.cfi_startproc
# %bb.0:
	leaq    (%rsi, %rdi, 2), %rax
	retq
.Lfunc_end0:
	.size   julia_f_125,  .L.func_end0-julia_
	.cfi_endproc

	.section              ".note.GNU-stack", ""
```

You also don't need to declare types. Julia infers them for you.
```julia
julia> @code_typed f(1,2)
CodeInfo(
1 - %1 = Base.mul_int(2, a)::Int64
    %2 = Base.add_int(b, %1)::Int64
         return %2
) => Int64

julia> @code_typed f(1.0,2.0)
CodeInfo(
1 - %1 = Base.mul_float(2.0, a)::Float64
    %2 = Base.add_float(b, %1)::Float64
         return %2
) => Float64
```

C/C++/Fortran complied ahead-of-time.
Because of the JIT compilation the first time a function is called it will take up more time.
You need to compile each time a new datatype is used within a function.
This makes that it has the possibility to be just as fast as C with the ease of use like Python.
```julia
julia> a =[1,2,3,4,2,3];

julia> @time unique(a);
0.017588 seconds (9.16 k allocations: 486.733 KiB, 99.59% compilation)

julia> @time unique(a);
0.000014 seconds (6 allocations: 544 bytes)
```


It figures out all the types based on the given input. This process is called type-inference.
Julia is also a functional orientated programming language with little to no [object-oriented](Object-oriented Programming) support.

## Distributed computing
Julia has different types of support for [distributed computing](Distributed Computing).
You can use the Distributed or de MPI package.
```julia
using Distributed
@everywhere println(
"I am proc $(myid()) from $(nprocess())")

using MPI
MPI.Init()
comm = MPI.COMM_WORLD
rank = MPI.Comm_rank(comm)
nranks = MPI.Comm_size(comm)
println("I am rank $rank of $nranks")
```

| Distributed     | MPI                                                    |
| --------------- | ------------------------------------------------------ |
| More task based | Better performance because of all the related research |
# [[Multi-Threading]]
Julia has its own multi-threading capabilities to divide processes across CPU's. You can also implement [[CUDA]] or [[SIMD]] using Julia.
```julia
using Base.Threads: @threads
a = rand(1000)
b = rand(1000)
@threads for i in 1:length(a)
	a[i] += b[i]
end
```
# Why do we use Julia
The [Galewsky benchmark](Galewsky%20Benchmark) is also a way to test your computing but also your paralysation skills. For this you need Julia.

Because of the scientific framework a lot of research is done to calculate/simulate [Partial Differential Equations](Partial%20Differential%20Equation) (PDEs) using Julia. Like creating source code for PDE
PDEs are solved with computes by transforming them into very large [sparse linear equations](Sparse%20Matrix).
![[Pasted image 20240907152848.png]]
# Assignments
## 1
Implement a function `ex1(a)` that finds the largest item in the array `a`. It should return the largest item and its corresponding position in the array. If there are multiple maximal elements, then the first one will be returned. Assume that the array is not empty. Implement the function in the next cell. Test your implementation with the other one.
```julia
function ex1(a)
    j = 1
    m = a[j]
    for (i,ai) in enumerate(a)
        if m < ai
            m = ai
            j = i
        end
    end
    (m,j)
end
```
## 2
Implement a function `ex2(f,g)` that takes two functions `f(x)` and `g(x)` and returns a new function `h(x)` representing the sum of `f` and `g`, i.e., `h(x)=f(x)+g(x)`.
```julia
ex2(f,g) = x -> f(x) + g(x)
```
## 3 
Function `mandel` estimates if a given point `(x,y)` in the complex plane belongs to the [Mandelbrot set](https://en.wikipedia.org/wiki/Mandelbrot_set).
```julia
function mandel(x,y,max_iters)
    z = Complex(x,y)
    c = z
    threshold=2
    for n in 1:max_iters
        if abs(z)>threshold
            return n-1
        end
        z = z^2 +c
    end
    max_iters
end
```
If the value of `mandel` is less than `max_iters`, the point is provably outside the Mandelbrot set. If `mandel` is equal to `max_iters`, then the point is provably inside the set. The larger `max_iters`, the better the quality of the estimate (the nicer will be your plot).

Plot the value of function `mandel` for each pixel in a 2D grid of the box.

$$(−1.7,0.7)×(−1.2,1.2)$$

Use a grid resolution of at least 1000 points in each direction and `max_iters` at least 10. You can increase these values to get nicer plots. To plot the values use function `heatmap` from the Julia package `GLMakie`. Use `LinRange` to divide the horizontal and vertical axes into pixels. See the documentation of these functions for help. `GLMakie` is a GPU-accelerated plotting back-end for Julia. It is a large package and it can take some time to install and to generate the first plot. Be patient.
```julia
using GLMakie
max_iters = 100
n = 1000
x = LinRange(-1.7,0.7,n)
y = LinRange(-1.2,1.2,n)
heatmap(x,y, (i,j) -> mandel(i,j,max_iters))
```
