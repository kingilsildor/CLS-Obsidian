# Foundations of Infectious Disease Modeling
## Why do we [model](Model) infectious diseases
> *Heesterbeek & Roberts (1995)*

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

## [[System Dynamics]]
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
## Transmission fazes
### I → R
**Discrete setting:** per time step $\gamma Y$ individuals move from the Infected to Recovered.
The probability to recover at time $t$ is then $p(t) = \gamma(1-\gamma)^{(t-1)}$.

**Continuous setting:** There is a rate $\gamma Y$ individuals per time unit moving from the Infected to Recovered.
We assume that recovery is a rate based on a Poisson process.

### S → I
We assume that S -> I transition is governed by a constant rate $\lambda$  so the rate per time unit becomes $\lambda X$, with $X$ being the number of susceptible individuals. 

In this approach the amount of infected people aren't taking into account, that makes the [model](Model) not relatable to real life. So new infected people can arise without other infected.

For this it's important to add a way to calculate the amount of infected with it.

## Equations
$$
\begin{align}
(1.1)\;\frac{dX}{dt} &= -\lambda X \\
(1.2)\;\frac{dY}{dt} &= \lambda X - \gamma Y \\
(1.3)\;\frac{dZ}{dt} &= \gamma Y \\
\end{align}
$$
$$
\begin{align*}
X &= \text{total amount of susceptible} \\
Y &= \text{total amount of infected} \\
Z &= \text{total amount of removed} \\
\end{align*}
$$
$$
\begin{align*}
&\frac{dX}{dt} + \frac{dY}{dt} + \frac{dZ}{dt} = 0
\end{align*}
$$

## Fixed points
$$
\begin{align*}
X^*, Y^*, Z^* \\
\frac{dX^*}{dt} &= 0 \rightarrow -\lambda X^* = 0\\
\frac{dY^*}{dt} &= 0 \rightarrow \lambda X^* - \gamma Y^* = 0\\
\frac{dZ^*}{dt} &= 0 \rightarrow \gamma Y^* = 0\\

X^* &= 0\;\; Y^*= 0\;\; Z^*=N
\end{align*}
$$
## Solve the model
Assume initial conditions:
$$
\begin{align*}
X(0)=X_0 \;\;Y(0)=N-X_0\;\;Z(0)=0 \\
\end{align*}
$$
Look eq (1.1)
$$
\begin{align*}
\frac{dX}{dt} &= -\lambda X \\
X(t)&=X_0e^{-\lambda t}
\end{align*}
$$
substitute in eq (1.2)
$$
\begin{align*}
\frac{dY}{dt} &= \lambda X_0e^{-\lambda t}-\gamma Y\\
Y &= \alpha
\end{align*}
$$
Try solution 
$$
\begin{align*}
Y &= \alpha e^{-\lambda t} + \beta e^{-\gamma t} \\
-\lambda \alpha e^{-\lambda t} - \gamma\beta e^{-\gamma t} &= \lambda X_0 e^{- \lambda t} - \gamma [\alpha e^{-\lambda t} + \beta e^{- \gamma t}] \\
-\lambda \alpha e^{-\lambda t} &= \lambda X_0 e^{- \lambda t} - \gamma\alpha e^{-\lambda t} \\
-\lambda \alpha &= \lambda X_0 - \gamma\alpha \\
-\gamma \alpha - \lambda \alpha &= \lambda X_0 \\
-\alpha(\gamma-\lambda)&= \lambda X_0 \\
\alpha &= \frac{\lambda X_0}{-\gamma - \lambda}
\end{align*}
$$
$$
\begin{align*}
Y(0)&=N-X_0 \\
Y &= \alpha e^{-\lambda t} + \beta e^{-\gamma t} \\
Y(0) &= \alpha + \beta \\
\alpha + \beta &= N - X_0 \\
\frac{\lambda X_0}{-\gamma - \lambda} + \beta &= N - X_0 \\
\beta &= N - X_0 - \frac{\lambda X_0}{\gamma - \lambda} \\
\beta &= \frac{\gamma(N-X_0) - \lambda N}{\gamma - \lambda} \\
\end{align*}
$$
$$
\begin{align*}
Y &= \frac{\lambda X_0}{-\gamma - \lambda} e^{-\lambda t} + \gamma (N - X_0) - \frac{\lambda N}{\gamma - \lambda} e^{-\lambda t} \\
(2)\;\; Y &= e^{-\lambda t} \left[ \frac{\lambda X_0}{\gamma - \lambda} + \frac{\gamma(N-X_0) - \lambda N}{\gamma - \lambda}e^{-(\gamma-\lambda)t} \right] \\
\xi &= \gamma - \lambda \;\; \text{note:} \lim_{\gamma \rightarrow \lambda} = \lim_{\xi \rightarrow 0}

