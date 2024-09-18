# What is Computational Science?
A short explanation of Computational Science is doing science with/using computers.
Whereby science is the activity to understand the world around us using observation and experiments.

You can look at nature as it is or to all the different systems that are in place within nature, for example the [mathmetical models](Mathematical%20Model.md).
$$
\begin{align*}
\frac{dx}{dt} &= \alpha x - \beta xy \\
\frac{dy}{dt} &= \delta xy - \gamma y \\
\end{align*}
$$
$$\text{Lotka–Volterra equations}$$

![[Modelling & Simulation Cycle.png]]
The [modelling & simulation cycle](Modelling%20&%20Simulation%20Cycle.md) to recreate patterns from data analysis. 

The [difference between Data Science and Computational Science](Computational%20Science%20VS%20Data%20Science) is: Computational Science is more than prediction but understanding, or asking why something happens.
Whereby Data Science is based on the philosophy of [[Francis Bacon]], Computational Science is based on the ideas created by [[Karl Popper]].
People can predict but that doesn't mean that they understand the underlining principles.
A model can and should be changed when the result doesn't match the theory.
To really understand a system you need to not only predict future things but also know how the system will be effected by changes.

# What is a System?
A [system](System) is an isolated part of reality that we wish to study through scientific inquiry. It's a potential source of data with it's own structure and behaviour.

|                      |                    |
| -------------------- | ------------------ |
| **System Structure** | The inner workings |
| **System Behaviour** | The in/output      |

# What is an Experiment?
An [experiment](Experiment) is the process of extracting data from a given system by exerting it through its inputs. Whereby the experimental frame are the specific conditions under which the system observed is experimented with.
# What is a Model?
	“...a physical, conceptual, or mathematical representation of a real phenomenon that is difficult to observe directly. Scientific models are used to explain and predict the behaviour of real objects or systems and are used in a variety of scientific disciplines, ranging from physics and chemistry to ecology and the Earth sciences ... scientific models at best are approximations of the objects and systems that they represent—they are not exact replicas.”

A [model](Model) is an informative abstraction of an real world object. This can be a physical, conceptual or mathematical representation.
Because of the abstraction all models are wrong, but some are more useful than other models.
You have to be aware of the assumptions you make when creating your model and to have the possibility to validate them.
A model is always related to the tuple of System and Experiment.
In many cases it's more natural to first create a model of a system and next perform the experiments on the model.

Some models can be solved analytically, other models need a simulation to do that.
# What is a Simulation?
A [simulation](Simulation) is an experiment performed on a mathematical model.
Creating simulations is sometimes cheaper and more ethical than doing real world experiments. 
But be aware of the assumptions, because simulations can go wrong.
# Why do we Model?
To:
1. Explain
2. Predict
3. Understand
4. Discover new questions
5. enz..
# Overview of Mathematical Models
Different ways to look at the relationships between the input and time
## [[Continuous-Time Models]]
The state of a system changes continuously over time and can be represented by differential equations.
Lumped parameter models: [ODE](Ordinary%20Differential%20Equation)
Distributed parameter models: [PDE](Partial%20Differential%20Equation)
## [[Discrete-Time Models]]
Can arise from discrete versions of continuous-time models. The state is discretised and "jumps" in time. Events can happen at any time but only within the time intervals. These models can also be represented by differential equations.
You need to have the timesteps small enough to capture the evolution in the model. Not to small because that will take to much computational power.
## [[Discrete-Event Models]]
State change changes at certain points in time and ‘jumps’ in time without a fixed time. 
Events can happen at any time as a que of events.