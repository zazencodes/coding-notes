## Week 2 - Beyesian Inference

- This module discusses:
    - Using Bayes' for continuous random variables
    - Understanding conjugacy
    - Making inferences

### Continuous Variables and Eliciting Probability Distributions

- The binomial distribution is the discrete equivalent of the gaussian distribution
- pmf = discrete, e.g.:
    - binomial
    - Poisson
- pdf = continuous, e.g.:
    - normal
    - uniform
    - beta (note: beta(1, 1)==uniform)
    - gamma
- Bayes' rule ensures that the change in probabilities from prior to posterior is the uniquely rational solution. With enough data, the posterior should converge regardless of prior beliefs
- Conjugacy occurs when the new belief (i.e., the posterior distribution) is in the same family as your prior belief but with new parameter values (which have been updated)
    - e.g. The binomial distibution is a conjugate pair with the beta function. With prior belief that data comes from a binomial distribution with known n and unknown p where p comes from the beta pdf with parameters alpha and beta. Then observing x successes in n trials we should update beta(alpha, beta) -> beta(alpha+x, beta+n-x)
- Conjugacy means that we can simply the continuous version of Bayes' rule without having to do the integral. On the other hand, we can take the integrals using computers without the prior probabilities having to be conjugate pairs.


###  Three Conjugate Families

1) beta-binomial family
- We consider an example where there are 700 women who take morning after treatments. Either they do the standard treatment or RU-486. We can model the probability of the therapy NOT working (i.e. pregnancy) using the binomial distribution, that is: if pregnant the type of therapy can be determined by the binomial distribution (i.e. a weighted coin)
- Frequentist approach:
    - Null hypothesis: p >= 0.5 (i.e. RU-486 is just as good or better), alternate hypothesis p < 0.5
    - p-value = 0.5^4 = 0.0625. This is greater than 0.05 (a commonly used threshold) therefore we DO NOT reject the null hypothesis and conclude that RU-486 is just as good or better - although its not conclusive that RU is superior
    - The p-value (the significance probability) is the chance of observing data that is as or more supportive of the alternative hypothesis that the data that was collected, when the null hypothesis is true
- Bayesian approach:
    - Initially we have a uniform distribution given by beta(1, 1)
    - The posterior probability can then be calculated as beta(1+0, 1+4) using conjugacy and the fact that we had 4 trials and 0 successes (RU-486 pregnancies)
    - We now have an average probability of alpha/(alpha+beta) = 1/6 +/- 0.13
    - We could conclude (using calculus but they won't show us) that the p<0.5 hypothesis is 96.8% probable

2) gamma-Poisson family
- Data comes from a Poisson distribution and the prior and posterior are both Poisson distributions
- The Poisson distribution has a single parameter lambda which is both the mean and variance and the function is only defined for positive integers
- The gamma function can be parameterized by two parameters: theta and k, where the mean is theta*k and the standard deviation is theta*sqrt(k)
- After observing events, we can update the gamma parameters in a similar way to how we updated the beta parameters earlier
- In general, the Poisson distribution is good for count data

3) normal-normal family
- If data comes from a normal distribution with known standard deviation but unknown mean, where the prior knowledge of mu has some mean and standard deviation (i.e. is normally distributed) then the posterior density is also normally distributed
- Must assume that the data points are independent
- The posterior distribution for mu (after seeing the data) has mean equal to a weighted average of the prior mean and sample mean
- Consider an example where a chemist is weighing ammonium nitrate:
    - The scale has a known standard deviation of 0.2 mg
    - She guesses the sample mass to be 10 +/- 2 mg
    - Based on this, she guesses the prior weight distribution to be normal with mean 10 and standard deviation 2
    - She collects 5 measurements and finds the average to be 10.5 mg
    - Updating the prior normal distribution she updates the mean to 10.499 and standard deviation to 0.089

### Non-Conjugate Priors
- Sometimes beliefs about a system can not be expressed in terms of a convenient conjugate prior. e.g. the RU-486 example where a reasonable prior is uniform from 0-0.5 and has a point mass of 50% at 0.5. This is a reasonable prior because we have no reason to believe that it's worse than the standard therapy (i.e. p>0.5) but it either equivalent (0.5) or better
- The posterior can be approximated in this case using computational sampling methods

### Credible Intervals
- These are the Bayesian alternative to confidence intervals of the frequentist method
- The credible interval is given by the point estimate from the sample +/- the standard error * a critical vlue
- A e.g. 95% confidence interval can be interpreted as follows:
    - 95% of similarly constructed intervals will contain the true mean
    - NOT the probability that the true mean lies within the bounds is 95% because it either is with prob=100% or is not with prob=0%
- A e.g. 95% credible interval can however be interpreted as a probability:
    -the probability the true mean is contain within the interval is 95%
- As such, any credible interval is valid so long as the value is equal to the area under the probability distribution for the lower and upper bounds. The shortest such interval is the idela choice.
- Unlike the frequentist equivalent, this interval is often asymmetric

### Predictive Inference
- This arises when the goal is to find the posterior distribution over some random variable that depends on a parameter (as opposed to finding the posterior distribution over just that parameter)
- One example is, with reference to the prussian cavalry study, to determine the number of officers run down by horses as opposed to the number of soldiers in general
- Consider an example where we have two weighted coins. One coin has P[heads]=0.7 and the oher has P[heads]=0.4. You draw one coin at random
    - Prior belief = 0.5
    - The game starts and you draw a coin and toss two heads. The posterior probability of drawing heads can be determined from Bayes' rule as 0.754
    - You are interested in the predictive probability of flipping a heads if you flip again
    - We know the probability that our coin is the p[heads]=0.4 one is 0.246 so we can determine the probability of getting heads as P[heads]=0.754*0.7 + 0.246*0.4 = 0.626
- Most realistic problems are more complicated and require integrals

