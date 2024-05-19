Notes taken from **P.D. Hoff, A First Course in Bayesian Statistical Methods**

## Poisson model

If $Y|\theta \sim Pois(\theta)$ then the pdf of y given $\theta$ is $P(Y=y|\theta) = e^{-\theta}\frac{\theta^y}{y!}$

Suppose $y = (y_{1}, \dotsc,y_{n})$ & $y_{i} \sim^{iid} \; Pois(\theta)$, then

$$
\begin{aligned}
P(y|\theta) = \prod_{i=1}^n p(y_i|\theta) &= \exp(-n\theta)\;\theta^{\sum y_i}\; \frac{1}{\prod y_i!}\\
&= f(t,\theta)g(y) \quad \text{where}\; t= \sum y_i \;\text{is a sufficient statistic for} \; \theta \; and \; T\sim Pois(n\theta)
\end{aligned} 
$$

Consider the gamma prior. where $\theta \sim gamma(a,b)$ then $p(\theta) = \frac{b^a}{\Gamma(a)} \theta^{a-1} \exp(-b\theta)$ and $E(\theta) = a/b, Var(\theta) = a/b^2, Mode = (a-1)/b \; \text{if a } \gt 1 $

### Posterior

$$
\begin{align*}
p(\theta|t) &\propto p(t|\theta)p(\theta) \\
&\propto \theta^t \exp(-n\theta) \cdot \theta^{a_0 -1} \exp(-b_0\theta) \\
&\propto \theta^{a_0 +t-1} \exp(-(b_0+n)\theta)
\end{align*}
$$

Clearly $\theta|y \sim gamma(a_0 +t,b0+n)$

In fact, $E(\theta|y) = \frac{a_0 + t}{b_0 + n} = \frac{b_0}{b_0 + n}(\frac{a_0}{b_0}) + \frac{n}{b_0 + n}(\frac{t}{n})$ and $Var(\theta|y) = \frac{a_0+t}{(b_0 + n)^2}$

### Posterior predictive distribution 

$$
\begin{aligned}
P(\tilde{Y} = \tilde{y}|y) &= \int_0^\infty P(\tilde{Y} = \tilde{y}|\theta)P(\theta|y) d\theta\\
&= \int_0^\infty e^{-\theta}\frac{\theta^{\tilde{y}}}{\tilde{y}!} \cdot \frac{b^a}{\Gamma(a)} \theta^{a-1} \exp(-b\theta) \\
&= \frac{b^a}{\Gamma(a)\tilde{y}!} \frac{\Gamma(a+\tilde{y})}{(b+1)^{a+\tilde{y}}} \int_0^\infty \frac{(b+1)^{a+\tilde{y}}}{\Gamma(a+\tilde{y})} \theta^{a+\tilde{y} -1}e^{-(b+1)\theta}\\
&= \frac{\Gamma(a+\tilde{y})}{\Gamma(a)\tilde{y}!} (\frac{b}{b+1})^a(\frac{1}{b+1})^{\tilde{y}}\\
&= \frac{(a+\tilde{y}-1)!}{(a-1)!\tilde{y}!} (\frac{b}{b+1})^a(\frac{1}{b+1})^{\tilde{y}}\\
&= {a+\tilde{y}-1\choose\tilde{y}!} (\frac{b}{b+1})^a(\frac{1}{b+1})^{\tilde{y}} \implies \text{this is the pdf of a negative binomial distribution for a >1 }\\
\end{aligned}
$$

Hence, $\tilde{Y}|y \sim NB(a,\frac{b}{b+1})$ and $E(\tilde{Y}|y ) = \frac{a}{b} = E(\theta|y)$ ,and $Var(\tilde{Y}|y) = \frac{a(b+1)}{b^2} = Var(\theta|y)\cdot(b+1)$

The predictive variance is a measure of uncertainty on prediciton $\tilde{Y}$. The uncertainty stems from the uncertainty within the population and the variability in sampling from population