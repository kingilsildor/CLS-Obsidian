# Modelling Infectious Diseases
[Why do we model](Model)? (*Heesterbeek & Roberts (1995)*)
1. To gain insight into **mechanisms** influencing disease spreading.
2. To **focus the thinking** process on clear statement of assumptions & hypotheses. 
3. To derive **new insights and hypotheses** from mathematical analysis or [simulation](Simulation).
4. To establish **relative importance** of different processes and parameters.
5. To create **though experiments** and what if questions, because some experiments are often logistically or ethically impossible.
6. To explore **management options**.

We try not to predict future trends but approximate them.
# [[System Dynamics]]
System dynamics is an approach to analyse the nonlinear behaviour of complex systems over time by incorporating elements such as stocks, flows, feedback loops, table functions, and time delays.

![[images.png]]
**[Stock](Stock%20&%20Flow):** a variable that can be quantified at a point in time.
**[Flow](Stock%20&%20Flow):** a variable to measure the movement from stock A to stock B over an interval.

Stock flow models can be transformed into solving [ODE's](Ordinary%20Differential%20Equation).
The stock represents some quantity $Q(t)$ 
Its time derivative $\frac{dQ(t)}{dt}$ is the rate of change and should equal the total flows in - and out of the stock.
$$\frac{dQ(t)}{dt} = \sum_{i=1}^{n}{F_i}$$
With $F_i$ all flows into the stock or out of the stock.
For coupled stock this results into sets of coupled ODE's, which can be nonlinear if flows are nonlinear in the stocks themselves.
# Naïve SIR
[Acute infections](Epidemiology) -> Fast infections
[Chronic infections](Epidemiology) -> Last for much longer periods

Assumption is that the host develops a life long resistance within the [Naïve SIR model](SIR%20Model)
![[Pasted image 20240915202109.png]]

$$\begin{align*}
\lambda \rightarrow \text{Force of Infection} \\
\gamma \rightarrow \text{Rate of Recovery} \\
= 1/\text{infectious period}
\\\\
X = \text{total amount of susceptible} \\
Y = \text{total amount of infected}
\end{align*}
$$

In [discrete time](Discrete-Time%20Models) every timestep an agent has a probability to get infected/recovered
In [continuous time](Continuous-Time%20Models) the infected/recovered variable is a probability per unit of time
## Transmission fases
### I → R
**Discrete setting:** per time step $\gamma Y$ individuals move from the Infected to Recovered
**Continuous setting:** There is a rate $\gamma Y$ individuals per time unit moving from the Infected to Recovered.

### S → I
constant rate $\lambda$, so the rate per time unit becomes $\lambda X$.
In this approach the amount of infected people aren't taking into account, that makes the [model](Model) not relatable to real life. 

## Equations
$$
\begin{align*}
\frac{dX}{dt} &= -\lambda X \\
\frac{dY}{dt} &= +\lambda X - \gamma Y \\
\frac{dZ}{dt} &= +\gamma Y \\
\end{align*}
$$
## Naïve SIR, example of dynamics
![[Pasted image 20240915211139.png]]
## Naïve SIR phase plot
![[Pasted image 20240915211156.png]]
# [ODE](Ordinary%20Differential%20Equation) solution
# SIR Model (without demography)
The transition from **S → I** is determined by:
1. The amount of infectious individuals
2. The underlying population contact structure
3. The probability of transmission given contact

The force of infection $\lambda$ is the per capita rate at which susceptible are getting infected, so the rate at which infected are produced is $\lambda X$
# SIR Model (with demography)
# Introduction to Numerical Solvers
# SIR Variants
