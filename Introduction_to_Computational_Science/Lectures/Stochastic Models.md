# Stochastic Models
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

## Comparison
Stochastic effects are important when numbers are small, e.g. during invasion of new disease.
If things very between population individual outcomes are not certain, and it adds the opportunity for environmental conditions.

With deterministic models one can calculate every point based on the given initial condition before hand. 

## Importance classes of stochasticity
- ***[Monte Carlo simulation](Monte%20Carlo%Method)***
	- Any model can be made stochastic by using a pseudo-random number generator to "roll the dice" on whether events occur.
- ***Branching process***
	- Model of invasion in a large susceptible population.
	- Allows flexibility in distribution of secondary infections but does not account for depletion of susceptibles.
- ***Chain binomial***
	- Model of an epidemic in a finite population.
	- For each generation of transmission, calculates new infected individuals as a binomial random draw from the remaining susceptibles.
- ***Diffusion***
	- Model of an endemic disease in a large population.
	- Number of infectious individuals does a random walk around its equilibrium value -> quasi-stationary distribution.


## State variables
We can't really get extension.
If you want you can use a non negative function in *solve_ivp*, but it's more of an hack
In many kind of models we want to thread objects as discrete entities.

***Continuous state variables*** arise naturally in differential equation models.
- Mathematically tractable, but biological interpretation is vague
- Ignoring discreteness of individuals can yield actuarial model results.
- Quasi-extinction threshold: assume that population goes extinct if continuous variable drops below a small value
***Discrete state variables*** arise naturally in many stochastic models, which treat individuals explicity.

## Population
![[Pasted image 20240930101844.png]]
Currently look at random mixing.
If you want multi-group each group has it's own SIR model with different parameters.
When mixing the groups you need parameters between them. You also need to know which heterogeneities are important for the question. 

No two individuals are completely alike, resulting in different kind of infectivity.
We don't know how people immune systems are. If you only know the average just stick to the SIR model.

Groups are often modelled using multi-group models, but other options such as networks, ABM's and [PDE's](Partial%20Differential%20Equation) are also usefull.

# Stochastic SIR Equations
Within the deterministic SIR equation the dynamics were always the same based on the initial conditions. But this is often not realistic for real epidemics.
The role of chance is important within these stochastic models:
- **For small population sizes**
- **For small number of infectious.**

Ways to introduce this in the model:
- Adding noise to the deterministic [ODE's]()
- Adding noise to values of the parameters in the deterministic ODE'S
- Individual-level, explicit modelling of random events

The [Gillespie algorithm](Gillespie%20Algorithm) is an way to reactions whereby the rates are known.

## Random variables
Computers use mostly pseudorandom number generators, hereby the number is based on a given seed.
Random variables can be generated multiple ways given a Continuous Random Variable $X$:
- ***Probability Density Function***
- ***Cumulative Distribution Function***
- 

Consider a Continous Random Varibale $X$, whereby $X$ takes values in a continuous interval. The probbility that $X$ lies in the interval $[a, b]$ is $P (a \leq X \leq b)= \int_{-\infty}^xf(t)dt$.
This is the ***proability density function (pdf)*** of $X$, with $f(x) > 0$ and $\int f(x)dx =1$

#TODO 


## SIR features
The number of susceptibles $X$, the number of infectious $Y$, and the number of recovered $Z$ are now random variables. Because of this they have an underlying **pdf**.


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
If you try to use lock-down as a metric, every noise will have a larger impact on the output because small events can trigger outbreaks.

***Observational Noise:*** we get noise in our data because the recorder data isn't clean enough
***Process Noise***: Natural noise that can happen within a system.

We can add Process noise to equations. 
Within the SIR equation this can be done by changing contacts per unit time $\kappa$ or probability of disease transmission upon contact $c$.

Consider $\frac{dx}{dt} = \xi$ where $\xi$ is Gaussian random variable with mean $0$ and $\sigma =1$.
You can solve this with forward [[Euler Method]]:
$$
\begin{align}
x_{t+\delta t} &= x_t + \delta t \frac{dx}{dt} \\
&= x_t + \delta t \xi \\
&= x_0 + \delta t \sum^{t/\delta t}_1 \xi
\end{align}
$$
For $\delta t \rightarrow 0$ the noise terms effectively cancel and the variance in $x$ will be zero. This reflects the problem with adding noise terms to [ODE's](Ordinary%20Differential%20Equation).
The trick to resolve this is to scale the noise with the time steps, $\xi = \frac{\xi}{\sqrt{\delta t}}$ (diffusive scaling) and write $\frac{dx}{dt} = f\xi$.
In this way, in the long run, the mean of $x$ is zero, and the $\sigma$ grows like $f \sqrt{\delta t}$, so diffusive behavior.

We first assume that the variables themselves fluctuate, to explore the impact of stochasticity on the dynamics.
Based on this we use the following steps:
1. Consider constant noise in the transmission term
2. Look at properly scaled noise in all rates in the model

### Noise in the transmission term
To add noise in the transmission term, we will use a function $f(X, Y)$ to scale the randomness in response to the current variable sizes.
$$
\begin{align}
\frac{dX}{dt} &= \nu N - \left[ \beta XY / N + f(X, Y)\xi \right] - \mu X \\
\frac{dX}{dt} &= \left[ \beta XY / N + f(X, Y)\xi \right] - \gamma Y - \mu Y \\
\frac{dZ}{dt} &= \gamma Y - \mu Z
\end{align}
$$
![[Pasted image 20241021172713.png]]

### Scaled Noise
W have assumed that all the rates in the SIR model are due to [Poisson processes](Poisson%20Distribution), for which we know that the mean value equals the variance, leading to $f = \sqrt{rate}$
We can use this to generalize all the rates within the SIR model.
$$
\begin{align}
\frac{dX}{dt} &= \left[\nu N + \sqrt{vN} \xi_1 \right] - \left[ \beta XY / N + \sqrt{\beta X Y / N}\xi_2 \right] - \left[\mu X + \sqrt{\mu X} \xi_3 \right]\\
\frac{dX}{dt} &= \left[ \beta XY / N + \sqrt{\beta X Y / N} \xi_2 \right] - \left[\gamma Y + \sqrt{\gamma Y} \xi_4 \right] - \left[\mu Y + \sqrt{\mu Y} \xi_5 \right]\\
\frac{dZ}{dt} &= \left[\gamma Y + \sqrt{\gamma Y} \xi_4 \right] - \left[ \mu  Z + \sqrt{\mu Z} \xi_6 \right] \\
\\

\xi_1 &= \text{Birth rate noise} \\
\xi_2 &= \text{Infection rate noise} \\
\xi_3 &= \text{Susceptible death rate noise} \\
\xi_4 &= \text{Recovery rate noise} \\
\xi_5 &= \text{Infected death rate noise} \\
\xi_6 &= \text{Recovered death rate noise}
\end{align}
$$

## Advantages & disadvantages of stochastics
### Advantages:
1. We do some minor modifications to the existing equations
2. The correspondence between the deterministc and stochastic equations is clear.
3. When the noise is $0$ the model behaves like the deterministic one
4. The computational overhead is small

### Disadvantages
1. The models to not incorporate the discrete, individual nature of populations
2. Not suitable if number of initial infected is small

# Event Driven Approaches
When looking at the data measles dies out between outbreaks in Iceland, but not in the UK or Denmark.
![[Pasted image 20241021174140.png]]

Because Iceland is an island, the population will be fully recovered after a certain period.
Only the influx of new people to the island will stark a new outbreak (i.e. via boat).
This phenomena is called an event driven approach.

