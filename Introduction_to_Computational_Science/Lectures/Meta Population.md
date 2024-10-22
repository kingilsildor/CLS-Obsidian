- Ways to introduce spatial models (Riley – Science)  
- Important concepts  
- Meta Population Models (force of infection)  
- Deterministic and Stochastic subpops combined – consequences.  
- Commuters  
- Coarse Graining (overview)  
- Gleam example

# Spatial Models
Because transmission is a local process, it can happen that different area's will have a different spreading rate. This is partly because population is not evenly spread. Cities will have more people in it than the countryside and people move around from area to area.

Spatial models give the opportunity to capture spatial components of a system.
They can:
- determine the rate of spatial spread
- Understand influence of heterogeneity of population densities
- Understand how to use spatial models to optimize control measures

Spatial models can be made in multiple abstractions:
![[Pasted image 20241016143735.png]]
**Patch:**
- All members of the same patch (town, city) will have the same force of infection
**Distance:**
- 

# Important Concepts
***Heterogeneity:*** Non uniformity within a given population.
This can arise via spatial differences.
- Spatial differences can already be in the system, whereby different social structures lead to different transmission rates
- Emerge within a system whereby different movements and patterns lead to these different transmission rates.

***Interaction:*** is key concept because it shows how individuals move through a system. This allows for the spreading of a disease. 
Interaction can be modelled by adding additional force of infection due to infectious individuals in other locations.
- Interaction between two populations should decrease with distance $d$ with them.
***Transmission kernel*** $K$ can capture this distance
- Exponential $K \propto \exp(-Ad)$
- Gaussian $K \propto \exp(-Ad^2)$
- Power law $K \propto d^{-A}$

***Isolation:*** A population protected from infections
***Local  Extinction:*** Due to smaller subpopulation sizes in spatial models, local extinctions are common. Whereby interaction with larger subpopulations where the disease is endemic will then lead to new invasions of the pathogen.

***Scale:***
- Scale of interactions
- Scale of simulation

Increasing the scale can give use more details, but can also take more computational power.
You need to find a balance so that it's large enough to observe all the interesting dynamics.
Same for the population, you can simulate this down to the individual or a wider area.

# Metapopulations
- Subdivide the population in distinct subpopulations.
- Each subpopulation has independent dynamics.
- Each subpopulation has a limited form of interaction.

When adding metapopulations in the SIR framework we assume that we have $n$ subpopulations, and $(X_i, Y_i, Z_i)$ individuals in each pool for subpopulation $i$.
The total population in subpopulation $i$ is $N_i$ whereby the total population is $N = \sum^n_{i=1} N_i$.
$$
\begin{align*}
\frac{dX_i}{dt} &= \nu_i N_i-\lambda_i X_i-\mu_iX_i \\
\frac{dY_i}{dt} &= \lambda_i X_i - \gamma_i Y_i - \mu_i Y_i 
\end{align*}
$$

***Force of infection*** of a given subpopulation $i$ is related to the amount of infectious individuals in population $j$. In general cases:
$$
\lambda_i = \beta_i \sum^n_{j=1} \rho_{ij} \frac{Y_j}{N_i}
$$
Whereby $\rho_{ij}$ is the relative strength of transmission to population $i$ from population $j$.
Because we assume that transmssion takes place in population $i$ we scale the infectious individuals from $j$ to the same scale as $i$.
![[Pasted image 20241022100753.png]]

# Deterministic vs Stochastic subpopulations
We consider two subpopulations, $n=2$.
- Both of equal size
- Ignore demograph (we assume homogenity)
- Fully susceptible at start
- Introduce infection in subpopulation 1
- We assume there is only intrecation from 1 to 2, so: $p_{ii} =1, p_{12}=0,$ and $p_{21}$ is small.

## Deterministic
We model subpopulation 1 with a deterministic model and subpopulation 2 with either models.
$$
\frac{dY_2}{dt} = \beta_2\rho_{21}\frac{Y_1}{N'}X_2+\beta_2\frac{Y_2}{N'}X_2 - \gamma_2 Y_2
$$
If we divide the equation by $N'$ and consider early dynamics where $S_2 = \frac{X_2}{N'} \approx 1$ leading to
$$
\frac{dI_2}{dt} = \beta_2\rho_{21}I_1 + \beta_2I_2-\gamma_2I_2
$$
Which can be solved as
$$
I_2(t) = \int^t_0 \beta_2\rho_{21}I_1(s) \exp([\beta_2 - \gamma_2]s)ds
$$
We observe
1. The disease is present in population 2 immediate from the start of the epidemic in population 1
2. Early very small infections that arrive in population 2 trigger an exponential growth at rate $\beta_2 - \gamma_2$.

