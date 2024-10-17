> When a Julia function contains a ***!*** it means that the parameters within the function can be changed.
# Basic functions
| **Function**   | **Description**                                                                                                                       |     |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------- | --- |
| `Tasks()`      | Represents a task or a collection of tasks.                                                                                           |     |
| `yield()`      | Returns the control back to the caller or pauses the current task to resume later.                                                    |     |
| `fetch()`      | Fetch the result from a remote worker.                                                                                                |     |
| `put!()`       | Append an item `v` to the channel `c`. Blocks if the channel is full.                                                                 |     |
| `take!()`      | Removes and returns a value from a `Channel` in order. Blocks until data is available.                                                |     |
| `procs()`      | Returns the processes.                                                                                                                |     |
| `workers()`    | Returns the workers. Similar to processes, but without the main process.                                                              |     |
| `nprocs()`     | Returns the number of processes as an integer.                                                                                        |     |
| `nworkers()`   | Returns the number of workers as an integer.                                                                                          |     |
| `myid()`       | Returns the ID of the current process, with the main process having an ID of 1.                                                       |     |
| `remotecall()` | Execute a give function.                                                                                                              |     |
| `pmap()`       | The `pmap` function in Julia allows you to apply a given function `f` to each element of one or more collections `lsts` in parallel.s |     |
# Asynchronous programming
## Tasks
A task is a piece of computational work that can be started at some point in the future, and that can be interrupted and resumed.
```julia
function work()
	println("Starting work")
	sleep(7)
	a = rand(3,3)
	b = rand(3,3)
	r = a + b
	println("Finishing work")
	r
end

# Create a task
t = Task(work)

# Scedule a task
schedule(t)

# Fetch the result
fetch(t)
```

Besides tasks do not run in parallel.
If tasks do not run in parallel, what is the purpose of tasks? Tasks are handy since they can be interrupted and to switch control to other tasks.
``yield`` gives us the opportunity to switch to another task.

Tasks need variables to run. If we try to create a task with a function that has arguments, it will result in an error when we schedule it.
If we need, we can capture variables in the function to be run by the task.
```julia
a = rand(3,3)
b = rand(3,3)

fun = () -> add(a,b)
t = Task(fun)
schedule(t)
```

## Channels
Channels send data between tasks. It is like a FIFO queue which tasks can put values into and take values from.
You can iterate over channels until the channels closes/
```julia
chnl = Channel{int}()

# If we execute the cell more than 5 times, it will raise an error since the channel is closed.
@async begin
	for i in 1:5
		put!(chnl, i)
	end
	close(chnl)
end

# iter over a channel
for i in chnl
    @show i
end
```
Calling `put!()` blocks the tasks until another task calls `take!()`.

