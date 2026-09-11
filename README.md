# Ad Exposure vs. Conversion: An Experimentation Analysis

An end-to-end A/B test analysis on a real 588K-row marketing dataset, framed as a product decision: **is it worth showing users a promotional ad experience, or does the lift not justify the cost?**

## Business context

Consumer apps constantly face this question — should we ship a new promotional banner, push notification, or discount surface? This notebook analyzes a real experiment where one group of users (**ad**) saw ad creatives while a holdout group (**psa**) saw a generic Public Service Announcement instead, and walks through the full decision-making process a product/data analyst would use to turn that experiment into a recommendation.

## Questions answered

1. Was the experiment actually powered well enough to trust the result?
2. Did showing ads significantly increase conversion?
3. Does the effect hold consistently across days and times, or is it driven by one segment?
4. Is the lift big enough to be worth the cost of running ads at scale?

## Dataset

- **Source:** [Kaggle – Marketing A/B Testing](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing)
- **Size:** 588,101 users
- **Columns:** `user id`, `test group` (ad/psa), `converted` (bool), `total ads` seen, `most ads day`, `most ads hour`

## Methodology

| Step | What it does |
|---|---|
| **EDA** | Conversion rate and ad-exposure comparison across groups |
| **Power analysis** | Checks whether the (unbalanced 96/4) group sizes could reliably detect the observed effect at 80% power before trusting the result |
| **Hypothesis testing** | Two-proportion z-test for conversion rate; Shapiro–Wilk + Levene's test to choose between a t-test and Mann-Whitney U test for ad-exposure volume |
| **Segment analysis** | Breaks the lift down by day of week and time-of-day bucket to check the effect isn't driven by one slice of traffic |
| **Cost-benefit estimate** | Converts the conversion lift into an estimated incremental revenue figure and weighs it against an assumed ad-serving cost |

## Key findings

- The experiment was well-powered — achieved power at the observed effect size is effectively 1.0, so the result isn't underpowered noise.
- Users shown ads converted at a statistically significantly higher rate than the PSA group (p < 0.001).
- The lift is positive across **every** day of the week and time-of-day bucket, not concentrated in one segment.
- Under illustrative cost/revenue assumptions, the incremental revenue from the lift outweighs the estimated ad-serving cost by a wide margin.

## Tech stack

- Python, pandas, NumPy
- `scipy.stats` (Shapiro–Wilk, Levene's test, Mann-Whitney U)
- `statsmodels` (power analysis, two-proportion z-test)
- Matplotlib, Seaborn

## Repository structure

```
├── ad_conversion_ab_test_analysis.ipynb   # Full analysis notebook
├── marketing_AB.csv                        # Dataset (or link, if not committing raw data)
└── README.md
```

