---
layout: post
title:  "Two-way ANOVA and Tukey test for non-additivity without replications"
categories: Statistics
tags: statistics regression anova causal-inference
author: ZijunZhang_
academia: true
mathjax: true
---


## Two way ANOVA
Cell-means version of the model
$$y_ijk = \mu_ij + \epsilon_ijk, \epsilon_ijk \sim N(0, \sigm^2)$$

factor-level version
$$y_ijk = \u + \alpha_i + \beta_j + (\alpha\beta)_{ij} + \epsilon_ijk$$
where (these constraints provide the desired model identifiability and degree of freedoms)
\sum_{i=1}^{a} \alpha_i =0, 
\sum_{j=1}^{b} \beta_j =0,
\sum_i=1^a (\alpha\beta)_{ij} = \sum_i=j^b (\alpha\beta)_{ij} = 0


Analysis of variance based on parition of total sum of squared deviations from grand mean.

\sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} (y_{ijk} - \bar{y}_{...})^2 = 
\sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} (y_{ijk}) - \bar{y}_{ij.})^2 + \sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} ( \bar{y}_{ij.} - \bar{y}_{...} )^2

[INSERT EQU TO COMPUTE SS. HERE]

[INSERT two-way ANOVA TABLE HERE WITH EXPECTATION]

When there are no replications, assuming interaction terms in two-way ANOVA will eat the error sum of squares, hence is 
statistically inhibitive. To fix this, need to treat interaction term as error term and assume no interaction effects (pool
interaction/error).

If assumed wrong model (i.e. use a non-interactive model when there is interaction effects), it would tend to yield
"conservative" tests for A,B main effects since expectation of denominator of F-test is potentially greater than $\sigma^2$.


## Tukey test for non-additivity

[HANDOUT]

In essense Tukey test for non-additivity is testing for a multiplicative interaction term when there are no replications.

Below is how a two-way ANOVA table will look like for testing interactions without replications:

Source	df 	SS 	MS 	F
A 	a-1	SSA	MSA	F_A = MSA/MS_{Rem}
B 	b-1 SSB MSB F_B = MSB/MS_{Rem}
AB  (a-1)(b-1) SSAB  MSAB [Not doable, use Tukey partition] 
    Non-addiviity  1   SSAB*   MSAB*=SSAB*  F_{AB} = MSAB*/MS_{Rem} --> F_{1, ab-a-b}
    Remainder  ab - a -b  SSRem*  MSRem*
----
Total  ab-1



## Half-normal distribution
Daniel, 1959 Technometrics

If $Z \sim N(0,1)$, then $|Z|$ follows a  "half-normal" distribution, often denoted as $\chi$ (square root of $\chi^2$).

Suppose we break down all effects into orthonormal 1-df. contrasts. [same length-important so that contrasts have same variance]

Take absolute values of each $\hat{L}=c^T \cdot \bar{Y}$, order from smallest to largest. 

As a side note, using the Half-normal here is because without replication, we cannot estimate the standard error $SE^2(\hat{L}) = MSE \sum_{i=1}{r} \frac{c_i^2}{n_i}$, where MSE cannot be computed.

Plot $|contrasts|$ against expected order statistics from $\chi$.

Carry at test of largest |contrasts|.

Remove largest, repeat process.

## ANOVA without replication
Three strategies:
- Assume no interaction effects and treat interaction as error terms
- Tukey test of non-additivity; parametrize certain type of interactions (i.e. multiplicative combinations of main effects)
- Half-normal plot


