---
layout: post
title:  "A quick Intro to one-way ANOVA"
categories: Statistics
tags: statistics regression anova causal-inference
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

$Y = X\beta ==> Y = X^{\*} \beta^\*$

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

Suppose F-test rejects $H_0$.

All possilbe contrasts:
 - c1 = (0, -1, 1, 0, 0)
 - c2 = (0,  0, 0, -1, 1)
 - c3 = (-4, 1, 1, 1, 1)
 - c4 = (0, 1, 1, -1, -1) // c4* = (0, 1, -1, 1, -1)

We say contrasts are _*orthonormal*_ when they are mutually orthogonal and have length 1.


## Orthogonal polynomial contrasts

Suppose have experimental factor with continuous underlying scale (say, age groups).
Suppose ANOVA F-test rejects H_0, what next?

Idea when there is underlying continiuum: orthogonal olynomial contrasts.

For 4 groups:
- Linear: c1 = (-3, -1, 1, 3)
- Quadratic: c2 = (1, -1, -1, 1)
- Cubic: c3 = (-1, 3, -3, 1)

c1, c2, c3 mutually orthogonal. The names come from fact that if group means are correlated with contrast coefficients 
(yielding larger numericator in T = \frac{\hat{L}}{SE(\hat{L})} )


Quadratic is not significant if trend is linear. Quadratic significant when means have U-shape or inverted U-shape,
or curvature where after accounting for linear trend, residuals have U-shape or inverted U-shape (in this case both
linear and quadratic will be significant).

Cubic significant when pattern in means has wavy shape.


How to generate orthogonal polynomial coeffs?
For example, r=4.
- Take numbers, 1, 2, 3, 4; Regress on constant; Save residuals; Residuals = linear contrast coeffs.
  - fitted value: 2.5, Residuals: (-1.5, -0.5, 0.5, 1.5)
- Take numbers, 1^2, 2^2, 3^2, 4^2; Regress on constant; Save residuals; Residuals = quadratic contrast coeffs.
- Take numbers, 1^3, 2^3, 3^3, 4^3; Regress on constant; Save residuals; Residuals = cubic contrast coeffs.


Testing Contrasts are a bit more sensitive than the model-wise F-test.

When use ANOVA on all orthogonal contrasts, that should add up to all explainable sum of squares (between-group ss.)
in the model-wise ANOVA table.


## Factorial design

Consider study of cholesterol levels with 4 drugs and 2 diets.

Proposes desgin:

|----|----|----|----|----|----|
|Group 	|1	|2	| 3|	4|	 5|
|----|----|----|----|----|----|
|Drug 	|P 	|P 	|D1 	|D2 	|D3|
|----|----|----|----|----|----|
|Diet 	|R 	|LF	|R 	|R |	R|
|----|----|----|----|----|----|
|n 	|10	|10	|10	|10	|10|

Factorial desgin:


|		Drug 	|
|:----:|:----:|:----:|:----:|:----:|:----:|
|	|	  | P |  D1 | D2 | D3 |
|Diet     |R |	6	|6	|6	|6|
|	      |L |	6	|6	|6	|6|


ANOVA Tables

Tow-way factorial desgin

|Source	| df  |
|-----	|----|
|group 	|	7|
|error	|	40|
|Total	|	47|


Further, two-way factorial design partitioning treatment SS:

|Source	| df   |  df|
|-----	----   | ----|
|drug 	|	3  | (a-1)|
|diet 	|   1   | (b-1)|
|drug x diet 	|	3  | (a-1)(b-1)|
|error	|	40  | ab(n-1)|
|----	| ----- | ----- |
|Total	| 47	| abn - 1 |

Factorial designs more effectively use units. Can get the same main effects with fewer units than studying
separately, plus gain the insights into interactions.

Half replication (example of "fractional factorial design"): List all possible treatment combinations, group them 
by treatment combos with even number of "+" levels and odd number of "+" levels. Each set of the half would be a
possible choice of the half replication.

