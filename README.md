# Estimation & Bootstrapping — Quantifying Uncertainty Without a Formula

## Objective

I explored how to measure uncertainty in statistical estimates, compare confidence intervals, and use bootstrap resampling to estimate uncertainty for statistics without a simple standard-error formula.

## Methodology

- Loaded the UCI Adult Census Income dataset containing 32,561 respondents.
- Compared the standard deviation of individual ages with the standard error of the estimated mean.
- Examined how standard error changes with sample sizes of 25, 100, 400, and 1,600.
- Constructed a parametric 95% confidence interval for mean age using the central limit theorem (CLT).
- Compared the distributions of age and positive capital gains to examine how skewness affects the CLT.
- Bootstrapped the median age and median positive capital gain to investigate when bootstrap intervals collapse.
- Calculated a bootstrap confidence interval for mean positive capital gains using a manual loop.
- Compared percentile and bias-corrected and accelerated (BCa) bootstrap intervals using `scipy.stats.bootstrap`.
- Estimated the difference in mean weekly working hours between men and women using a two-sample bootstrap.
- Used AI to write a reusable bootstrap confidence-interval function and checked its output against my manual calculation and SciPy.
- Simulated confidence-interval coverage for the mean and median of lognormal data using 400 experiments with sample sizes of 50.
- Estimated the 90th-to-10th percentile ratio of positive capital gains and calculated its BCa confidence interval.

## Key Findings

### Standard Deviation and Standard Error

The standard deviation of age was 13.640 years, while the empirical standard error of the mean for samples of 100 was 1.361 years. The formula estimate was 1.364 years. Increasing the sample size reduced the standard error, demonstrating that larger samples generally produce more precise estimates.

### Parametric Confidence Intervals

The sample mean age was 38.34 years, with a parametric 95% confidence interval of [36.49, 40.18]. The true population mean was 38.58 years. The CLT approximation worked more quickly for age than for the highly right-skewed positive capital gains data.

### Bootstrap Results

The bootstrap median age was 37.0 years, and its bootstrap standard error was 0.0000. Every resample returned the same median because repeated whole-number ages pinned the median at 37.0. This demonstrated that a collapsed bootstrap interval can reflect the structure of the observed data rather than a coding error.

For positive capital gains, the mean was $12,939 and the median was $7,298. The mean was substantially higher because of the long right tail.

The manual bootstrap produced a standard error of $424 and a 95% percentile interval of [$12,114, $13,780].

SciPy produced the following intervals:

- **Percentile interval:** [$12,173, $13,796]
- **BCa interval:** [$12,180, $13,809]

The manual and SciPy percentile intervals differed because they used different bootstrap resamples. When I supplied `seed=42` to my revised AI function, its interval matched SciPy's percentile interval. This showed the importance of reproducibility when checking statistical code.

### Difference in Weekly Working Hours

Men reported an average of 42.43 hours per week, while women reported an average of 36.41 hours. The observed difference was 6.02 hours, with a 95% BCa confidence interval of [5.73, 6.28] hours.

The interval excluded zero, indicating an association in this dataset. However, this does not establish causation, and the 1994 data may not represent today's workforce.

### Confidence-Interval Coverage

In a simulation using lognormal data, the nominal 95% intervals achieved the following coverage rates:

| Target | Method | Coverage |
|---|---|---:|
| Mean | Parametric | 92.5% |
| Mean | Percentile bootstrap | 91.5% |
| Median | Parametric | 66.8% |
| Median | Percentile bootstrap | 93.2% |

For the mean, the 1.0-percentage-point difference between methods was within the Monte Carlo standard error of approximately 1.3 percentage points. The methods performed similarly in this simulation.

For the median, the mean-centred parametric interval performed poorly, covering only 66.8% of experiments. The bootstrap interval covered 93.2%, much closer to the nominal 95% level.

### Inequality Measure

The 90th-to-10th percentile ratio of positive capital gains was 6.45, with a 95% BCa confidence interval of [6.24, 8.37].

The interval was wide and asymmetric, illustrating the uncertainty involved in estimating a statistic influenced by extreme values. This result should be interpreted cautiously because extreme quantiles can be sensitive to the observations in the tails.

## Reflection

This lab helped me understand the difference between variation in individual observations and uncertainty in an estimate. I learned how to construct and interpret confidence intervals, use bootstrap resampling for statistics without a simple standard-error formula, and recognize when the bootstrap has limitations.

I also learned that different bootstrap runs can produce slightly different intervals because they use different random samples. Setting a seed makes results reproducible, while checking AI-generated code against known results helps determine whether the implementation is working correctly. Overall, the lab showed me why understanding the statistical method is just as important as writing the code.