\end{align*}
$$
Substitute $\xi$ in eq (2)
$$
\begin{align*}
Y &= e^{-\lambda t} \left[ \frac{\lambda X_0}{\xi} + \frac{(\xi+\lambda)(N-X_0) - \lambda N}{\xi}e^{-\xi t} \right] \\
Y &= e^{-\lambda t} \left[ \frac{\lambda X_0}{\xi} + \frac{\xi(N-X_0)-\lambda X_0}{\xi}e^{-\xi t} \right] \\
\end{align*}
$$
Taylor Expansion
$$
\begin{align*}
e^{-\xi t} &= \sum^{\infty}_{i=0}\frac{(-1)^i}{i!}(\xi t)^i \\
e^{-x}&= 1 - \frac{x^1}{1!}+\frac{x^2}{2!}-\frac{x^3}{3!}+...\\
\xi &\rightarrow 0\\
\frac{\xi(N-X_0)-\lambda X_0}{\xi} &- \frac{\xi(N-X_0)-\lambda X_0}{\xi} \xi  t + \frac{\xi(N-X_0)-\lambda X_0}{\xi} \sum^{\infty}_{i=2} \frac{(-1)^i}{i!}(\xi t)^i \\
Y &= e^{-\lambda t}  \left[ \frac{\lambda X_0}{\xi} + \frac{\xi(N-X_0)-\lambda X_0}{\xi} - \frac{\xi(N-X_0)-\lambda X_0}{\xi} \xi  t + .... \right] \\
N - X_0 &= \frac{\lambda X_0}{\gamma-\lambda} + \frac{\gamma (N-X_0) - \lambda N}{\gamma - \lambda} \\
Y &= e^{-\lambda t} \left[ (N-X_0) - \xi(N-X_0) - \lambda X_0 \right]t + \xi (N-X_0) - \lambda X_0 \sum^{\infty}_{i=2} \frac{(-1)^i}{i!}(\xi t)^i\\
\end{align*}
$$
In the limit $\xi \rightarrow 0$:
$$
Y = e^{-\lambda t} \left[ (N-X_0) + \lambda X_0 t \right]
$$

## Naïve SIR, example of dynamics
![[Pasted image 20240915211139.png]]
## Naïve SIR phase plot
![[Pasted image 20240915211156.png]]
# SIR Model (without demography)
The transition from **S → I** is determined by:
1. The amount of infectious individuals
2. The underlying population contact structure
3. The probability of transmission given contact

The force of infection $\lambda$ is the per capita rate at which susceptible are getting infected, so the rate at which infected are produced is $\lambda X$

We assume that the force of infection is proportional to $Y$.
***Mass action:*** Assumes that the risk of infection depends on the proportion of infected individuals in the population.
- Independent of population size N.
- Makes sense intuitively
- Shown to be correct for e.g. measles
$\lambda = \beta Y / N$

***Density dependent:*** Assumes that the risk of infection depends on the absolute number of infected individuals, irrespective of population size.
- Depends on population size
$\lambda = \beta Y$

## Derivation of frequency dependent transmission term
Assumptions:
- Homogeneous mixing between S and I
- Susceptible have on average $\kappa$ contacts per unit time
- A fraction $I = Y/N$ are contacts with infectious individuals

