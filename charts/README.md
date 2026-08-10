# Charts

Static PNG copies of every major chart from the notebook — open any file with an image viewer, no Jupyter required. Colors are consistent throughout the project: 🟦 **Surf**, 🟧 **Ultimate**.

For the interactive (hover/zoom/pan) versions of these same charts, plus the full interactive dashboard, open [`Megaline project`](Megaline_project.ipynb).

---

## Calls

### Average Monthly Call Minutes by Plan
`calls/avg_call_minutes.png`

**Business question:** Do Surf and Ultimate users have different calling behavior?
**What it shows:** Average monthly call minutes per plan, by month.
**Observation:** Average usage is nearly identical between plans (Surf 428.75 vs. Ultimate 430.45 minutes, ~0.4% difference).
**Business implication:** The revenue difference between plans is not driven by different calling *habits* — it's driven by what happens when Surf users, who have a much smaller included allowance, exceed it.

### Distribution of Monthly Call Minutes by Plan
`calls/call_distribution.png`

**Business question:** How is call usage spread across the customer base — are there many heavy users, or is usage evenly distributed?
**What it shows:** Histogram of monthly minutes, split by plan, with mean/median reference lines highlighting right-skew.
**Observation:** Both plans are right-skewed with high variability (std ≈ 235–240 minutes); most users cluster at moderate usage, with a smaller tail of heavy callers pulling the mean above the median.
**Business implication:** A plan built around the "average" user under-serves the sizable minority of heavy users — this is exactly the overage revenue Surf captures.

### Boxplot of Monthly Call Minutes by Plan
`calls/call_boxplot.png`

**Business question:** How does the *spread* of call usage compare between plans, beyond the average?
**What it shows:** Boxplot summarizing median, quartiles, and outliers of monthly minutes per plan.
**Observation:** Both plans show a wide interquartile range and long upper whiskers/outliers, confirming the right-skew seen in the histogram.
**Business implication:** Neither plan's calling behavior is "safer" than the other in terms of spread — the commercial difference comes entirely from where each plan's limit sits relative to that spread.

---

## Messages

### Average Monthly Messages by Plan
`messages/avg_messages.png`

**Business question:** Do Ultimate users, who pay more, actually send more texts?
**What it shows:** Average monthly messages per plan, by month.
**Observation:** Ultimate users send noticeably more messages on average (37.55 vs. 31.16, ~20% higher).
**Business implication:** Unlike calls and data, messaging shows a real behavioral gap between plans — though it may reflect differences between the two customer bases rather than the plan itself.

### Distribution of Monthly Messages by Plan
`messages/message_distribution.png`

**Business question:** Is the messaging gap consistent across the whole customer base, or driven by a few heavy texters?
**What it shows:** Histogram of monthly message counts by plan.
**Observation:** Both distributions are right-skewed, with Ultimate's distribution shifted slightly higher overall.
**Business implication:** The shift is broad-based rather than a few outliers — reinforcing that Ultimate customers text more as a group.

### Boxplot of Monthly Messages by Plan
`messages/message_boxplot.png`

**Business question:** How does message-count variability compare between plans?
**What it shows:** Boxplot of monthly message counts per plan.
**Observation:** Surf's box sits lower with a smaller median; Ultimate's median and upper quartile are both higher.
**Business implication:** Combined with the 50-message Surf limit, this modest usage gap is still enough to push **21.6% of Surf users into overage**, versus effectively 0% of Ultimate users.

---

## Internet

### Average Monthly Internet Usage by Plan
`internet/avg_data_usage.png`

**Business question:** Is data usage — the most expensive overage category — similar across plans?
**What it shows:** Average monthly GB used per plan, by month.
**Observation:** Usage is close between plans (Surf 16.67 GB vs. Ultimate 17.31 GB, ~4% difference).
**Business implication:** Because Surf's 15 GB allowance sits almost exactly at the average Surf user's usage, even small variation pushes the majority of Surf users into overage.

### Distribution of Monthly Internet Usage by Plan
`internet/data_distribution.png`

**Business question:** How much of the Surf base is realistically at risk of exceeding their data limit?
**What it shows:** Histogram of monthly GB usage by plan.
**Observation:** A large share of the Surf distribution sits at or above the 15 GB line, while Ultimate's 30 GB allowance comfortably covers almost its entire distribution.
**Business implication:** Data is the single biggest driver of Surf's overage revenue, and the biggest risk for Surf customers of "surprise" charges.

### Boxplot of Monthly Internet Usage by Plan
`internet/data_boxplot.png`

**Business question:** How does the spread of data usage compare between plans?
**What it shows:** Boxplot of monthly GB usage per plan.
**Observation:** Surf's interquartile range sits much closer to its own limit than Ultimate's does to its limit.
**Business implication:** This gap between "typical usage" and "plan limit" is the structural reason **57.9% of Surf users** pay data overage, vs. only **5.7% of Ultimate users**.

---

## Revenue

### Distribution of Monthly Revenue by Plan
`revenue/revenue_distribution.png`

**Business question:** How does actual monthly revenue per user compare between plans?
**What it shows:** Histogram of monthly revenue by plan.
**Observation:** Ultimate's revenue clusters tightly around its $70 fixed fee; Surf's revenue is much more spread out, with a long tail from overage charges.
**Business implication:** Ultimate is a predictable, fixed-fee-driven revenue stream. Surf is a variable, usage-driven one — riskier per customer, but with more upside from heavy users.

### Boxplot of Monthly Revenue by Plan
`revenue/revenue_boxplot.png`

**Business question:** Which plan has more consistent, forecastable revenue per customer?
**What it shows:** Boxplot of monthly revenue by plan.
**Observation:** Ultimate's box is narrow and close to $70; Surf's box is wide, with many high-value outliers above $60.
**Business implication:** This variance difference (var ≈ 130 for Ultimate vs. ≈ 3,068 for Surf) is exactly what motivated using **Welch's t-test** (unequal variances) rather than a standard Student's t-test for the hypothesis tests below.

---

## Statistical Tests

### Statistical Test Summary
`statistical_tests/hypothesis_results.png`

**Business question:** Are the revenue differences observed above real, or could they be due to random sampling noise?
**What it shows:** A table summarizing both Welch's t-tests — H0, α, p-value, and decision.
**Observation:** Both tests reject H0 at α = 0.05: Surf vs. Ultimate revenue (p ≈ 3.17 × 10⁻¹⁵) and NY-NJ vs. other regions revenue (p = 0.0335).
**Business implication:** The plan-level revenue gap is backed by strong statistical evidence. The regional gap is also significant, but the p-value is close enough to the threshold — and the NY-NJ sample small enough — that it deserves a follow-up check with more data before being treated as a settled fact.
