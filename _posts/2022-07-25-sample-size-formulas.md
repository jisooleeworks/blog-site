---
layout: post
title: Sample size formulas
---
Although online tools are available to obtain a sample size,  I make a note of formulas for sample size calculation. Being aware of them may help you use online calculators more confidently. It seems that formulas come in different flavors. However, there are more commonly used ones. I will list them up, after check the fundamentals of sample size determination. 

# Factors to consider in sample size determination

Before formulas, let's look through what affect the sample size:
1. It depends on the **size of difference** that you would like to detect. If differences are smaller, a larger sample size is required. That is, you may not be able to detect a small difference (e.g., cannot reject the null hypothesis), if a sample size is not large enough for it. In other words, larger differences can be caught with smaller sample sizes. 
2. It depends on the **standard deviation**. The smaller the standard deviation is, the smaller the sample size it. 
3. It depends on the **size of Type 1 error**. If you are strict on it and thus set it smaller, you need a larger sample size. 
4. It depends on the **size of Type 2 error**. If you set it smaller, a larger sample size is required. In other words,  you are likely to fail to reject the null hypothesis, if your sample size is not large enough for your Type 2 error level. 

In practices, the third and forth factor seems easy to determine. I may follow kind of conventions: e.g., 0.05 or 0.01 for Type 1 error, and 0.1 or 0.2 for Type 2 error. But, the difference and standard deviation are tricky. With regard to the difference, we need to think of what meaningful difference is for a given situation. For instance, "is it worthwhile conducting a study to see if there is 0.1% increase in conversion rate?" With regard to estimation of the standard deviation, it is suggested to refer to existing work, or conduct pilot test, if feasible, and with caution [1][2].

# For the mean

Formulas for sample size calculation have different detail according to study designs. We will first look at formulas for the mean to give you a sense of how they are formed fundamentally.

## Finding a confidence interval
Suppose you target a margin of error that is calculated as follow:

$$
E = Z_{1-\alpha/2}\frac{\sigma}{\sqrt{n}}
$$

By solving this for n using algebra, you get the below formula that results in the sample size needed for the target margin of error:

$$
n = \frac{Z_{1-\alpha/2}^2\sigma^2}{E^2}
$$

## Hypothesis testing
We consider not only Type 1 error, but also Type 2 error. In general, the formula for power computation involving the both is as follow (for the two-tailed test) [3]:

$$
Z_{power} = \frac{\delta}{SE} - Z_{1-\alpha/2}
$$

### One sample test

For the one sample t-test where $$\delta = \lvert\bar{x} - \mu\rvert$$, we plug the below and solve for n with some algebra:

$$SE = \frac{\sigma}{\sqrt{n}}$$

The derived formula is:

$$
n =\frac{(Z_{1-\alpha/2} + Z_{power})^2\sigma^2}{\delta^2}
$$

### Two sample test

For the two sample test where $$\delta = \lvert\bar{x_1} - \bar{x_2}\rvert$$, we plug the below:

$$
SE = \sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{rn_2}},\newline
\text{where r is ratio of group 2 to group 1 size}
$$

The derived formula is [4]:

$$
n_1 = \frac{(r+1)}{r}\frac{\sigma^2(Z_{power}+Z_{\alpha/2})^2}{\delta^2}
$$

If r=1 (equal groups), then 

$$n_1 = \frac{2\sigma^2(Z_{power}+Z_{\alpha/2})^2}{\delta^2}$$


### Paired test

The formula for the paired test is [4]:

$$
n = \frac{\sigma_d^2(Z_{power}+Z_{\alpha/2})^2}{\delta^2},\newline
\text{where}~\sigma_d^2~\text{is the standard deviation of the within-pair difference.}
$$


# For the proportion
Getting a confidence interval,

$$
n = \frac{Z_{1-\alpha/2}^2p(1-p)}{E^2}
$$

One sample test,

$$
n =\frac{(Z_{1-\alpha/2} + Z_{power})^2p(1-p)}{\delta^2}
$$

Two sample test [4],

$$
n_1 = \frac{(r+1)}{r}\frac{(Z_{power}+Z_{\alpha/2})^2\{p_1(1-p_1)+p_2(1-p_2)\}}{\delta^2}
$$

Note:
* All the above with regard to the hypothesis testing assumes two-tailed test; if we do one-tailed test, $$Z_{1-\alpha}$$ used, intead of $$Z_{1-\alpha/2}$$.
* If the standard deviation of the population is unknown and the sample size is small, the formulation depends on the t distribution. It causes a problem because critical values of the t distribution depneds on degrees of freedom, which depends on the sample size which we are trying to estimate. [This document describes how to manage this issue.](https://www.itl.nist.gov/div898/handbook/prc/section2/prc222.htm)

# Adjustment
While we have looked around a collection of formulas and may feel quite ready, we need to be aware of situations where those formulas will not work well, and some adjustment is necessary [5]. 

The frequently found adjustment is use of the finite population correction factor when we draw a sample from a finite small population. It multiplies the obtained sample size with the factor: n_final = factor * n.

$$
n_{finit\_pop} = \frac{n}{1+(\frac{n-1}{N})} = \frac{nN}{n+(N-1)}\newline
\text{where N is the population size.}
$$

# Wrapping up

We have looked around formulas for sample size calculation that are most commonly mentioned. Athough a bit of difference exists in detail according to study designs, they share the same fundamental that was reviewed at the beginning of this post. Considering the fact that there are many other versions, the knowledge covered here may be a quite small fraction of the whole body of work. However, this much knowlege seems enough for us to enter values into sample size calculators being aware of what we are doing.

# Reference:
1. Whitehead AL, Julious SA, Cooper CL, Campbell MJ. Estimating the sample size for a pilot randomised trial to minimise the overall trial sample size for the external pilot and main trial for a continuous outcome variable. Stat Methods Med Res. 2016 Jun; 25(3):1057-73. [(link)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4876429/)
2. Bell ML, Whitehead AL, Julious SA. Guidance for using pilot studies to inform the design of intervention trials with continuous outcomes. Clin Epidemiol. 2018;10:153-157. [(link)](https://www.dovepress.com/guidance-for-using-pilot-studies-to-inform-the-design-of-intervention--peer-reviewed-fulltext-article-CLEP)
3. WISE, Claremont Graduate University. Power Calculations for One-Sample Z Test in Power Tutorial. [(link)](https://wise1.cgu.edu/power/computing.asp)
4. Kristin L. Sainani. Lecture 11 of course hrp259. [(link)](https://web.stanford.edu/~kcobb/hrp259/)
5. Monitoring & Evaluation. Sample Size Calculation. [(link)](http://monitoringevaluation.weebly.com/sample-size-adjustments.html)