Within a given time interval $(t, t+\delta t)$ the average number of contacts with infectious individuals is $\kappa (Y/N)\delta t$.
$c=$ probability of successful disease transmission upon contact

The probability that an individual doesn't get the disease after $\kappa (Y/N)\delta t$ contacts is:
$$
\begin{align*}
1 - \delta q &= (1-c)^{\kappa(Y/N)\delta t} \\
\delta q &= 1 - e^{-\beta(Y/N)\delta t} \\
\beta &= - \kappa \ln(1-c) 
\end{align*}
$$
After taking the Taylor expansion of the exponential, divide it by $\delta t$ and take the limit $\delta t \rightarrow 0$:
$$
\frac{dq}{dt} = \beta Y / N
$$
so
$$
\lambda = \beta X Y / N
$$
$$
\begin{align}
\frac{dX}{dt} &= -\beta X Y/ N \\
\frac{dY}{dt} &= \beta XY/N - \gamma Y \\
\frac{dZ}{dt} &= \gamma Y \\
\end{align}
$$
But for a constant population
$$
\begin{align}
\frac{dS}{dt} &= -\beta  S I \\
\frac{dI}{dt} &= \beta S I - \gamma I \\
\frac{dR}{dt} &= \gamma I \\
\end{align}
$$
## ***Threshold phenomena***
What determents if you we have an epidemic? 
$$
\begin{align*}
\frac{dI}{dt} &= I (\beta S - \gamma) \\
\frac{dI}{dt} &< 0 \;\;\text{infection descreasing} \\
\frac{dI}{dt} &> 0 \;\;\text{infection increasing} \\
\frac{dI}{dt} &= 0 \;\;\text{threshold}
\end{align*}
$$

$$
\begin{align*}
\frac{dI}{dt} &= 0 \;\; \text{when} \;\; I (\beta S - \gamma) = 0 \\
I &= 0 \\
S &= \frac{\gamma}{\beta}
\end{align*}
$$
We define $R_0$ as $R_0 = \frac{\beta}{\gamma}$
$$
\begin{align*}
\frac{dI}{dt} &< 0 \;\; S > \frac{1}{R_0} \\
\frac{dI}{dt} &> 0 \;\; S < \frac{1}{R_0} \\
\end{align*}
$$
If we assume at $t=0$ that we will have 1 infected person $I(0) = \frac{1}{N}$.
Then $S(0)=1-\frac{1}{N}$. Because $N >> 1$ within our case studies then $S(0)=1$, so $1 > \frac{1}{R_0}$ we will have a spread or if $R_0 > 1$.

## ***Epidemic Burnout***
$$
\begin{align}
\frac{dS}{dt} &= -\beta  S I  \qquad\qquad\qquad -\beta S^* I^* = 0 \\
\frac{dI}{dt} &= \beta S I - \gamma I  \;\qquad\qquad \beta S^* I^*-\gamma I^* = 0 \\
\frac{dR}{dt} &= \gamma I \;\;\;\;\qquad\qquad\qquad \gamma I^* = 0 \\
\end{align}
$$

Fixed points:
$(S=S^*, I=0, R=1-S^*)$ endemic state

What happened if $t \rightarrow \infty$
Change S as function of R
$$
\begin{align*}
\frac{dS}{dR} = \frac{dS/dt}{dR/dt} &= -\frac{\beta S}{\gamma} = -R_0S \\
S(t) &= S(0)e^{-R_0 \cdot R(t)} \\
e^{-R_0 \cdot R(t)} &> 0 \;\;\text{so}\;\; S(t) > 0 \\
0 \leq R(t) \leq 1 &\rightarrow S(0)e^{-R_0} \leq S(t) \leq S(0)
\end{align*}
$$
Based on this model:
- Always some susceptible left
- The chain of transmission breaks due to the decline in infected people not due to a lack of susceptible people.