## Stochastic
The interaction between population 2 and 1 will trigger events in 2 that lead to infectious in population 2. Because the initial rates are small the probability that the disease goes extinct is $\frac{1}{R_0}$. We can estimate the probability that an epidemic is triggered in population 2:
$$
\begin{align}
\text{P(epidemic)} &= \sum^{N'}_{n=1}\;\text{P(case in 1 cause}\;n\;\text{cases in 2)} \times \text{P(}\;n\;\text{initial cases lead to epidemic)} \\

&= \sum^{N'}_{n=1} \exp \left( -\beta_2\rho_{21} \int^{\infty}_0 I_2(s)ds \right) \frac{\left(\beta_2\rho_{21}\int^\infty_0I_1(s)ds \right)^n}{n!} \times \left[1 - \left(\frac{\gamma}{\beta_2} \right)^n \right] \\
&= \exp \left( -\beta_2\rho_{21} \int^{\infty}_0 I_2(s)ds \right)
\left[ \left(
\exp \left( \beta_2\rho_{21} \int^{\infty}_0 I_2(s)ds \right)
\right) - \exp \left( \gamma\rho_{21} \int^{\infty}_0 I_2(s)ds \right)
\right] \\
&= 1- \exp \left( -\beta_2\rho_{21} \left[
1 - \frac{\gamma}{\beta_2}
\right]
\int^{\infty}_0 I_2(s)ds \right)
< 1 - \exp \left( 
-\beta_2\rho_{21}\left[ 
1 - \frac{\gamma}{\beta_2}
\right]
\right) \\
\text{P(epidemic)} &<  1 - \exp \left( 
-\beta_2\rho_{21}\left[ 
1 - \frac{\gamma}{\beta_2}
\right]
\right)
\end{align}
$$

If the interaction $\rho_{21}$ is small enough, there is a high probability that the disease will not spread in population 2. When $\rho_{21}$ is larger the pathogen can spread but their can be a significant delay for spreading.

In stochastic metapopulation models the spread of infectious disease is slower!

# Commuters
Commuters live in one population but travel occasionally to another population.
Within the Netherlands this is a normal occurrence.
We will use $j$ as the living population and $i$ as the population the individuals move to.
$X_{ij}, Y_{ij}$ and $N_{ij}$ are the number of susceptibles, infecteds and total currently in population $i$ that live in population $j$.

$l_{ij}$: rate at leave their home and commute
$r_{ij}$: return rate
Both these values van be found based on real life data.

$$
\begin{align}
\frac{dX_{ii}}{dt} &= v_{ii} N_{ii} - \beta_i X_{ii} \frac{\sum_j Y_{ij}}{\sum_j N_{ij}} - \sum_j l_{ji} X_{ii} + \sum_j r_{ji} X_{ji} - \mu_{ii} X_{ii} \\[8pt]
\frac{dX_{ij}}{dt} &= v_{ij} N_{ij} - \beta_i X_{ij} \frac{\sum_j Y_{ij}}{\sum_j N_{ij}} + l_{ij} X_{jj} - r_{ij} X_{ij} - \mu_{ij} X_{ij} \\[8pt]
\frac{dY_{ii}}{dt} &= \beta_i X_{ii} \frac{\sum_j Y_{ij}}{\sum_j N_{ij}} - \gamma Y_{ii} - \sum_j l_{ji} Y_{ii} + \sum_j r_{ji} Y_{ii} - \mu_{ii} Y_{ii} \\[8pt]
\frac{dY_{ij}}{dt} &= \beta_i X_{ij} \frac{\sum_j Y_{ij}}{\sum_j N_{ij}} - \gamma Y_{ij} + l_{ij} Y_{jj} - r_{ij} Y_{ij} - \mu_{ij} Y_{ij} \\[8pt]
\frac{dN_{ii}}{dt} &= v_{ii} N_{ii} - \sum_j l_{ji} N_{ii} + \sum_j r_{ji} N_{ii} - \mu_{ii} N_{ii} \\[8pt]
\frac{dN_{ij}}{dt} &= v_{ij} N_{ij} + l_{ij} N_{jj} - r_{ij} N_{ij} - \mu_{ij} N_{ij}
\end{align}
$$

We use frequency transmission with $\sum_j N{ij}$ the total in population $i$

For the coarse graining the commuter model 5 assumptions are defined:
1. all parameters do not depend on $i, j$
2. $\nu = \mu$
3. All metapopulations of equal size $N_i = N\;\forall\;i$
4. Dynamics of commuting is fast, and it reaches an equilibirum fast with $\alpha = \frac{l}{r}$
	- $N_{ii} = \frac{1}{1+\alpha(n-1)}N$
	- $N_{ij} = \frac{1}{1+\alpha(n-1)}N$
5. Dynamics of commuting is much faster than the dynamics of disease and demography
	- $X_{ij} = \frac{1}{1+\alpha(n-1)}X_i$
	- $Y_{ij} = \frac{1}{1+\alpha(n-1)}Y_i$

![[Pasted image 20241022110047.png]]
