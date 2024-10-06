# Basic functions
`procs()`:
return the processes.

`workers()`:
return the workers.
Similar to the processes, but without the main process.

`nprocs()`
Return the amount of processes as an int

`nworkers()`
Return the amount of workers as an int.

`myid()`
Return the ID of the current process.
Main will be 1.

# Create workers
```julia
using Distributed
machines = [("user@server1",3),("user@server2",4)]
addprocs(machines)
```

```julia
using Distributed
using ClusterManagers
addprocs(SlurmManager(128), partition="debug", t="00:5:00")
```

# Run code remotely
```julia
proc = 2
ftr = remotecall(ones,proc,2,3)
fetch(ftr)
> 2×3 Matrix{Float64}:
> 1.0  1.0  1.0
> 1.0  1.0  1.0
```

Runs function ones with parameters `2` and `3` on process `2`.
When running on separate processes call the function `fetch()` to get the result.
Remotecall is hereby asynchronous.

# Macro's
## @spawnat
Creates a wrapper around the `remotecall` function, making it less tedious. 
```julia
@spawnat proc ones(2,3)
```
is the same as
```julia
fun = () -> ones(2,3)
remotecall(fun,proc)
```
`@spawnat` executes a task in a remote process in parallel.

@sync

## @async
`@async` generates a task that runs asynchronously in the current process. Most of the time this will be main.

## @fetchfrom
Combines `@spawnat` and `@fetchfrom`, so the output of the macro returns what `fetch()` would returned.
```julia
a = @fetchfrom proc begin
    sleep(3)
    zeros(2)
end

> 2-element Vector{Float64}:
> 0.0
> 0.0
```

@inbound