$$
\begin{align*}
S(\infty) = 1 - R(\infty) &= S(0)e^{-R_0 \cdot R(t)} \\
I(\infty) &= 0 \\
1 - R(\infty) - S(\infty) &= 0 \\
1 - R(\infty) - S(0)e^{-R_0 \cdot R(\infty)} &= 0 \\
\end{align*}
$$

## Epidemic Curve
An approximating of the number of new cases per time interval.
Explore:
$$
\begin{align*}
(1)\;\;\frac{dR}{dt} = \gamma I = \gamma (1-S-R) &= \gamma \left( 1-S(0)e^{-R_0 \cdot R(t)} \right)\\
\end{align*}
$$
Assume $R_0 \cdot R$ is small:
$$
\begin{align*}
e^{-x} &= 1-\frac{x^1}{1!} + \frac{x^2}{2!} \\
e^{-R_0 \cdot R} &= 1 - R_0 \cdot R + \frac{1}{2}(R_0 \cdot R)^2
\end{align*}
$$
Substitute in eq (1)
$$
\begin{align*}
\frac{dR}{dt} &= \gamma \left[ 1-S(0) \left(1- R_0 \cdot R + \frac{1}{2}(R_0 \cdot R)^2  \right) \right] \\
&= \gamma \left[ 1-S(0) + \left(S(0)R_0 -1\right)R+\frac{1}{2}S(0)R_0^2R^2 \right] \\
R(t) &= \frac{1}{R^2_0} \left[S(0)R_0 - 1 + \alpha \tanh \left(\frac{1}{2} \alpha \gamma t - \phi \right) \right] \\\\
\alpha &= \left[ (S(0)R_0-1)^2 + 2 S(0)I(0)R^2_0 \right]^{\frac{1}{2}} \\
\phi &= \tan^{-1} \left[\frac{1}{\alpha}(S(0)R_0-1) \right] \\\\
\frac{dR}{dt} &= \frac{\gamma \alpha^2}{2R^2_0S(0)} \text{sech}^2 \left(\frac{1}{2}\alpha\gamma t - \phi \right)
\end{align*}
$$
![[Pasted image 20241020211752.png]]
# SIR Model (with demography) #TODO 
Within the previous models the assumption was made that the model was on a short time scale so no birth and deaths were taking into account. Within this models without demography no endemic state will happen, because all the pathogens will go extinct.
When adding birth and death (***demographic processes***) fresh susceptible will be added to the system.

Assuming a life span of $1 / \mu$ years, the rate of which each individual will suffer from natural mortality $\mu$ per year.
Assuming that $\mu$ is the birth rate, so the population size stays constant.
$\mu(S+I+R) = \mu$ into the S compartment.

$$
\begin{align*}
\frac{dS}{dt} &= \mu - \beta S I - \mu S \\
\frac{dI}{dt} &= \beta S I - \gamma I - \mu I \\
\frac{dR}{dt} &= \gamma I - \mu R \\
\\
R_0 &= \frac{\beta}{\gamma + \mu} 
\end{align*}
$$
## ***Fixed Points***
$$
\begin{align}
(2.1)\;\; \frac{dS^*}{dt} &= \mu - \beta S^* I^* - \mu S^* \\
(2.2)\;\; \frac{dI^*}{dt} &= \beta S^* I^* - \gamma I^* - \mu I^* \\
(2.3)\; \frac{dR^*}{dt} &= \gamma I^* - \mu R^* \\
\end{align}
$$
A fixed points happens when $I^*$ and $R^*=0$, giving the point $(1,0,0)$. This is the disease free state based on eq (2.3).
Now look at:
$$
\begin{align}
\frac{dI^*}{dt} &= \beta S^* I^* - \gamma I^* - \mu I^* = 0 \\
\frac{dI^*}{dt} &= I^* (\beta S^* - \gamma - \mu) = 0 \\
\\
\text{then if:}\\
S^* &= \frac{\gamma + \mu}{\beta} \\
S^* &= \frac{1}{R_0}\\
\\
\text{we have:}\\
\frac{dI^*}{dt}&=0 \\
\\
\text{subsitute in eq (2.1):} \\
\frac{dS^*}{dt} &= \mu - \frac{\beta I^*}{R_0} - \frac{\mu}{R_0} = 0 \\
\\
I^* &= \frac{\mu - \frac{\mu}{R_0}}{\frac{\beta}{R_0}} \\
&= \frac{\mu}{\beta}(R_0-1)
\\
\text{then:} \\
R^* &= 1 - S^* - I^I \\
&= 1 - \frac{1}{R_0} - \frac{\mu}{\beta}(R_0 - 1) \\
&= \frac{\gamma}{\beta}(R_0 - 1)
\\
\text{So our endemic fixed point:} \\
\left(\frac{1}{R_0}, \frac{\mu}{\beta}(R_0 - 1), \frac{\gamma}{\beta}(R_0 -1) \right)
\end{align}
$$



