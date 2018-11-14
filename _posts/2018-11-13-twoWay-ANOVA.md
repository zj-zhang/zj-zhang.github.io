---
layout: post
title:  "Two-way ANOVA and Tukey test for non-additivity without replications"
categories: Statistics
tags: statistics regression anova causal-inference
author: ZijunZhang_
academia: true
mathjax: true
---

* content
{:toc}

In this post, we will generalize over one-way ANOVA to two-way and multi-way ANOVA, and discuss a few solutions to test for interaction terms when there is no replication.



## Two-way ANOVA definition
Cell-means version of the model:
$$y_{ijk} = \mu_{ij} + \epsilon_{ijk}, \epsilon_{ijk} \sim N(0, \sigma^2)$$

Factor-level version:
$$y_{ijk} = \mu + \alpha_i + \beta_j + (\alpha\beta)_{ij} + \epsilon_{ijk}$$

where (these constraints provide the desired model identifiability and degree of freedoms)

$$\sum_{i=1}^{a} \alpha_i =0$$,

$$\sum_{j=1}^{b} \beta_j =0 $$,

$$\sum_{i=1}^a (\alpha\beta)_{ij} = \sum_{j=1}^b (\alpha\beta)_{ij} = 0 $$.


Analysis of variance based on parition of total sum of squared deviations from grand mean.

$$\sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} (y_{ijk} - \bar{y}_{...})^2 = $$
$$\sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} (y_{ijk} - \bar{y}_{ij.})^2 + \sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} ( \bar{y}_{ij.} - \bar{y}_{...} )^2$$

The above equation can be seen as 1) $SS_{residual}$, or within-group variance; and 2) $SS_{treatment}$, or between-group variance. The Between-group variance can be further partitioned as:


$$ SS_{treatment} = \sum_{i=1}^{a} \sum_{j=1}^{b} \sum_{k=1}^{n} ( [\bar{y}_{i..} - \bar{y}_{...}] + [\bar{y}_{.j.} - \bar{y}_{...}] + [\bar{y}_{ij.} - \bar{y}_{i..} - \bar{y}_{.j.} + \bar{y}_{...} )^2 $$
$$= SSA + SSB + SSAB $$

that is, the main effect os A/estimate of $\alpha_i$ (SSA), the main effect os B/estimate of $\beta_j$ (SSB), and the A.B interaction estimate $(\alpha\beta)_{ij}$ (SSAB).

## Two-way ANOVA table

| Source	| df 	| SS 	| MS 	| E(MS) | F  |
| -----		| -----	|-----	| -----	| ----- |-----|
|A 	| a-1	| SSA	| MSA	| $\sigma^2 + \frac{bn\sum_i(\mu_{i..}-\mu{...})}{(a-1)}$ | $F_A = \frac{MSA}{MSE}$ |
|B 	| b-1 	| SSB  | MSB 	| $\sigma^2 + \frac{an\sum_j(\mu_{.j.}-\mu{...})}{(b-1)}$ | $F_B = \frac{MSB}{MSE}$ |
|AB | (a-1)(b-1) | SSAB | MSAB | $\sigma^2 + \frac{n\sum_i\sum_j(\mu_{ij.}-\mu{...})}{(a-1)(b-1)}$ | $F_{AB} = \frac{MSAB}{MSE}$ |
|Error| ab(n-1) | SSE | MSE | $\sigma^2$ |  |
| -----		| -----	|-----	| -----	| -----|
| **Total**  | **abn-1** |  


E(MS) is the expected mean, or non-centrality parameter for the F-test.

When there are no replications, assuming interaction terms in two-way ANOVA will eat the error sum of squares, hence is 
statistically inhibitive. To fix this, need to treat interaction term as error term and assume no interaction effects (pool
interaction/error).

If assumed wrong model (i.e. use a non-interactive model when there is interaction effects), it would tend to yield
"conservative" tests for A,B main effects since expectation of denominator of F-test is potentially greater than $\sigma^2$.


## Tukey test for non-additivity

In essense Tukey test for non-additivity is testing for a multiplicative interaction term when there are no replications, where $(\alpha\beta)_{ij}=D\cdot\alpha_i\cdot\beta_j$, $D$ is some constant. The hypothesis testing is $H_1: D\ne0$ vs $H_0:D=0$. 

Below is how a two-way ANOVA table will look like for testing interactions without replications:


| Source	| df 	| SS 	| MS 	| F  |
| -----		| -----	|-----	| -----	| -----|
|A 	| a-1	| SSA	| MSA	| $F_A = \frac{MSA}{MS_{Rem}}$ |
|B 	| b-1 	| SSB   | MSB 	| $F_B = \frac{MSB}{MS_{Rem}}$ |
|AB | (a-1)(b-1) | SSAB | MSAB | [Not doable due to 0-df for error term, use Tukey partition] |
| Non-addiviity|  1  | SSAB* |  MSAB\*=SSAB\* | $F_{AB} = \frac{MSAB^*}{MS_{Rem}} \sim F_{1, ab-a-b}$ |
|    Remainder | ab - a -b | SSRem | $MS_{Rem}$ |   |
| -----		| -----	|-----	| -----	| -----|
| **Total**  | **ab-1** |  



## Half-normal distribution
Daniel, 1959 Technometrics

If $Z \sim N(0,1)$, then $\|Z\|$ follows a  "half-normal" distribution, often denoted as $\chi$ (square root of $\chi^2$).

Suppose we break down all effects into orthonormal 1-df. contrasts. [same length-important so that contrasts have same variance]. The following procedure is used to test interaction terms using Half-normal distribution:

1. Take absolute values of each $\hat{L}=c^T \cdot \bar{Y}$, (i.e. contrasts ) order from smallest to largest. 

As a side note, using the Half-normal here is because without replication, we cannot estimate the standard error $SE^2(\hat{L}) = MSE \sum_{i=1}{r} \frac{c_i^2}{n_i}$, where MSE cannot be computed.

2. Plot $\|contrasts\|$ (i.e. $\hat{L}$) against expected order statistics from $\chi$.

3. Carry at test of largest \|contrasts\|.

4. Remove largest, repeat process 2-3.

## ANOVA without replication
Three strategies:
- Assume no interaction effects and treat interaction as error terms
- Tukey test of non-additivity; parametrize certain type of interactions (i.e. multiplicative combinations of main effects)
- Half-normal plot


