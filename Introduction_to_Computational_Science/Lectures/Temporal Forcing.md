The SIR model doesn't explain the fluctuations within the data we see. 
Some years/ part of years have more or less infected people.
![[Pasted image 20241009112636.png]]

As see the measles has peaks in 1935, 1940 but not in between them.
# Seasonal effects
Soper's assumptions:
- Short infectious period $\tau = 1/\gamma$
- Large $R_0$ (childhood disease)
- Infectious go to compartment $R$ by next period $\tau$
- No deaths during infection
- Large number of $X$ (susceptible)

If we now take a discrete time approach, so we start with $X_t, Y_t, Z_t$ and then find expressions for $X_{t+\tau}, Y_{t+\tau}, Z_{t+\tau}$.

Based on the assumptions the following formula can be constructed:
$$
\begin{align}
X_{t+\tau} = X_t + \mu - \beta_t Y_t X_t &= \mu + X_t(1 - \beta Y_t) \\
Y_{t+\tau} &= \beta X_t Y_t \\
Z_{t+\tau} &= Z_t + Y_t - \mu
\end{align}
$$

Because the susceptible population will hardly change over a time step $X_{t+\tau} \approx X_t$.
Based on $\frac{Y_{t + \tau}}{Y_t} = \beta X_t$ we can approximate this as $B X_{t+\tau}$

If we want a shorter infectious period $\tau$ by a factor $\alpha$, we will have $1/\alpha$ periods in $\tau$.
We will than need to take $1/\alpha$ time steps of duration $\alpha \tau$.
This results in:
$$
\begin{align}
\frac{Y_{t + \tau}}{Y_t} &= (\beta X_{t+\tau})^\alpha \\
(\frac{Y_{t + \tau}}{Y_t})^\alpha &= \beta X_{t+\tau}
\end{align}
$$

## Soper's research
The monthly cases in month $t$ are equal to $Y_t$.
Whereby the infectious period of measles is approximately two weeks, so $\alpha = 1/2$.

At the peak of the epidemic, $\frac{dY}{dt} = 0, Y_{t+\tau} ~ Y_t$ given a first guess for $\beta$.
Based on the data, $\beta$ wasn't constant.
![[Pasted image 20241009114901.png]]

## Conclusion:
"A range of statistical approaches have revealed that transmission of childhood infections
varies seasonally, peaking at the start of the school year and declining significantly in the summer months."
# Temporal Forcing & Harmonic Resonance
## Modelling forcing
Given:
$$
\begin{align}
\frac{dX}{dt} &= \mu N - \beta(t)XY/N \\
\frac{dY}{dt} &= \beta(t)XY/N - \gamma Y
\end{align}
$$
We ignore death of susceptibles and infecteds, so everybody constracts the infection during their lifetime, a good approximation for measles.
$$
\beta(t) = \beta_0(1 + \beta_1 \cos(\omega t))
$$

![[Pasted image 20241021141751.png]]
$\beta_0$ is the average transmission rate, $\beta_2$ how much the rate varies for the season, $\omega$ the period of the forcing, $0 < \beta_1 < 1$ is the amplitude of seasonality. $R_0 = \beta_0 / \gamma$, representing a yearly average. Note that at certain periods (say when $\cos(\omega t) \sim 1)$ the instantaneous growth rate can be much faster.

To approximate the dynamics we define $X^*$ and $Y^*$ as the equilibrium points for the unforced model, so with $\beta_1 = 0$.
Then with $\beta_1 << 1$ and $X = X^*(1+x)$ and $Y = Y^*(1+y)$, substituting in the model, and omitting higher order terms we find:
$$
\frac{d^2y}{dt^2} + \mu R_0 \frac{dy}{dt} + \mu \beta_0 y = -\beta_1 \omega \gamma \sin{(\omega t)}
$$
This gives a solution with the same period of scillations $\omega$ and the amplitude of the oscillations $M$ is:
$$
M = \beta_1 \omega \gamma 
\left[ 
\left( \mu \beta_0 - \omega^2
\right)^2 + 
\left(
\omega \mu R_0
\right)^2
\right]^{-\frac{1}{2}}
$$
## Types of Resonance
### Harmonic Resonance
If the forcing frequency is close to the natural frequency F of the unforced SIR dynamics, the amplitude of the oscillations peaks. We will get an ***Harmonic Resonance***.

