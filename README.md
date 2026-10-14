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
- **BCa interval:** [$