# Distributed Computing
## Processes
The simplest way of creating processes for parallel computing is to add them locally in the current Julia session. This created a separated Julia instance for each process.
If more than one process is available, the first process is called the _main_ or _master_ and the other the _workers_. If only a single process is available, it is the master and the first worker simultaneously.
```julia
using Distributed
addprocs(3)
```
For large parallel computations, one typically needs to use different computers in parallel.
```julia
using Distributed
machines = [("user@server1", 3), ("user@server2"), 4]
addprocs(machines)
```
![[Untitled 1.png]]
Previous way of starting workers in other machines is very low-level. Happily, there is a Julia package called [ClusterManagers.jl](https://github.com/JuliaParallel/ClusterManagers.jl) that helps to create workers remotely in number of usual scenarios. For instance, when running the following code from the login node in a computer cluster, it will submit a job to the cluster queue allocating 128 threads. A worker will be generated for each one of these threads. If the compute node have 64 cores, 2 compute nodes will be used to create to contain the 128 workers (see below).
```julia
using Distributed
using ClusterManagers
addprocs(SlurmManager(128), partition="debug", t="00:5:00")
```
![[Untitled.png]]

## Executing code remotely
The most basic thing we can do with a remote processor is to execute a given function on it. This is done by using function `remotecall`. To make clear how local and remote executions compare, let's call a function locally and then remotely. Next cell uses function `ones` to create a matrix locally. `remotecall` does not return the underlying function, to get the result `fetch` is needed.
It is important to note that `remotecall` does not wait for the remote process to finish. It turns immediately. This can be checked be calling remotely the following function that sleeps for 10 seconds and then generates a matrix.
```julia
proc = 2
ftr = remotecall(ones,proc,2,3)
fetch(ftr)
```
## Data movement
Data movement is a crucial part in distributed-memory computations and it is usually one of its main computational bottlenecks. Being aware of the data we are moving when using functions such as `remotecall` is important to write efficient distributed algorithms in Julia. Julia also provides a special type of channel, called remote channel, to send and receive data between processes.

It can be done explicit or implicit, based on how the functions are structured.
```julia
# explicit
proc = 4
a = rand(10,10)
b = rand(10,10)
ftr = remotecall(+,proc,a,b) # Send
fetch(ftr); # Receive

# implicit
proc = 4
a = rand(10,10)
b = rand(10,10)
fun = () -> a+b
ftr = remotecall(fun,proc) # Send
fetch(ftr); # Receive
```
Another way of moving data between processes is to use remote channels. Their usage is very similar to conventional channels for moving data between tasks, but there are some important differences. In the next cell, we create a remote channel. Process 4 puts several values and closes the channel. Like for conventional channels, calls to `put!` might block, but next cell is not blocking the master process since the call to `put!` runs asynchronously on process 4.

Just like conventional channels, remote channels can be buffered. The buffer is stored in the process that owns the remote channel. By default this corresponds to process that creates the remote channel, but it can be a different one. For instance, process 3 will be the owner in the following example. You can loop over remote channels as an iterable.
```julia
buffer_size = 2
owner = 3
fun = ()->Channel{Int}(buffer_size)
chnl = RemoteChannel(fun,owner)

@spawnat 4 begin
    println("start")
    for i in 1:5
        put!(chnl,i)
        println("I have put $i")
    end
    close(chnl)
    println("stop")
end;

take!(chnl)
```

If we use `pmap` instead of broadcast, the different calls to `svd` will be distributed over the available processes.
```julia
@time pmap(svd,a);
```
## Limitations
This is a very common pitfall when running parallel code. If we define a function in a process, it is not automatically available in the other processes.

If a function has a name, Julia only sends the function name to the corresponding process. Then, Julia looks for the corresponding function code in the remote process and executes it. This is why the function needs to be defined also in the remote process. However, if a function is anonymous, Julia needs to send the complete function definition to the remote process. This is why anonymous functions do not need to be defined with the macro `@everywhere` to work in a remote call.

Each processor has their own package management.
## Data Transfer assignment
> **Question (NB3-Q1):** How many integers are transferred between master and worker? Including both directions
```julia
a = rand(Int,4,4)
proc = 4
@fetchfrom proc sum(a^2)
```
a) $17$
b) $32$
c) $16^2$
d) $65$

17 because a matrix of $4^2$ is send to the worker and it returns the sum of this matrix as value back

> **Question (NB3-Q2):** How many integers are transferred between master and worker? Including both directions.
```julia
a = rand(Int,4,4)
proc = 4
@fetchfrom proc sum(a[2,2]^2)
```
a) $2$
b) $17$
c) $5$
d) $32$

17 Even though we only use a single entry of the matrix in the remote worker, the entire matrix is captured and sent to the worker. Thus, we will transfer 17 integers like in Question 1.

>**Question (NB3-Q3):** Which value will be the value of `x` ?
```julia
a = zeros(Int,3)
proc = 3
@sync @spawnat proc a[2] = 2
x = a[2]
x
```

