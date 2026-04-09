# Employment Type & Salary Model
### Regression Research for Job Search Applications

---

## Background

One of the most common questions job seekers face is whether to pursue full-time
employment or contract/freelance work. The financial trade-off is rarely obvious —
contract roles often carry higher hourly rates but lack benefits, stability, and
long-term growth. Full-time roles offer predictability but may cap earning potential
for senior professionals.

This project builds a regression model that quantifies the salary difference between
employment types across job titles, experience levels, company sizes, and locations —
giving job seekers a data-driven answer to the question: *is going contract actually
worth it for someone like me?*

---

## Business Problem

A job search app needs to surface a "should I go contract?" insight for each user
based on their profile. To do this accurately, the app needs a model that:

- Predicts expected salary for both full-time and contract arrangements
- Accounts for experience level, job title, company size, and location
- Quantifies the premium (or discount) that contract work carries for that specific profile
- Updates as market conditions shift year over year

**This model powers a "Full-Time vs. Contract Calculator" feature in the app —
giving users a personalized salary comparison before they negotiate or switch.**

---

## Dataset

- **Source:** [ai-jobs.net Salaries](https://aijobs.net/salaries/download/) — CC0 Public Domain
- **Download:** `salaries.csv` from `aijobs.net/salaries/download/`
- **Observations:** 14,000+ real salary submissions (2020–2024)
- **Response variable:** `salary_in_usd` — annual salary in US dollars
- **Key predictors:**

| Column | Description |
|---|---|
| `work_year` | Year the salary was paid |
| `experience_level` | EN (Entry), MI (Mid), SE (Senior), EX (Executive) |
| `employment_type` | FT (Full-Time), CT (Contract), FL (Freelance), PT (Part-Time) |
| `job_title` | Role title (e.g. Data Scientist, ML Engineer) |
| `remote_ratio` | 0 = on-site, 50 = hybrid, 100 = fully remote |
| `company_size` | S (Small), M (Medium), L (Large) |
| `company_location` | ISO country code of employer |
| `employee_residence` | ISO country code of employee |

---

## Approach

The analysis is structured in three parts:

**Part 1 — Exploratory Analysis**
Understand salary distributions by employment type, identify which job titles and
experience levels show the largest full-time vs contract gaps, and check for
multicollinearity across predictors.

**Part 2 — Regression Modeling**
Five models are trained and compared on a 70/30 train/test split:

| Model | Purpose |
|---|---|
| Linear Regression | Baseline with all predictors |
| Stepwise Selection | Reduced, interpretable model |
| Ridge Regression | Handles multicollinearity in encoded predictors |
| Lasso Regression | Automatic feature selection |
| Elastic Net | Balanced regularization |

**Part 3 — Employment Type Impact**
Isolate and quantify the salary premium or discount for contract vs full-time work
after controlling for all other factors. This is the core output for the app feature.

---

## Key Findings

- Full-time roles average **$157,907/year**; contract roles average **$103,185/year**
- The contract discount is not uniform — senior and executive roles narrow the gap significantly
- Remote ratio has minimal effect on salary once experience and title are controlled for
- Ridge regression generalizes best to unseen data
- The employment type coefficient after controlling for all factors reveals the *true*
  financial cost or benefit of going contract for a given profile

---

## Production Recommendation

Deploy **Ridge regression** as the salary estimation engine with employment type
as a switchable input. The app feature works as follows:

1. User enters: job title, experience level, company size, location, remote preference
2. Model runs twice — once with `employment_type = FT`, once with `employment_type = CT`
3. App displays the predicted salary for each, the dollar difference, and a
   plain-English recommendation based on the gap size

---

## File Structure

```
employment-salary-model/
├── analysis.Rmd       # Full analysis — EDA, modeling, employment type comparison
├── requirements.R     # Package installation
└── README.md
```

---

## Setup

```r
source("requirements.R")
```

Download `salaries.csv` from [aijobs.net/salaries/download/](https://aijobs.net/salaries/download/)
and place it in the same folder as `analysis.Rmd` before knitting.

---

## Data License

The ai-jobs.net salary dataset is released under **CC0 (Public Domain)**.
Free to use, modify, and distribute for any purpose.
