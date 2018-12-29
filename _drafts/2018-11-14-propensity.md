

Rosenbaum & Rubin (R&R) 1983 Biometrika

Theory: Propensity score $\pi(Z=1|X)$ is a balancing score. At values of balancing scores, 
comparison of exposed vs. unexposed means(averages) provides unbiased estimation of average treatment effect.

Under Rubin Causal model, each unit has causal effect of treatment, which might differ across units.

Without stronger assumptions, can't identify unit-level causal effect. With statistical solution, can estimate
avearge causal effect.

R&R (1983) described three broad strategies for estimation:
- covariance adjustment (using propensity score as covariate in regression)
- matching on propensity score
- subclassfication (analog of Cochran 1968 Biometrics). Propensity scores satisfies theory for one X variable.

A fourth use of propensity scores: inverse-probability weighting. Analog to the survey sampling literature,
where a general strategy for unbiased estimation in unequal probability samples is to weight observations
inversely with probability of selection. ( in Little and Rubin, Stat. Analysis with Missing Data, chapter on
weighting methods discusses weighing inversely proportional to prob. of selection and response)


[SURGICAL VS. MEDICAL EXAMPLE]

"Double robustness" idea: obtain valid inferences if either propensity model correct or data model correct.

