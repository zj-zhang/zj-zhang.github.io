---
layout: post
title:  "A quick Intro to one-way ANOVA"
categories: Statistics
tags: programming python tensorflow
author: ZijunZhang_
academia: true
mathjax: true
---



ANOVA is parametrized by a regression framework.
$$Y = X \cdot \beta + \epsilon$$



* content
{:toc}

ANOVA is based on

$\sum_{i=1}^{r} \sum_{j=1}^{n_i} (y_{ij} - \bar{y})$

$ = \sum_{i=1}^{r}\sum_{j=1}^{n_i} (y_{ij} - \bar{y_i})^2 + \sum_{i=1}^{r}\sum_{j=1}^{n_i}(\bar{y_i} - \bar{y})^2 + 2 \sum_{i=1}^{r}\sum_{j=1}^{n} (y_ij - \bar{y_i})(\bar{y_i} - \bar{y} )$

Total sum of sq. = within-group + between-group + 0  (marginalize out $\bar{y_i}-\bar{y}$ and the remaining is zero by construction.)


## Display results in ANOVA table

|Source	|df  |SS  |MS  |F|
|------  |---| --- | --- | ---|
|Between |r-1  |$SS_{betw}$ | $MS_{betw}$  |F=$\frac{MS_{betw}}{MS_{within}}$|
|Within  |r(n-1)| $SS_{within}$ |$MS_{within}$ |<-- (error term) |
|------ | --- |
|Total | r$\cdot$n - 1|


F-test can be used to test:

$H_0: \mu_1 = \mu_2 = ... = \mu_r$

$H_1$: Not all $\mu_i$ are equal

If $H_1$, find which levels are the "driving" the significance?


## Inference for comparison

$D = \mu_i - \mu_{i'}, i \neq i'$

$\hat{D} = \bar{Y_{i\cdot}} - \bar{Y_{i'\cdot}}$

$Var(\hat{D}) = \frac{\sigma^2}{n_i} + \frac{\sigma^2}{n_{i'}} = \sigma^2 (\frac{1}{n_i} + \frac{1}{n_{i'}})$

The above is estimated by:

$SE^2(\hat{D}) = MSE(\frac{1}{n_i} + \frac{1}{n_{i'}})$

T-Test based on $T = \frac{\hat{D}}{SE(\hat{D})} \sim t_{df_{error}}$

## An alternative parametrization: Contrast coefficients

X = ([1, 0, 0,], [0,1,0], [0,0,1]) ==> X* = (([1, 1, 1,], [0,1,0], [0,0,1]))

$Y = X\beta ==> Y = X* \beta*$

then $\beta_1 = \mu_1, \beta_2 = \mu_2, \beta_3 = \mu_3$

In the alternative parametrization:

$\beta_1^{\*} = \mu_1, \beta_2^{\*} = \mu_2 - \mu_1, \beta_3^{\*} = \mu_3 - \mu_1$


## Inference on Contrasts
A contrast is a linear combination of group means where coefficients of linear combination sum to 0.

$L = \sum_{i}^{r} c_i \mu_i$, where $\sum_{i=1}^r c_i = 0$

Note: c=(1,1,-2) and c=(1/2, 1/2, -1) are effectively the same because they are testing
$H0: \mu_1 + \mu_2 = \mu_3$

Estimate L by

$\hat{L} = c^T \bar{Y_{i\cdot}} = c_1 \bar{Y_{1 \cdot}} + c_2 \bar{Y_{2 \cdot}} + .. + c_r \bar{Y_{r \cdot}}$

$Var(\hat{L}) = c_1^2 \cdot \frac{\sigma^2}{n_1} + c_2^2 \cdot \frac{\sigma^2}{n_2} +...
= \sigma^2 \sum_{i=1}^{r} \frac{c_i^2}{n_i}$

Estimate $Var(\hat{L})$ by

$SE^2(\hat{L}) = MSE \sum_{i=1}^{r} \frac{c_i^2}{n_i}$

Test $H_0: c^T \mu =0$, $H_1: c^T \mu \neq 0$

using $T = \frac{\hat{L}}{SE(\hat{L})} \sim t_{df_{error}}$

## Orthogonal contrasts
Two contrasts are orthogonal if 

$$c_1^T c_2 = 0$$


With r treatments, can define (r-1) mutually orthogonal contrasts 
(not a unique set of contrasts - there are different ways to achieve).

Example: consider y=measure of drug outcome; 5 potential treatments
1. Placebo
2. drug A 1
3. drug A 2
4. drug B 1
5. drug B 2
Suppose F-test rejects H_0.

All possilbe constrats:
 - c1 = (0, -1, 1, 0, 0)
 - c2 = (0,  0, 0, -1, 1)
 - c3 = (-4, 1, 1, 1, 1)
 - c4 = (0, 1, 1, -1, -1) // c4* = (0, 1, -1, 1, -1)

We say contrasts are _*orthonormal*_ when they are mutually orthogonal and have length 1.


