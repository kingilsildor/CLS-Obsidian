# What is Julia?
[[Julia]] is a general purpose programming language created for scientific computing.
The developers call it the "New Fotran"
Julia uses a [Just-in-time compiler](Just-in-time Compiler) (JIT) to generate specialized native machine code for the CPU or GPU.
C/C++/Fortran complied ahead-of-time.
Because of the JIT compilation the first time a function is called it will take up more time.
You need to compile each time a new datatype is used within a function.
This makes that it has the possibility to be just as fast as C with the ease of use like Python.

It figures out all the types based on the given input. This process is called type-inference.
Julia is also a functional orientated programming language with little to no [object-oriented](Object-oriented Programming) support.

Julia has different types of support for [distributed computing](Distributed Computing).
You can use the Distributed or de MPI package.

| Distributed     | MPI                                                    |
| --------------- | ------------------------------------------------------ |
| More task based | Better performance because of all the related research |
|                 |                                                        |
Julia has its own [multi-threading](Multi-threading) capabilities to divide processes across CPU's. You can also implement [[CUDA]] or [[SIMD]] using Julia.


# Why do we use Julia
The [Galewsky benchmark](Galewsky%20Benchmark) is also a way to test your computing but also your paralysation skills. For this you need Julia.

Because of the scientific framework a lot of research is done to calculate/simulate [Partial Differential Equations](Partial%20Differential%20Equation) (PDEs) using Julia. Like creating source code for PDE
PDEs are solved with computes by transforming them into very large [sparse linear equations](Sparse%20Matrix).
![[Pasted image 20240907152848.png]]

## PartitionedArrays