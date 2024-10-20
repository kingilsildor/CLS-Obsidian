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

Because the susceptible population will hardly change over a time step.
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

# Temporal Forcing & Harmonic Resonance
# Bifurcation Diagrams
# Vaccination