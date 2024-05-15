# Bayesian_stats
In bayesian stats, we need to specify a sampling model and a prior distribution 

The sampling distribution is a probability distribution that describes how $y$  depends $\theta$  and is expressed as a probbability density function $P(y|\theta)$ - which we call the **likelihood** function

The **prior** distribution describes any prior knowledge or beliefs that we have about $\theta$ before observing the data and is expressed as $P(\theta)$

The prior and likelihood are used to compute the conditional distribution of $\theta$ given the data $y$ using Bayes' theorem. The conditional distribution id called the posterior distribution.

$$
P(\theta|y) = \frac{p(\theta,y)}{p(y)} = \frac{p(\theta,y)}{\int p(y,t) dt} = \frac{ p(y|\theta) p(\theta)}{\int p(y|t) p(t) dt}
$$

If $\theta$ is discrete, the integral will be replaced by a sum over all possible values of $\theta$

Here we can see that $P(\theta|y) \propto P(y|\theta) P(\theta)$

The denominator ${p(y)} = \int p(y|t) p(t) dt$ is known as marginal distribution of data y
- We can think of this as a normalizing constant that allows $\int P(\theta|y) d\theta = 1$
- In complex models, this intergal is often intractable

## Comparison to non bayesian methods
Baysian statistics provide a formal approach to combine information from different sources. 
- It allows compromise estimates to emerge naturally ( weighted average of sample mean and prior mean ) and automatically gives increasing weight to the direct estimate as it becomes more reliable
- Provides stable inference and estimation when the sample size is low

## Sequential use of bayes' rule
Suppose we have 2 sets of independently collected $y_{1}$ and $y_{2}$.
The posterior for the full data set is:

\begin{align*}
P(\theta|y_{1},y_{2})  & \propto P(y|\theta) P(\theta) \\
& = P(y_{1}|\theta)P(y_{2}|\theta)P(\theta) \; [y_{1} \:\text{\&} \: y_{2} \:\text{are conditionally independent}] \\
& \propto P(y_{2}|\theta)P(\theta|y_{1})
\end{align*}


We can find the posterior distribution of $\theta$ given $(y_{1},y_{2})$ by treating the posterior $P(\theta|y_{1})$ as the prior distribution of $y_{2}$
- this can be applied when data arrive sequentially and independently over time

## Posterior predictive distribution

Suppose we have observed data $y = (y_{1}, \dotsc, y_{n})$ and want to find prediction of the value of a future observation $\tilde{Y}$. The posterior predictive distribution of $\tilde{Y}$ is :

$$
P(\tilde{y}|y) = \int p(\tilde{y},\theta|y) d\theta = \int p(\tilde{y}|\theta,y) p(\theta|y) d\theta = \int p(\tilde{y}|\theta) p(\theta|y) d\theta
$$

The distribution $P(\tilde{y}|y)$ summarizes the information concerning the likely value of a new observation, given the likelihood and prior distribution and data we have observed. 

## Conjugacy
A class $P$ of prior distribution is called **conjugate** for a sampling model $p(y|\theta)$ if 
$p(\theta) \in P \; \implies \; p(\theta|y) \in P$

i.e. conjugate prior to a sampling model belong to same family as posterior
- binomial and beta conjugate 
- poisson and gamma conjugate


## Sufficient statistic
Let t be a function of observations $y = (y_{1}, \dotsc,y_{n})$

the statistic t is called a sufficient statistic for $\theta$ given $y$ if : $p(y|t,\theta) = p(y|t)$

The above definition says that the conditional distribution of y, given the statistic t, does not depend on parameter $\theta$

To show t is a sufficient statistic, amounts to showing that $p(y|t,\theta)$ is independent of $\theta$ i.e. $p(y|t,\theta) = p(y|t)$

For any prior distribution, the posterior distribution $p(y|\theta)$ is the same as $p(y|t)$

### Factorization Theorem

To find sufficient statistic, we can use this theorem

A statistic t is sufficieint for $\theta$ given y $\iff$ there $\exists \; \text{functions f and g s.t.} \\ p(y|\theta) = f(t,\theta)g(y)$
- in binomial model, $f(t,\theta) = \theta^{t}(1-\theta)^{n-t}$ and $g(y) = 1$

## Interval estimation / Credible Set

A $100(1-\alpha)$% credible set for $\theta$ is a subset of $C$ of $\Theta$ s.t. 

