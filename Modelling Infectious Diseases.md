# Foundations of Infectious Disease Modeling
## Why do we [model](Model) infectious diseases
1. To gain insight into **mechanisms** influencing disease spreading.
2. To **focus the thinking** process on clear statement of assumptions & hypotheses. 
3. To derive **new insights and hypotheses** from mathematical analysis or [simulation](Simulation).
4. To establish **relative importance** of different processes and parameters.
5. To create **though experiments** and what if questions, because some experiments are often logistically or ethically impossible.
6. To explore **management options**.

We try not to predict future trends but approximate them using computational methods.

## Definitions

|                                    |                                                         | Example   |
| ---------------------------------- | ------------------------------------------------------- | --------- |
| [Acute infections](Epidemiology)   | Fast infections, where the pathogons are quicly removed | Influenza |
| [Chronic infections](Epidemiology) | Last for much longer periods                            | Herpes    |

## SIR Framework
SIR Framework is an example of compartmental models, where the population is assigned to different compartments with labels. For SIR this means (**S**usceptible, **I**nfectious, or **R**ecovered/**R**emoved).
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

## Variants
You can adapt a model framework to different kind of diseases and other problems.
No need to restrict to 1 model (SEIR for example) if the biology suggest otherwise.
![[Pasted image 202410041046555.png]]
![[Pasted image 202410041047095.png]]
![[Pasted image 202410041047235.png]]
![[Pasted image 202410041047355.png]]

| Model  | Usecase                              |
| ------ | ------------------------------------ |
| SEI    |                                      |
| SIRS   | Loss of immunity                     |
| SIS    | Acohol addiction                     |
| SEIR   | Encubation period                    |
| SICR   |                                      |
| SIRX   | Disease with environmental reservoir |
| SIR-SI | Vector-borne disease                 |
## Residence Times
Based on the SEIR model.
Exposed time when you are not yet infectious, during this time it's difficult to detect symptoms.
Given:
$$
\begin{align}
\frac{dE}{dt} = -\nu E \rightarrow E(t)=E_0e^{-\nu t} \\
\end{align}
$$
For a constant per capita rate of leaving compartment, the residence time in the compartment is exponentially distributed.
Based on the model one will see the following residence time:
![[Pasted image 20241006110715.png]]

But real life suggests something else:
![[Pasted image 20241006110734.png]]

To make this model better fit the data a "Box-car model" is used.
The 'exposed' compartment is divided into $n$ sub-compartments, each with a constant leaving rate of $\nu/n$.
This makes that the residence time is gamma-distributed, with same mean and flexible variance depending on the number of sub-compartments.
![[Pasted image 20241006112939.png]]

## Numerical solvers
For any numerical method, we need to create a discretization of our variables.
### [[Euler Method]]
A first-order numerical procedure for solving [ODEs](Ordinary Differential Equation) with a given initial value.
It's a simple way to solve ODE's, but not very accurate or stable.

$$
\frac{dy}{dt} = f(t,y)
$$
We will discretize the time axes $t_n = n \delta t$ with small time steps of $\delta t$.
If $\delta t = 0.25$ then:

| $t_n$ |      |
| ----- | ---- |
| $t_0$ | 0    |
| $t_1$ | 0.25 |
| $t_2$ | 0.5  |
| $t_3$ | 0.75 |
| $t_4$ | 1    |
We approximate:
$$
\begin{align}
\frac{dy}{dt} = \frac{y_{n+1}-y_n}{\delta t} = f(t_n, y_n) \\
y_{n+1} = y_n + \delta t \cdot f(t_n, y_n)
\end{align}
$$
The error between each time step is $\delta t^2$

![[Pasted image 20241006145604.png]]

Given a constant matrix $C$, the system will follow: 
$$
\frac{dy}{dt} = -Cy
$$
The Forward Euler becomes:
$$
\begin{align}
y_{n+1} = y_n - \delta t C \cdot y_n \\ 
y_{n+1} = y_n - (I-\delta t C)y_n \\
y_{n} = (I-\delta t C)^n y_0
\end{align}
$$
A matrix $A^n$ tends to zero for $n \rightarrow \infty$ only if the largest eigenvalue of $A$ has a magnitude smaller than 1.
So, $y_n$ is bounded as $n \rightarrow \infty$ only if the largest eigenvalue of $I - \delta t C$ is less then 1.
With $\lambda_{\text{max}}$ the large eigenvalue of $C$ this results in:
$$ \delta t < \frac{2}{\lambda_{\text{max}}} $$
So the timesteps must be small enough
### [[Runge-Kutta]]

# The Mathematics of Disease Spread
## Naive SIR
## SIR Equation (Without Demography)
## S -> I Transition
## Mass vs Density
## I -> R Transition
## Frequency-Dependent Transmission and Seasonality
## SIR Equation (With Demographics)


# Dynamics and Analysis of Disease Outbreaks
##  System Dynamics / Stock Flow
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

## Phase Plots
## Epidemic Threshold & Burnout
## Fixed Points
## Stability Analysis
## Oscillatory Dynamics