## Equilibrium
$$
\begin{align}
S^* &= \frac{\gamma + \mu}{\beta} \\
I^* &= \frac{\mu (1 - S^*)}{\gamma + \mu}
\end{align}
$$

## Stability Analysis
We can look at values near or around fixed points to see how the system behaves.
Given $S$ and $I$ we know $R$ so we get 2 variables.
![[Pasted image 20241021120529.png]]

Given a system:
$$
\frac{dx}{dt} = ax + by \;\; \frac{dy}{dt} = cx + dy
$$
We can calculate what happens around a fixed point based on the ***eigen vector & eigen values***.
$$
\begin{align*}
\overline X(t) &= \overline V e^{\lambda t} \\
\frac{d \overline X}{dt} &= \overline V \lambda e^{\lambda t} \\
\\
\text{Subsitute the result in the original formula:} \\
\frac{d \overline X}{dt} = A \overline X &\rightarrow \lambda e^{\lambda t} \overline V = e^{\lambda t} A \overline V\\
&\rightarrow \lambda \overline V = A \overline V\\
\\
\text{det}(A-\lambda I) = 0 \\
\text{det}
\begin{bmatrix}
a - \lambda & b \\
c & d - \lambda
\end{bmatrix} \\
(a - \lambda)(d - \lambda) - bc &= ad - \lambda d + \lambda^2 - bc \\
&= \lambda^2 - \lambda(a + b) + ad -bc \\
\\
\text{where:}\\
a + d &= \tau \;\; \text{trace of matrix} \\
ad + bc &= \Delta \;\; \text{det of matrix} \\
\\

\lambda_1 =\frac{\tau + \sqrt{\tau^2 - 4 \Delta}}{2} \\
\lambda_2 =\frac{\tau - \sqrt{\tau^2 - 4 \Delta}}{2}
\end{align*}
$$
$$
\begin{align*}
\tau &= \lambda_1 + \lambda_2 \\
\Delta &= \lambda_1 \lambda_2
\end{align*}
$$

|         | Stable? | $\Delta$     |                            | $\tau$     | Set          |                     |
| ------- | ------- | ------------ | -------------------------- | ---------- | ------------ | ------------------- |
| Saddle  |         |              |                            | $\tau < 0$ |              |                     |
| Centres |         | $\Delta = 0$ |                            | $\tau > 0$ |              |                     |
| Node    | True    | $\Delta < 0$ | $\lambda_1, \lambda_2 < 0$ | $\tau > 0$ | $\mathbb{R}$ | $\tau^2 > 4 \Delta$ |
| Node    | False   | $\Delta > 0$ | $\lambda_1, \lambda_2 > 0$ | $\tau > 0$ | $\mathbb{R}$ | $\tau^2 > 4 \Delta$ |
| Spiral  | True    | $\Delta < 0$ | $\lambda_1, \lambda_2 < 0$ | $\tau > 0$ | $\mathbb{C}$ | $\tau^2 < 4 \Delta$ |
| Spiral  | False   | $\Delta > 0$ | $\lambda_1, \lambda_2 > 0$ | $\tau > 0$ | $\mathbb{C}$ | $\tau^2 < 4 \Delta$ |



