# Megaline Plan Revenue Analysis

**Which prepaid plan — Surf or Ultimate — should Megaline back with more advertising budget?**

An end-to-end data analysis of Megaline's two prepaid plans (Surf and Ultimate), based on 500 clients' call, text-message, and internet-usage records from 2018, built to answer a single commercial question: **which plan generates more revenue?**

---

## Dashboard

The interactive dashboard is **embedded directly inside the Jupyter Notebook** — built entirely with Plotly, no external BI tool. It includes KPI cards, plan revenue comparison, usage comparison, plan-limit overage analysis, an interactive monthly trend (dropdown metric selector), revenue distribution, and a statistical test summary table.

**[→ Open the Interactive Dashboard Notebook](notebooks/Statistics_project_2.ipynb)** (Dashboard is Section 8, right after the data pipeline is built)

Static, standalone copies of every chart (including the dashboard's) are also available under [`charts/`](charts/) as PNG image files you can view without opening Jupyter.

---

## Executive Summary

- **Usage behavior is nearly identical** between plans (calls, messages, data all within ~0.4%–20% of each other) — customers use what they need, not what the plan allows.
- **Plan limits are the real revenue driver.** 36% of Surf users exceed their minute limit, 21.6% their message limit, and 57.9% their data limit — and pay overage. Almost no Ultimate users ever do.
- **Ultimate earns more per user**: $72.31 vs. $60.71 average monthly revenue, and is far more predictable (much lower variance).
- **Surf earns more in total**: $95,491 vs. $52,066, because it has a larger customer base (339 vs. 161 users).
- **Both differences are statistically significant** (Welch's t-test, α = 0.05).
- **Recommendation:** Ultimate for revenue-per-user efficiency; Surf for total revenue and market scale.

---

## Business Problem

Megaline's commercial department needs to know which of its two prepaid plans — **Surf** ($20/mo, 500 min / 50 texts / 15 GB) or **Ultimate** ($70/mo, 3000 min / 1000 texts / 30 GB) — brings in more revenue, in order to allocate advertising budget accordingly. This project performs a preliminary analysis on a sample of 500 clients' 2018 usage data to answer that question with evidence, not intuition.

---

## Dataset

Five source tables (500 users, 2018 activity) — see [`data/README.md`](data/README.md) for full details:

| Table | Grain | Key columns |
|---|---|---|
| `users` | 1 row / customer | `user_id`, `age`, `city`, `plan`, `reg_date`, `churn_date` |
| `calls` | 1 row / call | `user_id`, `call_date`, `duration` |
| `messages` | 1 row / SMS | `user_id`, `message_date` |
| `internet` | 1 row / session | `user_id`, `session_date`, `mb_used` |
| `plans` | 1 row / plan | monthly fee, included minutes/messages/data, overage prices |

---

## Methodology

1. **Data loading** — read all five CSVs.
2. **Data cleaning** — convert date columns to `datetime`; check for duplicates, negative values, and missing data.
3. **Datetime conversion** — extract billing month from each date column.
4. **Monthly aggregation** — per user per month: call count, minutes used (each call individually rounded up), message count, data used (rounded up to GB at the monthly total level, per Megaline's billing rule).
5. **User activity construction** — outer-join calls / messages / internet aggregates into one `user_activity` table (missing activity in a source table = 0 usage, not missing data).
6. **Plan information merge** — attach each user's plan terms (fees, allowances, overage prices).
7. **Revenue calculation** — fixed fee + `max(usage - allowance, 0) × overage price`, summed across minutes, messages, and data (see Section 7 of the notebook for the exact formula and why `.clip(lower=0)` is used).
8. **Exploratory analysis** — bar charts (average usage), histograms (distribution + skew), boxplots (spread) for calls, messages, internet, and revenue.
9. **Statistical hypothesis testing** — two Welch's t-tests (unequal variances/sample sizes).
10. **Dashboard construction** — Plotly KPI cards, comparison charts, overage analysis, interactive monthly trend, and a statistical summary table, all inside the notebook.

---

## Key Findings

- Average monthly minutes: **Surf 428.75** vs. **Ultimate 430.45** (~0.4% difference).
- Average monthly messages: **Surf 31.16** vs. **Ultimate 37.55** (~20% difference).
- Average monthly data: **Surf 16.67 GB** vs. **Ultimate 17.31 GB** (~4% difference).
- Share of user-months exceeding the plan limit — **Surf**: 36% (minutes), 21.6% (messages), 57.9% (data). **Ultimate**: ~0%, 0%, 5.7%.
- Average monthly revenue: **Ultimate $72.31** (low variance) vs. **Surf $60.71** (high variance).
- Total revenue across all user-months: **Surf $95,491** vs. **Ultimate $52,066**.

---

## Statistical Testing

Both tests use a **two-sided Welch's t-test** (`equal_var=False`), because the compared groups have unequal sample sizes and unequal variances.

### Test 1 — Surf vs. Ultimate Revenue
- **H0:** mean revenue(Surf) = mean revenue(Ultimate)
- **H1:** mean revenue(Surf) ≠ mean revenue(Ultimate)
- **α:** 0.05
- **p-value:** 3.17 × 10⁻¹⁵
- **Decision:** Reject H0
- **Business interpretation:** There is sufficient evidence that plan choice is associated with a real difference in average revenue.

### Test 2 — NY-NJ vs. Other Regions Revenue
- **H0:** mean revenue(NY-NJ) = mean revenue(other regions)
- **H1:** mean revenue(NY-NJ) ≠ mean revenue(other regions)
- **α:** 0.05
- **p-value:** 0.0335
- **Decision:** Reject H0
- **Business interpretation:** There is sufficient evidence of a regional revenue difference, though the p-value sits closer to the 0.05 threshold and the NY-NJ group is smaller (n=377 vs. n=1,916) — worth validating with more data before acting on it heavily.

---

## Business Recommendation

| Objective | Recommended plan | Why |
|---|---|---|
| **Maximize revenue per user** | **Ultimate** | Higher, more stable average revenue per customer. |
| **Maximize total / market-scale revenue** | **Surf** | Larger customer base + strong overage income (especially data) drives higher total revenue. |

Surf and Ultimate are each attractive depending on the objective: Ultimate is the efficient, predictable per-customer bet; Surf is the volume play that benefits from its larger addressable market and overage economics. Neither plan is a strictly "better" plan — the right answer depends on whether the advertising budget is optimized for **efficiency** or **scale**.

---

## Limitations

- Data covers a **single year (2018)** and a relatively **small sample (500 users)**.
- The **NY-NJ region group is smaller and imbalanced** relative to other regions combined (377 vs. 1,916 user-months).
- This is **observational data** — plan choice is not randomly assigned, so differences may partly reflect self-selection (e.g., heavier users choosing Ultimate) rather than the plan itself.
- **Statistical significance does not imply causation.** A rejected H0 means the observed difference is unlikely to be due to chance in this sample — it does not prove that switching a user's plan would change their spending behavior.

---

## Project Structure

```
megaline-plan-revenue-analysis/
│
├── README.md
│
├── notebooks/
│   └── Statistics_project_2.ipynb      # full analysis + embedded interactive dashboard
│
├── data/
│   ├── README.md
│   ├── megaline_users.csv
│   ├── megaline_calls.csv
│   ├── megaline_messages.csv
│   ├── megaline_internet.csv
│   └── megaline_plans.csv
│
├── charts/
│   ├── README.md
│   ├── calls/
│   ├── messages/
│   ├── internet/
│   ├── revenue/
│   └── statistical_tests/
│
└── .gitignore
```

---

## Technologies

- **Python 3**
- **pandas** — data cleaning, aggregation
- **NumPy** — rounding/billing logic
- **Plotly** (`plotly.express`, `plotly.graph_objects`, `plotly.subplots`) — every chart and the interactive dashboard
- **SciPy** (`scipy.stats`) — Welch's t-tests, skewness
- **Jupyter Notebook**

---

## How to Run

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd megaline-plan-revenue-analysis
   ```
2. **Install dependencies**
   ```bash
   pip install pandas numpy plotly scipy jupyter
   ```
3. **Open the notebook**
   ```bash
   jupyter notebook notebooks/Statistics_project_2.ipynb
   ```
4. **Run all cells** (Kernel → Restart & Run All). The notebook reads the CSVs from `../data/` relative to its own location, so keep the folder structure intact.
5. **Open the dashboard section** — scroll to **Section 8, "Interactive Dashboard"** for the executive summary view, or open any file under `charts/` with an image viewer for a static copy of an individual chart.

---

## Author

*Ədalət* — Data Analyst in training (DataCube & Hamburg IT Academy bootcamp). Feel free to connect or reach out with questions about this project.
