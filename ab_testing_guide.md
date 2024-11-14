# A/B Testing Comprehensive Guide

## Table of Contents
- [1. Introduction](#1-introduction)
  - [1.1 Purpose](#11-purpose)
  - [1.2 Key Concepts](#12-key-concepts)
- [2. Test Methodology](#2-test-methodology)
  - [2.1 Test Steps](#21-test-steps)
  - [2.2 Hypothesis Types](#22-hypothesis-types)
  - [2.3 Benefits of Randomization](#23-benefits-of-randomization)
- [3. Statistical Foundations](#3-statistical-foundations)
  - [3.1 P-value and Significance](#31-p-value-and-significance)
  - [3.2 Types of Errors](#32-types-of-errors)
- [4. Resampling Methods](#4-resampling-methods)
  - [4.1 When to Use](#41-when-to-use)
  - [4.2 Types of Resampling](#42-types-of-resampling)
  - [4.3 Permutation Testing Process](#43-permutation-testing-process)
- [5. Test Design and Implementation](#5-test-design-and-implementation)
  - [5.1 Prerequisites](#51-prerequisites)
  - [5.2 Running the Experiment](#52-running-the-experiment)
  - [5.3 Results Analysis](#53-results-analysis)
- [6. Special Considerations](#6-special-considerations)
  - [6.1 Post-Launch Monitoring](#61-post-launch-monitoring)
  - [6.2 Special Cases](#62-special-cases)
  - [6.3 Effect Size](#63-effect-size)

## 1. Introduction

### 1.1 Purpose
- Compare two competing options (A, B)
- Examples include:
  - Medical treatments
  - Design variants (ads, web)
  - Product options
  - Pricing strategies
- Used to:
  - Determine statistical differences
  - Identify optimal options for specific goals

### 1.2 Key Concepts
- Control group: Standard or no treatment
- Treatment group: New treatment
- Recommendation: Use equal group sizes for better statistical power

Imagine an e-commerce website "SuperShop" is testing if adding a countdown timer to their checkout page increases sales. They have 10,000 daily visitors to their checkout page, which they split equally into two groups of 5,000 each. The control group (Group A) sees the regular checkout page with a simple "Complete your purchase" message - this is the standard treatment that's currently in use. The treatment group (Group B) sees the same checkout page but with a new 15-minute countdown timer that shows "Complete your purchase (15:00 minutes remaining)" and counts down while they shop - this is the new treatment being tested. By keeping both groups equal at 5,000 visitors each, SuperShop can more easily detect if the countdown timer makes a statistically significant difference in purchase completion rates. For example, with these equal group sizes, they can reliably detect even a small 2% difference in conversion rates with 90% confidence, whereas uneven group sizes would require a longer testing period to reach the same level of confidence.

## 2. Test Methodology

### 2.1 Test Steps
1. **Idea Definition**
   - Define question and goal
   - Identify data/subjects and options
   - Choose test statistic

For SuperShop's checkout page experiment, the primary question is "Does adding a countdown timer increase purchase completion rates?" The goal is to improve conversion rates by creating a sense of urgency during checkout. The test subjects are visitors who reach the checkout page, with user data including their unique visitor ID, time spent on checkout, and whether they completed their purchase. The two options being tested are the standard checkout page (control) versus a checkout page with a 15-minute countdown timer (treatment). The key test statistic will be the conversion rate, calculated as the number of completed purchases divided by the total number of checkout page visitors in each group. Additional metrics to monitor include average cart value and cart abandonment rate to ensure the timer doesn't negatively impact other important business metrics. Statistical significance will be measured using a two-sided test with a 95% confidence level (α = 0.05) to determine if any observed difference between the groups is meaningful.

2. **Subject Selection**
   - Define complete subject set

3. **Randomization**
   - Randomly assign subjects to groups
   - Ensures bias elimination

When a visitor arrives at SuperShop's checkout page, the website's A/B testing system automatically assigns them to either the control or treatment group using a random number generator. For example, if the generated number is even, the visitor sees the regular checkout page; if odd, they see the countdown timer version. This randomization ensures that factors like time of day, customer demographics, or shopping habits don't accidentally concentrate in one group. Without random assignment, we might accidentally show the timer version mostly to morning shoppers or primarily to mobile users, which would bias our results. The random assignment also helps distribute unknown variables we haven't thought about – perhaps some customers are shopping during their lunch break or using different devices – across both groups evenly. This way, any difference we observe in conversion rates can be attributed to the countdown timer itself rather than these external factors.

4. **Results Collection**
   - Expose subjects to options
   - Measure results
   - Compute test statistic

Once visitors are randomly assigned to groups, SuperShop's system automatically displays either the standard checkout page or the countdown timer version. The system then tracks key metrics for each visitor over a 30-day testing period: whether they completed their purchase (primary conversion event), time spent on the checkout page, cart abandonment rate, and final purchase value. For example, if the control group has 500 completed purchases out of 5,000 visitors (10% conversion rate), and the treatment group with the timer shows 600 completions out of 5,000 visitors (12% conversion rate), the observed difference is 2 percentage points. The test statistic is calculated by comparing these conversion rates using a two-proportion z-test, which accounts for both the size of the difference and the sample sizes. The system also logs additional metrics like average cart value ($75 for control vs. $72 for treatment) to ensure the timer isn't negatively impacting purchase amounts while improving conversion rates.

### 2.2 Hypothesis Types
- **Null Hypothesis (H₀)**: Random choice (A = B)
- **Alternate Hypothesis (H₁)**:
  - Two-way test: A ≠ B
  - One-way test: A > B or A < B

In SuperShop's checkout timer test, the null hypothesis (H₀) assumes there's no real difference between the two checkout page versions – in other words, any observed difference in conversion rates between the standard page and the timer page is due to random chance. The alternative hypothesis (H₁) proposes that there is a genuine difference caused by the timer. SuperShop uses a two-way test since they want to detect any significant difference, whether positive or negative (A ≠ B). For example, while they hope the timer increases conversions, they need to know if it might decrease them too. If they were only interested in proving the timer increases conversions, they would use a one-way test (A > B). In numerical terms, if the control page has a 10% conversion rate, the null hypothesis says the timer page should also have a 10% rate, while the alternative hypothesis suggests it could be higher or lower. Through statistical testing, if they find a p-value less than 0.05, they'll reject the null hypothesis and conclude the timer does have a real impact on customer behavior.

### 2.3 Benefits of Randomization
- Eliminates bias
- Balances confounding variables
- Makes groups comparable
- Validates results
- Observed differences due to either random chance or real difference

## 3. Statistical Foundations

### 3.1 P-value and Significance
- **P-value Definition**: Probability of obtaining results as extreme as observed
- **Significance Level (α)**:
  - Predefined probability threshold (common values: 0.05, 0.01, 0.1)
  - Must be set before experiment
- **Decision Rules**:
  - If p-value < α: Reject null hypothesis
  - If p-value > α: Retain null hypothesis

### 3.2 Types of Errors
- **Type I Error (α)**:
  - False Positive
  - Rejecting H₀ when true
- **Type II Error (β)**:
  - False Negative
  - Accepting H₀ when false

## 4. Resampling Methods

### 4.1 When to Use
- Non-normal distributions
- Limited data collection ability
- High data collection costs
- Expired offers/campaigns

### 4.2 Types of Resampling
1. **Bootstrap Sampling**:
   - Sampling with replacement
   - Estimates precision and variability
   - Minimum 10,000 resamples recommended

2. **Permutation Testing**:
   - Sampling without replacement
   - Validates/rejects null hypothesis
   - Non-parametric test

### 4.3 Permutation Testing Process
1. Calculate observed difference between groups
2. Combine and shuffle data
3. Reassign to groups
4. Calculate test statistic
5. Determine significance

## 5. Test Design and Implementation

### 5.1 Prerequisites
1. **Define Key Metrics**:
   - Overall Evaluation Criterion (OEC)
   - Specific metrics for test goals

2. **Design Considerations**:
   - Population selection
   - Change magnitude
   - Implementation feasibility

3. **Sample Size and Duration**:
   - Calculate required sample size: n = f(α, β, δ)
   - Run for complete business cycles
   - Account for variations
   
n (per group) = 2(Zα + Zβ)² × [p(1-p)] / δ²

where:
n = required sample size per group
Zα = Z-score for significance level (1.96 for α = 0.05)
Zβ = Z-score for power (0.84 for β = 0.20, or 80% power)
p = expected baseline conversion rate
δ = minimum detectable effect (expected difference)

### 5.2 Running the Experiment
- Data Collection Methods:
  - Engineering collaboration
  - Logging instrumentation
  - CRM systems
  - Internal tracking

### 5.3 Results Analysis
1. **Sanity Checks**
2. **Statistical vs Practical Significance**
3. **Distribution Assumptions**

## 6. Special Considerations

### 6.1 Post-Launch Monitoring
- Metric conflicts
- Time-based effects
- Long-term vs short-term impacts

### 6.2 Special Cases
- Social network effects
- Two-sided markets
- Assumption violations
- Solutions for common issues

### 6.3 Effect Size
d = (M₁ - M₂) / sp

where:
sp = √[(s₁² + s₂²) / 2]

M₁ = Mean of group 1 (treatment)
M₂ = Mean of group 2 (control)
s₁ = Standard deviation of group 1
s₂ = Standard deviation of group 2
sp = Pooled standard deviation

Cohen's d Categories:
| Effect Size | d Value |
|------------|---------|
| Small      | 0.2     |
| Medium     | 0.5     |
| Large      | 0.8     |