In other words when the forcing of the seasons matched the forcing of the natural birth and death cycle, the forcing will be amplified.
![[Pasted image 20241021143827.png]]

### Subharmonic Resonance
For different ratios $\omega:F$ it is possible for forcing to excite sub harmonic resonance leading to oscillations with a longer period than the period forcing.
![[Pasted image 20241021143846.png]]

In the absence of seasonal forcing, the SIR family of models exhibit a **stable equilibrium**. The introduction of time dependent transmission rates can generate a variety of dynamical patterns depending on parameter values- ranging from simple annual epidemics to multiannual outbreaks and eventually chaos.

# Bifurcation Diagrams
How to visualize the changing dynamics as a function of a control parameter?
A **bifurcation diagram** shows the values visited or approached asymptotically (fixed points) pf a system as a function of a bifurcation parameter in the system.

A logistic map is an example of a bifurication diagram:
$$
x_{n+1} = rx_n(1-x_n)
$$
For the SIR model a bifurication diagram is created using the following parameters:
$X_n$: the ratio of existing population to the maximum population ($X=1$ is full capacity)
$r$: growth rate due to birth.

![[Pasted image 20241021145929.png]]

When $r$ increases past 3, the system undergoes a **period-doubling bifurcation**. Instead of stabilizing at one value, the population starts oscillating between two values, then four, then eight, and so on.
For $r$ values beyond about 3.57, the system enters a **chaotic regime** where the population values become unpredictable and do not settle into any periodic behavior. This is shown by the dense and seemingly random distribution of points, indicating chaotic fluctuations in population from generation to generation.

As the growth rate increases, the population begins to oscillate, doubling its period with each step.
# Vaccination and Dynamics
## Dynamic variability
Explanations for the dynamics variability can be:
- population birth rates that change over time
- Vaccination
![[Pasted image 20241021151108.png]]
## Vaccinations
Vaccination is the same as changing $\mu$ or $\nu$ (if birth rate is different than death rate) because these people to straight to the R pool.
High birth rate can lead to annual patterns.

Assume paediatric vaccination, where a fraction $p$ of newborns are vaccinated and therefore protected from infection.
$$
\begin{align}
\frac{dS}{dt} &= \nu(1-p) - \beta S I - \mu S \\
\frac{dI}{dt} &= \beta S I - \gamma I - \mu I \\
\frac{dR}{dt} &= \gamma I + \mu p - \mu R 
\end{align}
$$
To dynamically explore the modifications, we use a simple linear change of variables.
$$
\begin{align}
S = S'(1-p),\; I&=I'(1-p), \; R=R'(1-p)+p \\
\\
\frac{(1-p)dS'}{dt} &= \nu(1-p) - (\beta I'(1-p)+ \mu) S'(1-p) \\
\frac{(1-p)dI'}{dt} &= \beta S' I'(1-p)^2 - (\gamma - \mu) I'(1-p) \\
\frac{(1-p)dR'}{dt} &= \gamma I'(1-p) + \mu p - \mu R'(1-p) - \mu p 
\end{align}
$$When cancelling out the terms $(1-p$) on both sides we get:
$$
\begin{align}
\frac{dS'}{dt} &= \nu - (\beta I'(1-p)+ \mu) S' \\
\frac{dI'}{dt} &= \beta S' I'(1-p) - (\gamma - \mu) I'\\
\frac{dR'}{dt} &= \gamma I' - \mu R' 
\end{align}
$$
The transmission rate $\beta$ is replaced with $\beta(1-p)$. 
A system either subject to constant long-term vaccination of a fraction $p$ of newborns against an infection with a base reproductive rate $R_0$, or with a modified per capita birth rate of $\nu'$, is dynamically identical to a system with $R_0'=(1-0)\frac{v'}{v}R_0$.

When a critical level cannot be achieved (i.e. the disease will die out). Vaccination can still reduce the prevalence infection:
$$
I*=\frac{v(1-p)}{\gamma+\mu} - \frac{\mu}{\beta}
$$![[Pasted image 20241021153136.png]]