The value of x will still be zero since the worker receives a copy of the matrix and it modifies this copy, not the original one.

>**Question (NB3-Q4):** Which value will be the value of `x` ?
```julia
a = zeros(Int,3)
proc = myid()
@sync @spawnat proc a[2] = 2
x = a[2]
x
```

in this case, the code a(2)=2 is executed in the main process. Since the matrix is already in the main process, it is not needed to create and send a copy of it. Thus, the code modifies the original matrix and the value of x will be 2.
# Macro's

| Macro          | Description                                                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `@async`       | Put the code in an anonymous function, create a task and schedule it.                                                          |
| `@sync`        | Let the code wait for all the tasks to be finished                                                                             |
| `@spawnnat`    | generates an auxiliary function from the given block of code and calls `remotecall` for us.                                    |
| `@fetchfrom`   | The blocking version of `@spawnat`. It blocks and returns the corresponding result instead of a `Future` object.               |
| `@distributed` | Distributes the iterations of the loop across the available processes, allowing Julia to perform the calculations in parallel. |
## @spawnnat
```julia
@spawnat proc ones(2,3)
# is the same as
fun = () -> ones(2,3)
remotecall(fun,proc)
```
The relation between `@async` and `@spawnat` is obvious. From the user perspective they work almost in the same way. However, `@async` generates a task that runs asynchronously in the current process, whereas `@spawnat` executes a task in a remote process in parallel. In both cases, the result is obtained using `fetch`.

## @distributed
```julia
function compute_π(n)
    s = 1.0
    for i in 1:n
        s += (isodd(i) ? -1 : 1) / (i*2+1)
    end
    4*s
end

function compute_π_dist(n)
    s = 1.0
    r = @distributed (+) for  i in 1:n
        (isodd(i) ? -1 : 1) / (i*2+1)
    end
    4*(s+r)
end
```
Paralelizing this function might require some work with low-level functions like `remotecall`, but it is trivial using the macro `@distributed`. This macro runs the for loop using the available processes and optionally reduces the result using a given reduction function (`+` in this case).

# Summary
In order to start "thinking in parallel" you first need to be familiar with concepts of asynchronous programming, in particular tasks. In this notebook, we have seen the basics of working with tasks. Some key points to remember:

- How to create, schedule, and fetch from a task.
- Tasks run asynchronously, but not in parallel. You can have a single core CPU and still be able to work with several tasks.
- Channels are used to communicate data between tasks.
- Adding data (`put!`) or taking data (`take!`) from a channel might wait depending on the channel state. Be careful to avoid dead locks.

We have seen the basics of distributed computing in Julia. The programming model is essentially an extension of tasks and channels to parallel computations on multiple machines. The low-level functions are `remotecall` and `RemoteChannel`, but there are other functions and macros like `pmap` and `@distributed` that simplify the implementation of parallel algorithms.

# Assignments
Implement this "simple" algorithm (the telephone game):

Worker 1 generates a message (an integer). Worker 1 sends the message to worker 2. Worker 2 receives the message, increments the message by 1, and sends the result to worker 3. Worker 3 receives the message, increments the message by 1, and sends the result to worker 4. Etc. The last worker sends back the message to worker 1 closing the ring. See the next figure.
![[Untitled 3.png]]
```julia
f = () -> Channel{Int}(1)
chnls = [ RemoteChannel(f,w) for w in workers() ]
@sync for (iw,w) in enumerate(workers())
    @spawnat w begin
        chnl_snd = chnls[iw]
        if w == 2
            chnl_rcv = chnls[end]
            msg = 2
            println("msg = $msg")
            put!(chnl_snd,msg)
            msg = take!(chnl_rcv)
            println("msg = $msg")
        else
           chnl_rcv = chnls[iw-1]
           msg = take!(chnl_rcv)
           msg += 1
           println("msg = $msg")
           put!(chnl_snd,msg)
        end
    end
end
```