A network is a [complex systems](Complex%20Systems).
It has nodes and links/edges

# Basic measures of Networks
***Undirected:*** each edge goes both ways.
***Directed:*** an edge only goes one way.
## Degree
***Node degree:*** The number of links connected to the node
A node can have an ***in-degree*** an ***out-degree*** in a directed network.
The ***total degree*** is the sum of the in- and out-degree.

The ***average degree*** is calculated the following way:
**Undirected:**
$$\langle k \rangle \equiv \frac{2L}{N}$$
**Directed:**
$$\langle k \rangle \equiv \frac{L}{N}$$N - number of nodes
L - number of edges

***Degree Distribution*** is the probability distribution of the degrees over the whole network. Whereby the P(k) is the probability that a node with degree $k$ is chosen.
$$
P(k)=\frac{N_k}{N}
$$
$N_k$ - number of nodes with degree $k$

## Path length
***Distance:*** shortest path between two nodes. The distance needs to follow the direction of the arrows within a directed graph.
For calculating the distance you need to know what the maximum amount of edges can be if the network is fully connected.
$$ L_{\max} = \frac{N(N-1)}{2} $$
The ***average path length*** is calculated the following way:
**Undirected:**
$$\langle d \rangle \equiv \frac{1}{2L_{\text{max}}}\sum_{i,j\neq i}d_{ij}$$
**Directed:**
$$\langle d \rangle \equiv \frac{1}{L_{\text{max}}}\sum_{i,j > i}d_{ij}$$

***Diameter:*** Shortest longest path or the maximum distance between any pair of nodes.

## Clustering Coefficient
"What fraction of your neighours are connected?"
The ***clustering coefficient*** is for each node $i$ different.
$$
C_i = \frac{2e_i}{k_i(k_i - 1)}
$$
$e_i$ - the edge count between the neighbours of $i$
$k_i$ - the degree of node $i$

The clustering coefficient 

# Centrality
## Degree Centrality
***Degree centrality:*** How many friends do you have?
You can normalise this or not
**Non-normalised:**
$$
\begin{align*}
C_D(i) =k_i \\
\\
\langle C_D \rangle = \frac{\sum^N_{i=1}k_i}{N}
\end{align*}
$$
**Normalised:**
$$
\begin{align*}
C_D(i) = \frac{k_i}{N - 1} \\
\\
\langle C_D \rangle = \sum^N_{i=1} \frac{C_D(i)}{N}
\end{align*}
$$
## Betweeness Centrality
***Betweenness centrality:*** How important are you in a network?
Nodes with a higher betweenness centrality are important for transporting information, they are a bride in a given cluster of the network.

Calculate shortest path between all pairs of nodes  
What fraction of those paths does i lie on (not as end or start point)
$$
\begin{align}
C_B(i) &= \sum_{h\neq i\neq j} \frac{\sigma_{hj}(i)}{\sigma_{hj}} \\
\end{align}
$$
$\sigma_{hj}(i)$ - number of shortest paths $i$ lies on between $h$ and $j$
$\sigma_{hj}$ - total number of shortest paths between $h$ and $j$

You can normalise this
$$
\begin{align}
C'_B(i) = \frac{C_B(i)}{\frac{(N-1)(N-2)}{2}}
\end{align}
$$

![[Pasted image 20241022114102.png]]![[Pasted image 20241022114048.png]]
All shortest paths?  
Undirected network so shortest path A->E = E->A  
A->B $[A,B]$, A->C $[A,B,C]$, A->D $[A,B,C,D]$, A->E $[A,B,C,D,E]$  
B->C $[B,C]$, B->D $[B,C,D]$, B->E $[B,C,D,E]$  
C->D $[C,D]$, C->E $[C,D,E]$,  
D->E $[D,E]$

$C_B (A) = 0$  
$C_B (B) = 3$  
$C_B (C) = 4$  
$C_B (D) = 3$  
$C_B (E) = 0$

## Closeness Centrality
***Closeness centrality:*** How far am I away from the rest of the network
$d(i,j)$ - distance from $i$ to $j$
**Non-normalised:**
$$
\begin{align*}
C_C(i) = \frac{1}{\sum^N_{j=1}d(i,j)} \\
\end{align*}
$$
**Normalised:**
$$
\begin{align*}
C_C(i) = \frac{N- 1}{\sum^N_{j=1}d(i,j)} \\
\end{align*}
$$
# Network Structures
![[Pasted image 20241022122003.png]]