### Example using [[Lokta-Voltera]]
$$
\frac{dX}{dt} = 3x - x^2 - 2xy \;\; \frac{dY}{dt} = 2y - y^2 - xy
$$
Their are 4 possible fixed points within this system:
$$
(0,0), (x=0, y\neq0), (x\neq0, y=0), (x\neq0, y\neq0)
$$
Given the formula this results in:
$$
\begin{align}
x=0, y\neq0 \\
2y-y^2&=0 \\
y&=2 \\
(0,2)\\
\\
x\neq0, y=0 \\
3x-x^2&=0 \\
x&=3 \\
(3,0) \\
\\
x\neq0, y\neq0 \\
\text{divide by}\; x \;\text{on}\frac{dx}{dt} &\rightarrow 3 = x + 2y\\
\text{divide by}\; y \;\text{on}\frac{dy}{dt} &\rightarrow 2 = y + x \\
(1,1)
\end{align}
$$
To classify our fixed points we change the formula to:
$$
\begin{align}
f(x, y) = \frac{dx}{dt} &= x(3-x-2y) = 3x-x^2-2xy \\
g(x, y) = \frac{dy}{dt} &= y(2-y-x) = 2y-y^2-xy 
\end{align}
$$
Calculate the jacobian:
$$
J = 
\begin{bmatrix}
\frac{\delta f}{\delta x} & \frac{\delta f}{\delta y} \\
\frac{\delta g}{\delta x} & \frac{\delta g}{\delta y}
\end{bmatrix}
= 
\begin{bmatrix}
3-2x-2y & -2x \\
-y & 2-x-2y
\end{bmatrix}
$$
#### Fixed point $0,0$
$$
\begin{align}
J &= 
\begin{bmatrix}
3 & 0 \\
0 & 2
\end{bmatrix}
\\
\lambda_1 &=3, \lambda_2 = 2\\
\\
\Delta &= ad -bc \\
& = 3 \cdot2-0\cdot0\\
&= 6\\
\\
\tau &= a + d \\
&= 3 +2 \\
&= 5 \\
\\
\tau^2 - 4\Delta &= 0 \\
\tau^2 &= 4\Delta \\
5^2 &> 4 \cdot6 \\
25 &> 24 \\
\text{so}\;\; \tau^2 &> 4\Delta
\end{align}
$$
So this point is an ***Unstable Node***

#### Fixed point $0,2$
$$
\begin{align}
J &= 
\begin{bmatrix}
-1 & 0 \\
-2 & -2
\end{bmatrix}
\\
\lambda_1 &=-1, \lambda_2 = -2\\
\\
\Delta &= ad -bc \\
& = -1 \cdot-2+2\cdot0\\
&= 2\\
\\
\tau &= a + d \\
&= -1 + -2 \\
&= -3 \\
\\
\tau^2 - 4\Delta &= 0 \\
\tau^2 &= 4\Delta \\
(-3)^2 &> 4 \cdot2 \\
9 &> 8 \\
\text{so}\;\; \tau^2 &> 4\Delta
\end{align}
$$
Both eigenvalues negative so a ***Stable Note***

#### Fixed point $3,0$
$$
\begin{align}
J &= 
\begin{bmatrix}
-3 & -6 \\
0 & -1
\end{bmatrix}
\\
\lambda_1 &=-3, \lambda_2 = -1\\
\end{align}
$$
Both eigenvalues negative so a ***Stable Note***

#### Fixed points $(1,1)$
$$
\begin{align}
\Delta 
\begin{bmatrix}
-1 & -2 \\
-1 & -1
\end{bmatrix}
 &= 1 - 2 = -1 \\
\end{align}
$$
So $\Delta < 0$ gives us a ***Saddle Point*** 
![[Pasted image 20241021130646.png]]

