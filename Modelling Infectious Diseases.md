# Foundations of Infectious Disease Modeling
## Why do we model infectious diseases
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
![[Pasted image 20241004104655.png]]
![[Pasted image 20241004104709.png]]
![[Pasted image 20241004104723.png]]
![[Pasted image 20241004104735.png]]

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

- SIR Framework
- Numerical Solvers
- SIR Variants (basics) & Residence Times


# The Mathematics of Disease Spread
- Naive SIR and Solution
- SIR Equation (Without Demography)
- S -> I Transition (Mass vs Density)
- I -> R Transition
- Frequency-Dependent Transmission
- SIR Equation (With Demographic)

# Dynamics and Analysis of Disease Outbreaks
#  System Dynamics / Stock Flow
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


- System Dynamics /  Stock Flow
- Phase Plot
- Epidemic Threshold & Burnout
- Fixed Points
- Stability Analysis
- Oscillatory Dynamics

#
$\lambda$