| Network         | Pathlength                                                      | Clustering                                         | Degree Distribution                                           |
| --------------- | --------------------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------- |
| Regular Network | $l \approx N^{1/D}$                                             | $C\sim \text{const}$                               | $P(k)=\delta(k-k_d)$                                          |
| Erdos-Renyi     | $l_{\text{rand}}\approx \frac{\log N}{\log \langle k \rangle }$ | $C_{\text{rand}} = p =\frac{\langle k \rangle}{N}$ | $P(k) = e^{-\langle k \rangle}\frac{\langle k \rangle^k}{k!}$ |
| Watts-Strogatz  | $l_{\text{rand}}\approx \frac{\log N}{\log \langle k \rangle }$ | $C \sim \text{const}$                              | Exponential                                                   |
| Barabasi-Albert | $l \approx \frac{\ln N}{\ln \ln N}$                             | $C \sim \frac{(\ln N)^2}{N}$                       | $P(k) \sim k^{-gamma}$                                        |

***Erdos-Renyi /  Random Network***: A network is generated whereby each edge has a probability $p$ to be present or absent. 
***Watts-Strogatz / Small World Network:*** A network is generated with a bit of randomness, creating a network where nodes are grouped into clusters but are still globally connected through a few shortcuts.
***Barabasi-Albert / Preferential Attachment*** Each timestep new nodes are added with $m$ links that connects to $m$ new nodes already in the network based on a probability.

## Erdos-Renyi /  Random Network
A random graph will have in the beginning a lot of disconnected nodes but overtime and when the average degree $\langle k \rangle$ increases and a threshold is met, the whole network will fall in a clustered system.

![[Pasted image 20241022122412.png]]
Given a graph $G$ with 10 nodes. 
node $i$ connects with 4 of those nodes and not with the other 5.
The change of this is:
$$
\begin{align}
p \times p \times p \times p &\times (1-p) \times (1-p) \times (1-p) \times (1-p) \times (1-p) \\
&\rightarrow p^k(1-p)^{(N-1)-k} \\
P(k) &= \left(\frac{N-1}{k}\right)p^k(1-p)^{(N-1)-k} 
\end{align}
$$
Whereby:
$p$ - probability to connect
$(p-1)$ - probability to not connect
$\left(\frac{N-1}{k}\right)$ - select k nodes from $N-1$

This results in a normal distribution of nodes whereby
$\langle k \rangle = p(N-1)$
$\langle k^2 \rangle = p(N-1)(N-1)+p^2(N-1)^2$
	 it captures how much the values of a random variable deviate, on average, from the mean.
$\sigma^2_k = p(1-p)(N-1)$


If we don't know $N$ but we know that the network is large, we can use a [[Poisson Distribution]]:
$$
P(k) = e^{-\langle k \rangle} \frac{\langle k \rangle^k}{k!}
$$
This results in a normal distribution of nodes whereby
$\langle k \rangle = \langle k \rangle$
$\langle k^2 \rangle = \langle k \rangle(1+\langle k \rangle)$
	 it captures how much the values of a random variable deviate, on average, from the mean.
$\sigma^2_k = (\langle k^2 \rangle - \langle k \rangle^2)^{1/2} = \langle k \rangle^{1/2}$


These networks don't behave like real networks.
The average path length is around the same, but the clustering coefficient and degree distribution is wrong.

## Power Laws
Almost all real life networks follow a power law distribution 
$p(k) = k^{-\gamma}$
Whereby $2 < \gamma < 3$
![[Pasted image 20241022124438.png]]
Networks that follow this are called Scale-Free networks

## Barabasi-Albert / Preferential Attachment
It follows 2 principles:
**Growth**:
At each timestep we add a new node with $m (\leq m_0)$l links that connects the new node to $m$ nodes already in the network.
**Preferential attachment**:
The probablity $\Pi(k_i)$ that a link of the new node connects to node $i$ depends on the degree $k_i$ of node $i$ as:
$$
\Pi(k_i) = \frac{k_i}{\sum_jk_j}
$$
After $t$ timesteps the model generates a network with $N = t + m_0$ nodes and $m_0 + m t$ links.

Preferential attachment is an example of a positive feedback cycle where initially random variations are automatically reinforced, thus greatly magnifying differences. Within this system "the rich get richer"