## Oscillatory Dynamics
Because we have a $\mu$ that is much smaller than the $\gamma$ because people live on average 75 years and recover in a month.
So we can assume that $\left( \mu R_0^2 \right) << 4\mu (\gamma + \mu)(R_0 -1)$
We can state we have damped oscillations to endemic state.
![[Pasted image 20241021131056.png]]

When $t \rightarrow \infty$ the oscillations will dampen and an equilibrium will be reached. 
With a [Fourier analysis](Fourier%20Analysis) the singles can be analysed. Whereby the amplitude of the oscillation will be plot against the frequency (Hz).
# Numerical solvers
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
\frac{dy}{dt} &= \frac{y_{n+1}-y_n}{\delta t} = f(t_n, y_n) \\
y_{n+1} &= y_n + \delta t \cdot f(t_n, y_n)
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
y_{n+1} &= y_n - \delta t C \cdot y_n \\ 
y_{n+1} &= y_n - (I-\delta t C)y_n \\
y_{n} &= (I-\delta t C)^n y_0
\end{align}
$$
A matrix $A^n$ tends to zero for $n \rightarrow \infty$ only if the largest eigenvalue of $A$ has a magnitude smaller than 1.
So, $y_n$ is bounded as $n \rightarrow \infty$ only if the largest eigenvalue of $I - \delta t C$ is less then 1.
With $\lambda_{\text{max}}$ the large eigenvalue of $C$ this results in:
$$ \delta t < \frac{2}{\lambda_{\text{max}}} $$
So the timesteps must be small enough
### [[Runge-Kutta]]
Let an initial value problem be specified as follows:
$$
\frac{dy}{dy} = f(t, y), \;\; y(t_0) = y_0
$$
Nw we pick a step-size $\delta t >0$ and define given a fourth order Runge-Kutta (so, error $O(\delta t^2)$):
$$
\begin{align}
k_1 &= \delta t f(t_n, y_n) \\
k_2 &= \delta t f\left(t_n + \frac{1}{2} \delta t, y_n + \frac{1}{2}k_1\right) \\
k_3 &= \delta t f\left(t_n + \frac{1}{2} \delta t, y_n + \frac{1}{2}k_2\right) \\
k_4 &= \delta t f\left(t_n + \delta t, y_n + k_3 \right) \\
\\
\overline y_{n+1} &= \overline y_n + \frac{k_1}{6} + \frac{k_2}{3} + \frac{k_3}{3} + \frac{k_4}{6} + O(\delta t^5) \\
t_{n+1} &= t_n + \delta t \\
\end{align}
$$
![[Pasted image 20241021132906.png]]

More orders will give a more precise answer, but will take more computational power.
# Variants
You can adapt a model framework to different kind of diseases and other problems.
No need to restrict to 1 model (SEIR for example) if the biology suggest otherwise.
![[Pasted image 202410041046555.png]]
![[Pasted image 202410041047095.png]]
![[Pasted image 202410041047235.png]]
![[Pasted image 202410041047355.png]]

| Model  | Usecase                              |
| ------ | ------------------------------------ |
| **SEI**    | Diseases where exposed individuals can infect others before becoming fully infectious (e.g., some vector-borne diseases). |
| **SIRS**   | Loss of immunity (e.g., common cold, where individuals can lose immunity over time and become susceptible again). |
| **SIS**    | Recurrent conditions without permanent immunity, such as alcohol or drug addiction, where recovery is not permanent, and individuals can relapse. |
| **SEIR**   | Diseases with an incubation period where individuals are exposed but not yet infectious (e.g., measles or influenza). |
| **SICR**   | Diseases with carriers, where individuals can carry the disease without showing symptoms but still spread it (e.g., typhoid). |
| **SIRX**   | Diseases with an environmental reservoir, meaning pathogens persist outside of human hosts (e.g., cholera, anthrax). |
| **SIR-SI** | Vector-borne diseases, where there are two types of hosts, one being infected (SIR) and the other being the vector, like mosquitoes (SI) (e.g., malaria, dengue fever). |

# Residence Times
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

