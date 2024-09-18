# [Mathematical Topics](Mathematics)
## [[Derivative]]
The **instantaneous rate of change** of a function
$$\frac{ds}{dt}=\lim_{dt\to 0}\frac{s(t+dt)-s(t)}{dt}$$
Whereby $h$ is a small change
When using this formula:
$$
\begin{align*}
f(x) = x^2 \\
f'(x) = \frac{(x+dx)^2-x^2}{dx} \\
f'(x) = \frac{x^2+2xdx+dx^2-x^2}{dx} \\
f'(x) = \frac{2xdx+dx^2}{dx} \\
f'(x) = 2x+dx \\
f'(x) = \lim_{dx\to 0}2x+dt \\
f'(x) = 2x
\end{align*}
$$
### Constant factor rule
$$
\begin{align*}
c'= 0\\
(c \cdot f)'= c \cdot f\\
\end{align*}
$$
### Sum rule
$$
\begin{align*}
(f+g)'= f' + g'\\
(f-g)'= f' - g'\\
\end{align*}
$$
### Product rule
$$
\begin{align*}
(f \cdot g)'= f' \cdot g + f \cdot g'\\
(\frac{f}{g})'= \frac{f' \cdot g - f \cdot g'}{g^2}\\
\end{align*}
$$

### Chain rule
$$
\begin{align*}
(f \circ g)' = (f' \circ g) \cdot g'
\end{align*}
$$

### Inverse function rule
## [[Taylor Expansions]]
## [[Logarithm]]

## [[Trigonometric Functions]]
## [[Limits]]
## [[Integration]]
### Substitution rule / reverse chain rule
$$
\begin{align*}
\int f(g(x)) \cdot g'(x)dx = [u=g(x), du=g'(x)dx] \\
= \int f(u)du
\end{align*}
$$
When doing the substitution you set a variable to $u$: $\int z(3z-1)^3 dz$.
So $u=3z-1$, $dz = \frac{1}{3}du$.
Afterwards you express $z$ in terms of $u$: $z=\frac{u+1}{3}$
