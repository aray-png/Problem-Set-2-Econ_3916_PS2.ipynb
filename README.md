# Abigail Ray || ECON 3916 PS2: The Survey That Wasn't Random

## Overview
This project studies how nonresponse can bias survey results. Using a simulated user population, I examine how satisfaction affects response rates, confidence intervals, statistical significance, and power analysis.

## Objectives
- Simulate before-and-after satisfaction scores.
- Measure bias caused by selective survey responses.
- Calculate bootstrap confidence intervals and test their coverage.
- Compare survey means using a Welch two-sample t-test.
- Calculate statistical power and test whether larger samples reduce bias.
- Explore inverse-probability weighting as a potential correction.

## Step 1: Simulating the Survey

### Step 1.1 — Simulate the Population
I created simulated before-and-after populations using satisfaction scores, engagement minutes, and support-ticket history.

- True mean satisfaction before: 3.291
- True mean satisfaction after: 3.391
- True population improvement: 0.100 points

### Step 1.2 — Model Nonresponse
The probability of responding depends on satisfaction, engagement, and support-ticket history.

- Before response rate: 27.5%
- After response rate: 15.3%
- Observed mean before: 3.592
- Observed mean after: 4.309
- Observed improvement: 0.717 points

The observed improvement is much larger than the true improvement because more satisfied users are more likely to respond. This creates selection bias.

## Step 2: Confidence Intervals and Coverage

### Step 2.1 — Bootstrap Confidence Intervals
I calculated confidence intervals by hand and using SciPy.

- Mean percentile CI: [4.295, 4.324]
- Mean BCa CI: [4.295, 4.324]
- Median percentile CI: [4, 4]
- Median BCa CI: NaN

The by-hand and SciPy percentile intervals agree. The mean intervals are nearly identical. The median bootstrap distribution is degenerate because the median remains 4 across resamples, making the BCa correction undefined.

These intervals describe respondents and do not correct nonresponse bias.

### Step 2.2 — Coverage Simulation
I ran 200 repeated surveys with 500 bootstrap resamples per survey.

- Percentile coverage: 0.000
- Percentile Monte Carlo SE: 0.000
- BCa coverage: 0.000
- BCa Monte Carlo SE: 0.000

Neither method's intervals contained the true population mean of 3.391 in any of the 200 simulations. Both methods performed similarly. The intervals miss the truth because the respondents are not representative of the population.

## Step 3: Hypothesis Testing and Power

### Step 3.1 — Welch Two-Sample t-Test
I compared the observed before-and-after respondent means using a Welch two-sample t-test.

- Before mean: 3.592
- After mean: 4.309
- Observed difference: 0.717 points
- Cohen's d: 0.879
- Welch t-statistic: 66.331
- P-value: less than 0.001

The difference is statistically significant, but the observed improvement is much larger than the true population improvement of 0.100 points. Statistical significance does not eliminate selection bias or prove that the population improved by the observed amount.

### Step 3.2 — Power Analysis
I calculated the sample size needed to detect a true improvement of 0.10 points.

- Planning standard deviation: 0.887
- Standardized effect size: 0.113
- Required sample per group: 1,237
- Actual before respondents: 13,759
- Actual after respondents: 7,638
- Detectable effect at current sample sizes: approximately 0.035 points

I simulated repeated surveys with sample sizes from 100 to 2,000 respondents per group.

| Sample per group | Mean estimated gap | Bias vs. truth | RMSE |
|---:|---:|---:|---:|
| 100 | 0.720 | 0.621 | 0.631 |
| 250 | 0.717 | 0.617 | 0.621 |
| 500 | 0.716 | 0.617 | 0.619 |
| 1,000 | 0.717 | 0.617 | 0.618 |
| 2,000 | 0.717 | 0.617 | 0.617 |

The simulation shows that larger samples reduce random error but do not eliminate selection bias. The estimated improvement stays near 0.717 points instead of the true 0.100 points.

## Phase 4: AI Expansion — Inverse-Probability Weighting

I explored inverse-probability weighting (IPW) to adjust for differences between respondents and the full population.

The covariate-only model estimates response probabilities using engagement and support-ticket history. Because the model does not include satisfaction, which also affects response, substantial bias remains.

A simulation-only benchmark using the known response probabilities produces an estimate closer to the true population mean. This comparison shows how leaving an important predictor of response out of the weighting model can limit its effectiveness.

The benchmark uses information known from the simulation and would not generally be available in a real survey.

## Conclusion
The analysis shows that nonresponse can create large and misleading differences in survey results. The post-redesign respondent mean increased by 0.717 points, while the true population improvement was only 0.100 points.

Bootstrap intervals, hypothesis tests, and power calculations do not fix systematic selection bias. Increasing sample size alone is not enough. Better survey methods should address nonresponse directly, such as following up with a random sample of initial nonrespondents and collecting their satisfaction scores.

## Limitations
- The data and response mechanism are simulated.
- Confidence intervals calculated from respondents do not automatically represent the full population.
- Results depend on the assumptions used to model response probabilities.
- The simulation-only benchmark relies on information that may be unavailable in real-world surveys.

## Reproducibility
The notebook uses Python, NumPy, pandas, SciPy, and statsmodels. Random seeds are set to make the simulation results reproducible.

Run the notebook cells in order to generate the simulated data, bootstrap intervals, coverage estimates, hypothesis tests, power analysis, and weighting results.
