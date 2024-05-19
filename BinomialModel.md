Notes taken from **P.D. Hoff, A First Course in Bayesian Statistical Methods**

## Binomial Model

Consider a uniform(0,1) prior such that $p(\theta) = 1$

We can apply bayes rules to get the posterior :

$$
\begin{align*}
p(\theta|y) &\propto p(y|\theta)p(\theta) \\
&\propto \theta^y (1-\theta)^{n-y}
\end{align*}
$$

Hence $p(\theta|y) = \frac{\int_{0}^{1} \theta^y (1-\theta)^{n-y} \,d\theta}{C}$ for some normalizing constant C

For $p(\theta|y)$ to be a proper density 

$$
\int_{0}^{1} p(\theta|y)\,d\theta = \frac{1}{C} \int_{0}^{1}\theta^y (1-\theta)^{n-y} \, d\theta= 1 \\
\implies C = \int_{0}^{1}\theta^y (1-\theta)^{n-y} \, d\theta
$$

From calculus we know that $B(a,b) = \int_{0}^{1} \theta^{a-1} (1-\theta)^{b-1} \, d\theta $

Thus C = $B(y+1,n-y+1)$

Clearly,

$$
\begin{align*}
\theta &\sim Uniform(0,1) &\implies \theta|y \sim Beta(y+1,n-y+1) \\
Y|\theta &\sim Binomial(n,\theta)
\end{align*}
$$

### Properties of Beta
if $\theta \sim beta(a,b)$
- $E(\theta) = \frac{a}{a+b}$
- $Var(\theta) = \frac{ab}{(a+b)^2(a+b+1)}$
- if a>1 and b>1 then the mode is given by $\frac{a-1}{a+b-2}$

### In general
For a beta prior with $\theta \sim Beta(a_{0},b_{0})$ then

$$
\begin{align*}
p(\theta|y) &\propto p(y|\theta)p(\theta) \\
&\propto {n \choose y} \theta^y (1-\theta)^{n-y} \frac{1}{B(a_{0},b_{0})}\theta^{a_{0} - 1} (1-\theta)^{b_{0}-1} \\
&\propto \theta^{a_{0} +y - 1} (1-\theta)^{n-y+b_{0}-1}
\end{align*}
$$

hence, $p(\theta|y)$ has the same shape as pdf of $Beta(a_{0} +y,b_{0} + n -y)$

$$
\begin{align*}
\theta &\sim Beta(a_{0},b_{0}) &\implies \theta|y \sim Beta(a_{0} +y,b_{0} + n -y) \\
Y|\theta &\sim Binomial(n,\theta)
\end{align*}
$$

## Combing info
Recall that $E(\theta|y) = \frac{a+y}{a+y+b+n-y}$

$$
\begin{align*}
E(\theta|y) = \frac{a+y}{a+b+n} &= \frac{a+b}{a+b+n}( \frac{a}{a+b}) + \frac{n}{a+b+n}( \frac{y}{n}) \\
& = \frac{w}{w+n}\theta_{0} + \frac{n}{w+n}\bar{y} \\
& \text{where} \; \theta_{0} \; \text{is the prior mean and} \; \bar{y} \; \text{is the sample mean}
\end{align*}
$$

The posterior mean is a weighted average of the prior mean and the sample mean with weights proportional to $a_{0} + b_{0} \; \text{and} \; n$ respectively

We can think of $a_{0} \;, b_{0}$ as 'prior data' with 
- $a_{0}$ as prior number of 1's 
-  $b_{0}$ as prior number of 0's
- and $a_{0} + b_{0}$ as prior sample size

We can see that if n increase, the weight on sample mean increases.  if $n > a_{0}+ b_{0}$, the majority of info about $\theta$ will come from our data insetad of prior.
- if $n >> a_{0}+ b_{0}$ then $E(\theta|y) \approx \frac{y}{n}$




