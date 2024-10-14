# Complex system
Hard to reason about [complex systems](Complex%20Systems) without using computing as a way to model.
When modelling a complex systems you look at the interactive parts within the system.
You can't just study a traffic jam by observing a single driver.

Through interactions of individuals new structures arise.
These systems have hard to predict phenomena based on small changes
Cause -> effect is hard to understand.

# [[Agent Based Modelling]]
Simple systems to measure segregation user in [Schelling's Model of Segregation](Different%20Models)

# Infectious Diseases
## Definitions
| Verb                                     | Definition                                                                                            |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Disease                                  | A condition of the body, in which its functions are disturbed or deranged.                            |
| Infectious Disease                       | An illness due to a specific infectious agent or its toxic products that arises through transmission. |
| Pathogen                                 | An Infectious agent.                                                                                  |
| Communicable                             | Something that is transmittable from agent to agent.                                                  |
| Infection                                | The entry and development or multiplication of an infectious agent in the body of man or animals.     |
| Vector                                   | A living carrier that transports an infectious agent.                                                 |
| [Endemic](Epidemiology)                  | The usual or expected frequency of disease.                                                           |
| [Epidemic](Epidemiology)                 | The excess of expected occurrences.                                                                   |
| [Pandemic](Epidemiology)                 | The collection of epidemic's over a wide geographic area.                                             |
| Susceptible                              | Uninfected, but able to become infected.                                                              |
| Infectious                               | Infected and able to transmit to other susceptible agents.                                            |
| Immune                                   | Protection against an infection.                                                                      |
| Diseased/clinical infection              | Symptomatic Infected.                                                                                 |
| Latent infection / subclinical infection | Asymptomatic Infected.                                                                                |
![[Pasted image 20240915165605.png]]
![[Pasted image 20241009093648.png]]
Endemic:
- Transmission occur, but the number of cases remains constant
Epidemic:
- The number of cases increases
Pandemic:
- When epidemics occur at several continents
## Classification of Infectious Disease & Models of Transmission

| Pathogens | Example                     | Explanation                                                       |
| --------- | --------------------------- | ----------------------------------------------------------------- |
| Micro     | Bacteria / viruses / prions | Fast developments, only the host's infection  status is important |
| Macro     | Parasitic worms             | Complex life cycle                                                |

| Transmissons |     | Contact             | Example  |
| ------------ | --- | ------------------- | -------- |
| Direct       |     | Via close contact   | COVID-19 |
|              |     | Via a vector        |          |
| Indirect     |     | Via the environment | Mallaria |
|              |     | Via air             |          |
A vector is technically indirect , but within modelling we assume that it's a direct form

![[Pasted image 20241009093339.png]]

## Primary / Secondary cases
The person who comes into and infects a population is the primary case.
Those who subsequently contract the infection are secondary cases. Further spread is described as "waves" or "generations.

![[Pasted image 20241009100451.png]]
![[Pasted image 20240915170116.png]]
## Reproductive rate
A disease spreads in "waves", $\text{P}_0$ infects $\text{R}_0$ many people whereby $\text{R}_0$ is the basic reproductive number.
If $\text{R}_0 > 1$ the disease spreads otherwise the disease will die out.

$$
\text{basic} \;R_0 = \frac{\beta}{\gamma}
$$
## [[Spreading Phenomenon]]
The phenomenon where information spreads through a [system](System)/[network](Network) and where the spreading leads to local information processing.
Spreading of information in the broad sense is a very common phenomenon

Not only diseases can spread but also rumours, tweets or fires.