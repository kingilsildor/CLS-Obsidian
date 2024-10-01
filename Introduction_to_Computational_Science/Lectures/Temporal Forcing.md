Period of forcing enables maximum amplification if th


Harmonic Resonance: when the frequencies are the same.
Subharmonic Resonance:

# Bifurcation Diagrams
How to visualise the changing dynamics as a function of a control parameter?

## [Logistic map](Logistic%20Map)
$$X_{t+1} = rX_t(1-X_t) $$
$r =$ growth rate due to birth

within a logistic map you plot 
Preferable to keep it 2D

# Vaccination
Vaccination is the same as changing $\mu$ because these people to straight to the R pool.
High birth rate can lead to annual patterns.

$$
\begin{align}
\frac{dS}{dt} = \mu(1-p) - \beta S I - \mu S, \\
\frac{dI}{dt} = \beta S I - \gamma I - \mu I, \\
\frac{dR}{dt} = \gamma I + \mu p - \mu R.
\end{align}
$$
Rescade $\beta$
$$
\begin{align}
S = S'(1-p), \\
I = I'(1-p), \\
R = R'(1-p)+p.
\end{align}
$$
$$
\begin{align}
(1-p)\frac{dS'}{dt} = \mu (1-p) - \beta S'(1-p)I'(1-p)- \mu(S'(1-p)), \\
(1-p)\frac{dI'}{dt} = \beta S'(1-p)I'(1-p) - \gamma(I'(1-p)) - \mu(I'(1-p)), \\
(1-p)\frac{dR'}{dt} = \mu p + \gamma(I'(1-p)) - \mu(R'(1-p)+p).
\end{align}
$$
$$
\begin{align}
\frac{dS'}{dt} = \mu - \beta S'I'(1-p) - \mu S'. \\
\frac{dI'}{dt} = \beta S' I' (1-p) - \gamma I' - \mu I'.\\
\frac{dR'}{dt} = \gamma I' - \mu R'.
\end{align}
$$
 So $\beta \rightarrow \beta (1-p)$
 