The Barabási-Albert model emphasizes that network structure and evolution are inseparable, aiming to capture the processes that assemble networks, unlike other models that focus on connecting a fixed set of nodes. The model's key contribution is explaining the emergence of scale-free networks through the mechanisms of growth and preferential attachment, which are both necessary to generate such networks. However, the model raises the question of whether these two mechanisms explain all scale-free networks, with alternative models (e.g., link selection, copying, and optimization) also leading to scale-free structures without preferential attachment.
Despite its contributions, the Barabási-Albert model has limitations. It predicts a fixed degree exponent (γ=3) while real networks exhibit exponents between 2 and 5. It also generates undirected networks, whereas many real-world networks (e.g., the WWW or citation networks) are directed. The model overlooks processes like node/link disappearance and intrinsic node characteristics (e.g., novelty of research papers or utility of web pages). Ultimately, while it captures the fundamental principles behind scale-free networks, it is a simplified model that doesn’t account for the specific complexities of real systems, such as the Internet or the WW. To understand such systems' evolution, more detailed models are needed.
# Robustness
> Could we construct a network that is harder to be taken down by node failures?

***Robustness:*** The ability to withstand failure before a network will collapse. 
One can ***attack*** a network whereby hubs are being a removed or a network can have random ***failures*** whereby some nodes just fall out.

Scale-free networks are more error tolerant, but also more vulnerable to attacks.
This is because in a scale-free network their are less nodes with a high degree.
With a random network other nodes will easier fill in the role of hubs.

# Spreading in a network
Someone can be a super spreader if this individual infect hubs.
If a hub is infected a lot of other nodes will be affected.

Other things can also spread through a network like information and internet.
Spreading can only happen when the carrier and the other individuals are connected with one another.
![[Pasted image 20241022130730.png]]

## SI and SIS model
SI: Susceptible -> Infected
SI: Susceptible -> Infected -> Susceptible

## Behaviour of the SI model
Assuming homogeneous mixing
$$
\begin{align}
\frac{dI(t)}{dt} &= \beta \langle k \rangle \frac{S(t)I(t)}{N} \\
s &= S/N \\
i &= I/N \\
\\
\frac{di}{dt} &= \beta \langle k \rangle si\\
&= \beta \langle k \rangle i (1-i) \\
\frac{di}{i} + \frac{di}{(1-i)} &= \beta \langle k \rangle dt \\
\text{integrate both sides:} \\
\ln i -\ln(1-i)+c&=\beta \langle k \rangle t
\end{align}
$$
Hereby $\beta \langle k \rangle$ is the transmission rate.
Given the initial condition $i_0 = i(t=0)$ we get $c=i_0 / (1 - i_0)$, obtaining that the fraction of infected individuals increases in time as:
$$
i(t) = \frac{i_0e^{\beta \langle k \rangle t}}{1 - i_0+i_0e^{\beta \langle k \rangle t}}
$$
S from a logistic equation we go to a logistic function.
So if $i(t)$ is small:
$$
\begin{align}
\frac{di}{dt} &\approx \beta \langle k \rangle i \\
i &\approx i_0 e^{\beta \langle k \rangle t} 
\end{align}
$$
If $i(t) \rightarrow 1$:
$$
\frac{di}{dt} \rightarrow 0
$$
So within this model the fraction infected increases until everyone is infected.

## Behaviour of the SIS model
$$
\begin{align}
\frac{di}{dt} &= \beta \langle k \rangle i (1-i) - \mu i \\
i &= \left(1 - \frac{\mu}{\beta \langle k \rangle} \right)\frac{c e^{(\beta \langle k \rangle - \mu)t}}{1 + ce^{(\beta \langle k \rangle  - \mu) t}} 
\end{align}
$$
An endemic state happends when:
$$
\mu < \beta \langle k \rangle
$$
A disease free state when:
$$
\mu > \beta \langle k \rangle
$$

Within this model not everyone will get infected, but a stationary state will be reached when $t \rightarrow \infty$.

# Epidemics on Networks
When the assumption is made that their is homogeneous mixing, all the individuals can infect any other individuals. But in reality, an epidemic spreads along links / connections in a network.

A node with an higher degree will have an higher change to get infected, because their are more neighbors around them. They will also spread the disease more.

