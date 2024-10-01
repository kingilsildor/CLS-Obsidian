		# Modelling Considerations (revisit)
The current models are deterministic (i.e. we know exactly where we going)
If the noise is not to much you still get in the area
If the noise is to big you can push the values on another path

![[Pasted image 20240930093822.png]]

If you run the simulation many times the mean should approach the deterministic models.
You need to understand the variance. 

## Major decisions in designing a model
- Discrete vs continuous time
- Discrete vs continuous state variables
- Deterministic vs stochastic
- Random mixing vs structured population
- Homogeneous vs heterogeneous
	- And which heterogeneity to include?

![[Pasted image 20240930100652.png]]

When you have a question start with the simplest model possible.
### [[Episims]]
Derived from a transsims models
Within transformation simulation they use activity modelling.

CDRS uses 4G or 5G to triangulate your position.
The effort to collect this mobility data is huge!
\
## Deterministic vs stochastic models
Stochastic effects are important when numbers are small, e.g. during invasion of new disease.
If things very between population individual outcomes are not certain, and it adds the oppitunity for envirnmental conditions.

## Importance of stochasticity
If you try to use lockdown as a metric, every noise will have a larger impact on the output because small events can trigger outbreaks.

### [Monte Carlo simulation](Monte%20Carlo%Method)
Any model can be made stochastic by using a pseudo-random number generator to “roll the dice” on whether events occur.

## Chain binomial
A binomial draw to see how the transmission evolves

### Diffusion
Random walk around some values to see what happend to the system

## State variables
We can't really get extension.
If you want you can use a non negative function in *solve_ivp*, but it's more of an hack
In many kind of models we want to thread objects as discrete entities.

## Population
![[Pasted image 20240930101844.png]]
Currently look at random mixing.
If you want multi-group each group has it's own SIR model with different parameters.
When mixing the groups you need parameters between them.

No two individuals are completely alike, resulting in different kind of infectivity.
We don't know how people immune systems are.
If you only know the average just stick to the SIR model.

# Stochastic SIR Equations
Ways to introducte this in the model:
- Adding noise to the deterministic [ODE's]()
- Adding noise to values of the parameters in the deterministic ODE'S
- Individual-level, explicit modelling of random events

[Gillespie algorithm](Gillespie%20Algorithm)

## Random variables
Computers use mostly pseudo-random

Consider a Continous Random Varibale X, whereby X takes values in a continuous interval.
pdf = proability density function
How do the covariance impact the variables?

## SIR features !
1. Variability between simulations
2. Variance and covariance
	- Interaction between stochastic and underlying deterministic nonlinear dynamics can lead to negative covariance between number of susceptible and infectious.
3. Inscreased transients
	- The system moves away from our equilibrium based on the amount of noise. Some noise.
1. Stochastic Resonance
	- If our stochastic align with the natural frequency.
2. Extinctions
	- Even if $R_0 >1$ The disease can still die out. 

## Noise
Observational Noise: we get noise in our data because the recorder data isn't clean enough
Process Noise: Natural noise that can happen within a system.