## Week 3 - Decision Making

- This module discusses:
    - Loss functions
    - Making optimal decisions
    - Bayes factors

### Losses and Decision Making

- Different __loss functions__ correspond to different best __"point" estimates__
    - linear = mean best estimate
    - quadratic = mean best estimate
    - binary (1/0) = mode best estimate

- Consider you want to know how many cars will be sold at your dealership in a month. Given that there were 51 months of sales values in the dataset, we can calculate the value of various loss functions. We'll guess that g=30 cars were sold.
    - binary L0 (1/0) loss: add 1 for each sample != 30. Only 1 month had 30 cars sold so L0=50
    - linear loss L1: add the absolute values of the sample - 30. In this case we get L1=346
    - quadratic loss L2: add the square values of the sample - 30. In this case we get L2=3732

- Consider we have two competing hypotheses H1 and H2. We define P(H1 is true|data)=posterior of H1, P(H2 is true|data)=posterior of H2. We'll consider this along with a loss function. The example is HIV testing
    - H1=patient doesn't have HIV, H2=patient does have HIV
    - Define the loss function L(d) when decision d in made. Bayesian testing will then minimize L(d)
    - We only have two possible decisions d1 and d2 (has HIV, doesnt have HIV)
    - A correct decision has loss of 0 otherwise its some value w1 or w2 (the can be different, and in this case they should be!)
    - In this case we set w1=1000 (d1=wrong) and w2=10(d2=wrong)
    - The error is calculated as seen below where we have P(H1|+ive result)=0.88, P(H2|-ive result)=0.12
    - E[L(d1)] = p_no_HIV x L(d1=correct) + p_has_HIV x L(d1=wrong) = 0.88 x 0 + 0.12 x 1000
    - E[L(d2)] = p_has_HIV x L(d2=correct) + p_no_HIV x L(d2=wrong) = 0.12 x 0 + 0.88 x 10

- Bayes factor:
    - Prior odds is the ratio of prior psobabilities for each hypothesis: O[H1:H2]=P(H1)/P(H2)
    - Posteror odds is similar: PO[H1:H2] = P(H1|data)/P(H2|data) = __P(data|H1)/P(data|H2) + P(H1)/P(H2)__
    - The first term is the __bayes factor__ and the second term is the __prior odds__
    - The bayes factor quantifies the evidence of data arising from H1 and H2
    - For continuous distributions it's an integral: BF[H1:H2] = integral(P(data|theta,H1) / integral(P(data|theta,H2)) where theta are the model parameters (features??)

- Returning to the HIV test example:
    - H1 = patient does not have HIV, H2 = patient has HIV
    - Given the priors P(H1) = 0.998, P(H2) = 0.001 we get O[H1:H2] = 675
    - Given the posteriors P(H1|+ive prediction) = 0.87, P(H2|+ive prediction) = 0.13 we get PO[H1:H2] = 7.25
    - We can calculate the bayes factor as BF[H1:H2] = PO[H1:H2]/O[H1:H2] = 0.01 (using bayes rule)
    - We can also divide the __false negative rate__ by the __true positive rate__ to get the same result: P(+ive|H1)/P(+ive|H2) = 0.01

- Interpreting the bayes factor (Jeffreys 1961):
    - BF[H1:H2] = 1-3: little evidence for H1
    - BF[H1:H2] = 3-30: positive evidence for H1
    - BF[H1:H2] = 30-150: strong evidence for H1
    - BF[H1:H2] = >150: very strong evidence for H1
    - For the HIV example we look at BF[H2:H1] and see the evidence that H2 is correct (i.e. the patient has HIV). We calcualte BF=92 indicating strong evidence for the patient having HIV.

- Another interpretation uses the log scale (Kass & Raftery 1995)
    - 2*log(BF) = 0-2 little
    - 2*log(BF) = 2-6 strong
    - 2*log(BF) = 6-10 strong
    - 2*log(BF) = >10 very strong
    - For the HIV example we get 9 on this scale.

### Comparing two proportions using Bayes Factors

- Assumptions for inference about probabilities
    - Independence within and between groups (i.e. for each sample)
    - Common response rates betwen and within groups (e.g. if a group largely refused to answer than we have a problem! our data does not actually represent the population)
    - Unlike the frequentist approach we do not need to check sample sizes or skewness