$$
P(\theta \in C| y) = \int_{C} p(\theta|y) d\theta \ge 1-\alpha
$$

We used $\ge$ to accomodate discrete settings as well. In continuous setting, the set will be exact coverage

### Intepretation of credible sets
We treat the unknown parameter $\theta$ as a r.v. and the interval is fixed once data is observed

We say that the probability of $\theta$ lies in $C$ given observed data y is $1-\alpha$

In contrast, for frequentist stats, Y is regarded as random and give rise to a random interval which has prob $1-\alpha$ of containing the fixed but unknown $\theta$

I.e. if we could recompute $C$ for a large number of data sets collected in same way, about $100(1-\alpha)$% of them will contain the true value of $\theta$

### Quantile based / equal-tails intervals
We find 2 numbers $\theta_{\alpha/2} < \theta_{1 - \alpha/2}$ s.t. 

\usepackage{amsmath}
$$
P(\theta < \theta_{\alpha/2}) = \alpha/2 \; \& P(\theta > \theta_{1-\alpha/2}) = \alpha/2 \\
\implies P(\theta_{\alpha/2}< \theta <\theta_{1-\alpha/2} ) = 1-\alpha
$$

- e.g. posterior $\theta|y \sim Beta(3,9)$ , then we can use `qbeta(c(0.025,0.975),3,9)` to get the 95% quantile based CI

### Highest Posterior Density (HPD) region

The highest posterior density (HPD) credible set is defined as the set :
$$
C = \{\theta \in \Theta : p(\theta|y) \ge k(\alpha) \}
$$
where $k(\alpha)$ is the largest constant satisfying $P(\theta \in C|y) \ge 1-\alpha$

We can visualise the HPD region, by drawing a horizontal line and pushing it donw until the corresponding values of the $\theta$ - axis trap the appropriate probability

``` r
library(TeachingDemos)
hpd(qbeta,shape1 = a, shape = b, conf = 0.90)
```
or
```r
library(coda)
rand_sp = rbeta(10^6,3,9)
mcmc_obj = mcmc(rand_sp)
HPDinterval(mcmc_obj,prob = 0.95)
```
## Non informative prior

In some cases, no prior information exist or inference based dominantly on the data is dersired

For instance, suppose the parameter space is discrete and finite ie $\Theta = \{\theta_{1},\dotsc,\theta_{n}\}$
then the distribution $p(\theta_{i}) = 1/n$ does not favour any other value - as such it is a noninformative piror. If we have a bounded continuous paramter space we can use the uniform distribution $p(\theta) = \frac{1}{(b-a)}$

However when the parameter space is unbounded, suppose $\Theta = (-\infty,+\infty)$

We consider a uniform distribution over the real line such that $p(\theta)= c$ for $-\infty < \theta < \infty$
- we use the notation $p(\theta) \propto 1$

This distribution is **improper** as $\int_{-\infty}^{\infty} p(\theta) d\theta = +\infty$ for $\forall\; c>0$

Sometimes, an improper prior can be combined with the likelihood to give a proper posterior. proper posteriros will not always result.
### Reference Piror

The noninformative priors are closely related to the notion of a reference prior. 
Not all non informative prior  are always invaraint under reprameterization

A remedy to this problem is to rely on the particular modelling context to provide then most reasonable parameterization and subsequently apply the uniform prior on this scale.

### Jeffreys prior

the jeffreys prior is invariant to transformation

If the model only has a univariate parameter $\theta$, this prior is given by $p(\theta) \propto \; \sqrt{I(\theta)}$ where $I(\theta)$ is the expected Fisher information in the model:

$$
\begin{equation*}
I(\theta) = - E_{Y|\theta}[\frac{\partial^2}{\partial \theta^2} \log p(y|\theta)]
\end{equation*}
$$
- note that the form of the likelihood helps to determine the prior but not the actual observed data since we are averaging over Y


If $\theta$ is multi-dimensional, the jeffreys prior is given by $p(\theta) \propto \; \sqrt{\det \{ I(\theta) \}}$

and $I(\theta)$ is the expected Fisher information matrix whose $(i,j)$th entry is :

$$
\begin{equation*}
I_{ij}(\theta) = - E_{Y|\theta}[\frac{\partial^2}{\partial \theta_{i} \theta_{j}} \log p(y|\theta)]
\end{equation*}
$$

This can be cumbersome to use if the dimension of $\theta$ is high

A more common approach is to obtain a non-informative prior for each parameter and then form the joint prior as the product of these individual priors. 


