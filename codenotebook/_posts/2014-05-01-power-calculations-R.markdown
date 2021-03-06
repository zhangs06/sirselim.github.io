---
layout: post
category: codenotebook
title:  "Power calculations in R"
date:   2014-05-01
categories: R codenotebook power-calculations 
---

Power calculations are an essential element of good experimental design. They can inform on appropriate sample sizes to detect effects of a given size with varying degrees of confidence, or even provide information on the degree of confidence one will have within the confines of a small sample size and the maximum effect sizes one would expect to detect.

It may just be me, but there always seems to be something mystically 'scary' about power calculations, peoples perception of them, and how to go about them (until recently I'm not ashamed to have included myself in all 3!). It turns out that it's not that hard! There are a range of tools out there for calculating power, and I've recently come across another - **pwr** - a nice little package for R.

The *pwr* package has a good set of functions for calculating power across a range of experimental types, from ANOVA and t-tests to proportions and correlations.

    library('pwr')

The basics behind power calculation revolve around 4 fundamental properties:

  - effect size (*h* or *d* in pwr package functions)
  - sample size (*n*)
  - significance level or P(Type I error) = probability of finding an effect that is not there
  - power or 1 - P(Type II error) = probability of finding an effect that is there 

As long as we have 3 of the above, the 4th parameter can be calculated.

Here's an example experimental design that a good colleague of mine wanted to test the power of:

> We have an experiment where we are proposing to sequence 27 miRNAs in 60 individuals (30 case and 30 control). What power would we have to detect a fold change of 1.5 or greater?

So firstly we define our parameters:

  1. sample size = 30
  2. effect size = 1.5
  3. significance level: here we are going to apply a 'rough' multiple testing (0.05/27) = 0.002
  4. power = **?**

Now we can plug this into R.  For this example we will use the `pwr.t.test` function.  The 'two.side' argument is provided as the effect direction is not known.
    
    pwr.t.test(n = 30, d = 1.5, sig.level = 0.002, alternative = "two.sided")
    
    Two-sample t test power calculation 
    
    n = 30
    d = 1.5
    sig.level = 0.002
    power = 0.9933512
    alternative = two.sided

We have a power of 0.9933512, or 99% chance of detecting a 1.5 fold or greater effect in this study design. Pretty good!

Of course we might have a similar experiment, but we want to be able to detect very small changes with a high chance of success. Say we want to detect an effect size of 0.5 with a 95% power, what number of samples would be required for this?

    pwr.t.test(d = 0.5, sig.level = 0.002, power = 0.95, alternative = "two.sided")
    
    Two-sample t test power calculation 
    
    n = 181.7655
    d = 0.5
    sig.level = 0.002
    power = 0.95
    alternative = two.sided

By slightly rearranging the same test we can see that to achieve this type of effect size detection with high confidence we'd need a sample size of 182 in each group.

I found this R package to be really useful in my understanding and I will be using it for teaching others in the future. One point I would like to follow up on is the utility of this package for power calculations in high-throughput molecular genetic experiments such as microarrays and next-gen sequencing. I know that there are various custom power calculating tools out there for these types of experiments, so it might be time to explore how they differ (if they differ...?) from the functionality available in the *pwr* package. 