- Consider a bullying problem where male and female parents report bullying of their children
    - H1 = an equal number of male and female parents report bullying
    - H2 = an un equal number of ...
    - We'll use a beta prior p_male ~ Beta(a, b) assuming the data comes from a binomial distribution
    - am = number of males reporting bullying
    - bm = number of males not reporting bullying
    - The posterior can then be updated as: (p_male|data) ~ Beta(a+am, b+bm)
    - For the prior probability under H2 (p_male!=p_female):
        - We'll take p_male ~ Beta(0.5, 0.5) and p_female ~ Beta(0.5, 0.5)
        - Using small a and b is recommended when the amount of data is very small
    - For the prior probability under H1 (p_male=p_female):
        - We can __pool the prior__ beliefs from the alternative hypothesis H2 to determine this one
        - Via pooling we get p_male ~ Beta(am+af, bm+bf) ~ Beta(1, 1) and the same for females
    - We can now find the posterior probability of H1 (i.e. the probability that H1 is correct given the data) by calculating p(H1|data) = p(data|H1)*p(H1) / (p(data|H1)p(H1) + p(data|H2)p(H2))
    - It can be shown that p(H1|data) = PO[H1:H2] / (PO[H1:H2] + 1) where PO is the posterior odds -> p(data|H1)p(H1) / p(data|H2)p(H2)

- Comparing two means using Bayes factors with an example of zinc in drinking water. Looking specifically at the difference in concentration of zinc at the bottom compared to the surface.
    - H1 = no differences in mean concentrations at the surface and bottom
    - H2 = there is a difference in ...
        - H3 = more at the bottom
        - H4 = more at the surface
    - We'll assume the data comes from a normal distribution and thus use the Gamma conjugate prior
    - We find P(h1|data)=0.01 and P(H2|data)=0.98. These can be interpreted as probabilities of each hypothesis being correct
    - Say we want to report a point estimate for the mean of the difference in zinc content:
        - The posterior mean for mu_diff under H2 is n/(n+n0)*D where n=sample size, D=sample mean, and n0=prior precision (i dont know what the fuck that means)
            - This comes out to 10/(10+1) * 0.08 = 0.07 for our example
        - The unconditional posterior mean can be calculated as a weighted average:
            - P(h1|data) * 0 + P(H2|data) * n/(n+n0) * D = 0.07
    - To provide a level of certainty we turn to the posterior distribution and look at the credible interval between e.g. 0.025 and 0.975. The more accurate the hypothesis, the tighter this interval should be

- Can use a "normal quantile plot" to show that small amounts of data are roughly normally distributed (or otherwise)
 
- Consider an example where the subject claims to be able to make a subject mean differ from 0.5 (using their brain) when in fact its a computer generating random numbers
    - H1 is that mu=0.5
    - H2 is that mu!=0.5
    - APPARENTLY the frequentist would reject H1 because of a very small p-value, but the Bayes factor is ~15 suggesting H1 is very probable
    - We use a conjugate normal prior
    - Lindley's paradox can occur when the sample size is large, while Bartlett's paradox can occur when the specification is too vague. These paradoxes refer to a difference in predictions from frequentist and bayesian approaches
    - The Cauchy prior is like the normal but has a larger tail. It can help resolve the paradoxes above

- Consider an example of the effect of distraction on snacking
    - One group ate lunch while playing games, the other ate without distraction
    - The study reported average snack consumption for each group, what group snacked more??
    - We'll use the independent Jeffrey's prior which results in a Students-t distribution for the posterior. This distribution is like the normal but good for small sample sizes
    - We can generate a probability distribution for the difference in means and find the average of this along with the 95% credible interval. One way of finding this is to generate samples from the posterior distibutions for mu_a and mu_b using monte carlo sampling
    - H0= equal means (same snack consumption)
    - H1= means are unequal (snack consumption is different)
    - If working under the assmption of H1, we'll specify each prior probability as a normal with the SAME MEAN mu, on the other hand if working under H2 we'll assign these probabilities DIFFERENT MEAN mu

