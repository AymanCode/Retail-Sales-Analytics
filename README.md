# Retail Sales Analytics (Rossmann) — KPI Dashboard + Promo Impact

I built a small, end-to-end retail analytics project on the Rossmann Store Sales data. The goal: answer the questions a hiring manager actually cares about—**are promos worth it, what are the trends, which stores lead, and what’s the weekly pattern?** Python for data prep; Tableau to tell the story.

**Live dashboard:**  
https://public.tableau.com/app/profile/ayman.islam/viz/Book1_17554702610350/Dashboard1?publish=yes

![Dashboard overview](reports/Dashboard%201-3.png)

---

## Highlights

- **Interactive Tableau dashboard (4–5 views):**
  - Monthly sales trend (filter by store/year)
  - Promotion effect on average sales
  - Top stores by total sales
  - Average sales by day of week
  - Holiday context
- **Unadjusted promo lift:** avg sales with promo ≈ **8,228** vs **5,929** without → **~39%** higher.
- **Adjusted promo effect (OLS, robust SEs):** promotions add **≈ +1,491** sales per open day  
  **[95% CI: 1,483 – 1,499]**, controlling for **Customers** and seasonality (Month/DOW/Year).
- **Takeaway:** promos raise sales even after accounting for traffic—likely from higher spend per visit and/or product mix.

---

## What this answers

- How are **monthly sales** trending?
- How much do **promotions** move the needle?
- Which **stores** lead on total sales (and sales per customer)?
- What’s the **day-of-week** pattern (weekend strength / Sunday closures)?
- How do **holidays** relate to sales at a high level?

---

## How I built it (quick)

- **Python (Jupyter):** merged store metadata, added seasonal features (Month/DOW sine–cosine), basic EDA, exported tidy KPI CSVs.
- **Tableau:** each KPI CSV is a separate data source; built 4 core worksheets and assembled one dashboard with filters.
- **Adjusted promo effect:** OLS on open days  
  `Sales ~ Customers + Promo + Month_sin + Month_cos + DOW_sin + DOW_cos + Year`  
  with HC1 robust standard errors.

---

## Reproduce locally

1. **Get the data** (Kaggle Rossmann competition) and place here:
```

data/raw/train.csv
data/raw/store.csv

```
2. **Run the notebook** to create KPI tables → outputs land in:
```

data/processed/kpi\_month\_store.csv
data/processed/kpi\_promo.csv
data/processed/kpi\_store.csv
data/processed/kpi\_dow\.csv
data/processed/kpi\_holiday.csv
data/processed/kpi\_day\_of\_month.csv
data/processed/kpi\_comp\_bins.csv

```
3. **Open Tableau** and connect each CSV. Recreate:
- Trend (convert `YearMonth` with `DATEPARSE("yyyy-MM", [YearMonth])`)
- Promo (mean sales by Promo)
- Leaderboard (total sales by Store)
- Day-of-Week (mean sales by DOW)
- Holiday (optional)
4. **(Optional) Re-estimate OLS** using the code block in the notebook to print the Promo coefficient + 95% CI.

---

## Key findings (from this dataset)

- **Traffic drives sales** (Customers has the strongest relationship with Sales).
- **Promotions matter:** ~**39%** naïve lift and **≈ +1,491/day** after adjusting for traffic & seasonality.
- **Weekly seasonality** is clear (Saturday strongest; Sunday dips track closures).
- **Holiday effects** show up; **competition distance** differences are modest at this aggregate level.

---

## Repo layout

```

.
├─ README.md
├─ notebooks/
│  └─ 01\_eda.ipynb
├─ data/
│  ├─ raw/            # <- place Kaggle CSVs here (gitignored)
│  └─ processed/      # KPI exports used by Tableau
├─ reports/
│  └─ Dashboard 1-3.png
└─ .gitignore

```

---

## Stack

Python (pandas, numpy, matplotlib, statsmodels) · Tableau Public

---

## Next steps (if I extend this)

- Add a **weekly demand forecast** (lag features + Ridge/XGBoost) and report MAPE vs naïve.
- Break out promo effect by **StoreType** or **Assortment**.
- Automate KPI exports and schedule a refreshed Tableau extract.

---

**Notes**  
The “unadjusted lift” is a straightforward average-to-average comparison (Promo=1 vs 0).  
The OLS estimate is the marginal effect holding traffic and seasonality constant.
```
