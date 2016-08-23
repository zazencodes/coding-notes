## Week 1 - Bayes' Rule   

- P(A|B) = P(A & B) / P(B)
- Named after Thomas Bayes (~1700-1760)

### Diagnostic Testing
- True positive == **sensitivity**
- True negative == **specificity**
- e.g. military recruit HIV testing
    - P(HIV) and P(no HIV) prior to testing == **prior** probabilities. e.g. percentage of population diagnosed +ive
    - P(HIV|+ive test) == **posterior** probability example

### Bayesian Updating
- First branches of probabliity tree contain prior probabilities
- These priors should be updated if calculating things like e.g. P(HIV|2 +ive tests)
    - In this case the priors are P(HIV|+ive test) and P(no HIV|+ive test)

### Bayesian vs Frequentist Probability
- Frequentist definition => percentage of occurances divided by N as N->infinity

### Inference for a Proportion: Frequentist Approach
- The **p-value** is the probability that an observed or more extreme (i.e. in the direction of the alternative hypothesis) outcome, given that the null hypothesis is true
- The number of successes in a fixed number of trials for a categorical variable with two levels follows a binomial distribution
- Problems can be approached by defining a null hypothesis (i.e. its equally likely that either event occurs) and finding the probability of the case happening given the null hypothesis
```R
# In R we calculate the p-value by evaluating the binomial distribution P(k<=4)
sum(dbinom(0:4, size=20, p=0.5)) # 0-4 positive results out of 20
```

### Inference for a Proportion: Bayesian Approach
- For our hypothesis:
    - Delineate plausable models e.g. p could be 0.1, 0.2, 0.3 ...etc
    - We'll consider each model to have a **prior** probability of being true. This should be based on previous knowledge/information and **not** the current experiment
    - e.g. we could split up the priors as: p **prior** = 0.1 **0.06**, 0.2 **0.06**, 0.3 **0.06**, 0.4 **0.06**, 0.5 **0.52**, 0.6 **0.06**, etc
- We then calculate the likelihood P(data|model) for each model
- Using the binomial distribution, we calculate P(k=4, n=20, p) for each model\
```R
p <- seq(from=0.1, to=0.9, by=0.1)
prior <- c(rep(0.06, 4), 0.52, rep(0.06, 4))
likelihood <- dbinom(4, size=20, prob=p)
```
- After finding likelihoods, use Bayes' rule to calculate the posterior probability P(model|data)
    - P(model|data) = P(model & data) / P(data) = P(data|model)*P(model) / P(data)
```R
numerator <- prior*likelihood # P(model) is the prior, P(data|model) is the likelihood
denominator <- sum(numerator) # P(data), the sum of all possible probabilities
posterior <- numerator / denominator
print(sum(posterior))
```
- This Bayesian paradigm (as opposed to the frequentist approach) allows us to make direct probability statements about our models
    - In this example we can sum up the posteriors of the models where p<0.5 to find the probability of the treatment being effective

### Effect of the Sample Size on the Posterior
- The general technique learned here was
    - Define a **prior** distribution
    - Calculate the **likelihood** using the binomial distribution
    - Calculate the **posterior** using Bayes' theorem
- When adding more data, the likelihood and posterior distributions become increasingly more peaked at the maximum p value
    - This is because the likelihood dominates the prior
    - We would not see this trend if setting some of the priors to zero

### Frequentist vs Bayesian Inference
- Consider that you want to guess the percentage of yellow M&Ms in a random sample
- Frequentist method
    - Our null hypothesis is that we have 10% yellow
    - The alternative hypothesis is that we have >10% yellow
    - We'll define a significance level alpha=0.05 (which is a pretty standard value).
        - This is the probability of rejecting the null hypothesis when it is actually true (type 1 error)
        - A type 2 error is when we fail to reject the null hypothesis when it is actually false.
        - If the p value is smaller than alpha, we reject the null hypothesis
    - Recall that the p-value is the probability of an observed or more extreme outcome given that the null hypothesis is true
        - In this case we use the binomial distribution and calculate p(k>=1|n=5, p=0.1)
- Bayesian method
    - Our first hypothesis is that we have 10% yellow
    - Out second hypothesis is that we have 20% yellow
    - Each will be given a prior proabability of 0.5
    - Can use the binomial distribution to calculate likelihoods
    - At this point we'll have all the information needed to calculate the posterior using Bayes' rule
- The frequentist method is highly sensitive to the null hypothesis by the Bayesian method is independent of the order of each hypothesis