We split the nodes by their degree:
$$
\begin{align}
i_k = \frac{I_k}{N_k} \\
i = \sum_k P(k)i_k \\
\end{align}
$$
We can calculate the amount of infected of a given node using:
$$
\begin{align}
\frac{di_k(t)}{dt} = \beta(1-i_k(t))k\Theta_k(t)-\mu i_k(t)
\end{align}
$$
Whereby:
$\Theta_k$ - The density of infected neighbors of nodes with degree $k$.
$k\Theta_k(t)$ - Amount of infected neighbors.

I am susceptible with $k$ neighbors and $\Theta_k(t)$ of my neighbors are infected.

## Early Epidemic in SI and SIS
**SIS**
$$
\begin{align}
\frac{di_k(t)}{dt} = \beta(1-i_k(t))k\Theta_k(t)-\mu i_k(t)
\end{align}
$$
**SI**
$$
\begin{align}
\frac{di_k(t)}{dt} = \beta(1-i_k(t))k\Theta_k(t)
\end{align}
$$
So early in an epidemic:
$$
i_k(t) \approx 0
$$
Therefore:
$$
\frac{di_k}{dx} \approx \beta k \Theta_k(t)
$$
## Derivations
We assume that their is no degree correlation in the network.
So the probability that a node with degree $k$ connects to a node with degree $k'$ is independent of $k$.
This makes that the probability that a random link points to a node with degree $k'$ is:
$$
\frac{k' P_{k'}}{\sum_k k p_{k'}}
$$
Whereby $p_{k'}$ is the probability of finding a node with degree $k'$.
The number of neighbors who can be infected $(k'-1)$.
$$
(1)\;\; \Theta_k(t) = \frac{\sum(k'-1)P_{k'}i_{k'}(t)}{\langle k \rangle}
$$
So without degree correlations $\Theta_k(t)$ is independent of $k$
To get the rate of change for the number of neighbors who can be infected:
$$
(2)\;\; \frac{d\Theta}{dt} = \sum_k \frac{(k-1)P_k}{\langle k \rangle} \frac{di_k}{dt}
$$
For this the number of susceptible are dropped because they are independent of k.

**SI**
$$
(3)\;\;\frac{di_k}{dt} = \beta(1-i_k)k\Theta_k(t)
$$
We substitute eq (3) in (2):
$$
\begin{align}
\frac{d\Theta}{dt} &= \sum_k \frac{(k-1)P_k}{\langle k \rangle} \beta(1-i_k)k\Theta_k(t) \\
&= \beta\sum_k\frac{(k^2-k)P_k}{\langle k \rangle} (1 - i_k(t))\Theta(t)
\end{align}
$$
In the early time behavior t is small and $i_k(t) \approx 0$
$$
\begin{align}

\frac{d\Theta}{dt}&= \beta\sum_k\frac{k^2P_k-kP_k}{\langle k \rangle} \Theta(t) \\
&= \beta\sum_k\frac{\langle k^2 \rangle - \langle k \rangle}{\langle k \rangle} \Theta(t) \\
\\
\text{given}:\\
\langle k \rangle &= \sum_kkP_k \\
\langle k ^2\rangle &= \sum_kk^2P_k \\
\\
&= \beta \left( \frac{\langle k^2 \rangle}{\langle k^ \rangle} - 1\right) \Theta(t)\\
\\
\Theta(t)&= Ce^{t/\tau}\\
\text{Whereby:}\;\tau= \;\text{characteristic time}\; &= \beta \left( \frac{\langle k \rangle}{\langle k^2 \rangle - \langle k \rangle} \right) \\
\text{Using the initial condition}\; \Theta (t=0) &= c = i_0 \frac{\langle k \rangle -1}{\langle k \rangle} \\
(4) \;\;\Theta (t)&= i_0 \frac{\langle k \rangle -1}{\langle k \rangle} e^{t/\tau}
\end{align}
$$
If we put eq (4) in (3)
$$
\begin{align}
\frac{di_k}{dt} &= \beta(1-i_k)k i_0 \frac{\langle k \rangle -1}{\langle k \rangle} e^{t/\tau} \\
&= \beta_k i_0 \frac{\langle k \rangle -1}{\langle k \rangle} e^{t/\tau} \\
i_k &= i_0\left[1+\frac{k\langle k \rangle -1}{\langle k^2 \rangle - \langle k^2 \rangle} \left(e^{t/\tau}-1 \right) \right]
\end{align}
$$
For the SIS model

$$
\begin{align}
\frac{di_k}{dt} = \beta(1-i_k)k\Theta_k(t)-\mu i_k \\
\tau = \frac{\langle k \rangle}{\beta\langle k^2 \rangle- \langle k \rangle \mu}
\end{align}
$$
The higher the degree of the node the more likely that it will become infected.

A key distinction between the SIS model and the SI or SIR models lies in the number of susceptible neighbors a node can have. In SI and SIR models, an infected node must have at least one infected or recovered neighbor, limiting the number of susceptible neighbors. However, in the SIS model, previously infected neighbors can become susceptible again, meaning all the node’s connections are potential pathways for spreading the disease. Therefore, the infection spread calculation is adjusted to account for this difference.

## Random networks
For random networks the second moment is defined as:
$$
\begin{align}
\langle k^2 \rangle &= \langle k \rangle^2+\langle k \rangle \\
&= \langle k \rangle(\langle k \rangle + 1)\\
\\
\text{given the characteristic time for SIS}: \\
\tau^{SIS} &= \frac{\langle k \rangle}{\beta\langle k^2 \rangle- \langle k \rangle \mu} \\
\tau^{SIS}_{ER} &= \frac{\langle k \rangle}{\beta\langle k \rangle(\langle k \rangle + 1)- \langle k \rangle \mu} \\
&= \frac{1}{\beta(\langle k \rangle + 1) -\mu}
\\
\tau^{SIS}_{ER} &=  \frac{1}{\beta(\langle k \rangle + 1) -\mu} > 0
\end{align}
$$
Given spreading rate defined as:
$$
\lambda = \beta / \mu
$$
The pathogen persists when:
$$
\begin{align}
\lambda &>\frac{1}{\langle k \rangle +1}\\
\lambda_c &=\frac{1}{\langle k \rangle +1}
\end{align}
$$
As $\langle k \rangle$ is always finite, a random network always has a nonzero epidemic threshold, with key consequences:  
- If the spreading rate $λ$ exceeds the epidemic threshold $λ_c$ , the pathogen will spread until it reaches an endemic state, where a finite fraction $i(λ)$ of the population is infected at any time.  
- If $λ < λ_c$ , the pathogen dies out, i.e. $i(λ)=0$.

## Scale free
For a network with an arbitrary degree distribution we set $\tau^{SIS} > 0$ obtaining the epidemic threshold as:
$$
\lambda_c = \frac{\langle k \rangle}{\langle k^2 \rangle}
$$
As for a scale-free network $\langle k^2 \rangle$ diverges in the $N \rightarrow \infty$ limit, for large networks the epidemic threshold is expected to vanish.
This means that even viruses that are hard to pass from individual to individual can spread successfully

1. In a large scale-free network $\tau = 0$, which means, that a virus can instantaneously reach most nodes.
2. In a large scale-free network $\lambda_c = 0$, which means that even viruses with small spreading rate can persist in the population.

# Vaccinations
Vaccinations is a way to attack the network.
If the robustness of a network is low, vaccinations can immunized a lot of clusters.

For vaccinations a density $g$ individuals are randomly chosen to be immunized.
$$
\begin{align}
\beta &\rightarrow \beta(1-g) \\
\frac{(1-g)\beta}{\mu}&=\frac{1}{\langle k \rangle +1} \\
\frac{\beta}{\mu}(1-g_c)&=\frac{\langle k \rangle}{\langle k^2 \rangle} \\
g_c &= 1 - \frac{\mu}{\beta}\frac{\langle k \rangle}{\langle k^2 \rangle} \\
\end{align}
$$
Consequently, if vaccination increases the fraction of immunized individuals above $g_c$, it pushes the spreading rate under the epidemic threshold $\lambda_c$. In this case $\tau$ becomes negative and the pathogen dies out naturally. This explains why health official focus on vaccinating the population.

If $\langle k^2 \rangle \rightarrow \infty$, random immunization cannot prevent the outbreak.

## Vaccination in Scale-Free
Scale-free network $\langle k^2 \rangle$ diverges in the $N\rightarrow \infty$ limit
The hubs are responsible for the spread of the disease, but in many cases we cannot figure out who the hubs are.

If we follow an edge, you are likely to meet the high-degree nodes!
![[Pasted image 20241022143904.png]]