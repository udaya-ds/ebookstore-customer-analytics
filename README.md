# eBook Store — Customer Analytics & Subscription Prediction

End-to-end customer analytics project for the eBook Store, covering exploratory data analysis, correlation/association analysis, and predictive modeling for customer subscription likelihood and average monthly spend.

🔗 **Interactive Tableau Dashboard:** [Demographics Dashboard](https://public.tableau.com/app/profile/udaya.s/viz/eBookStoreCustomerfeatures/DemographicsDashboard1#1)

## Project Overview

This project analyzes customer demographic and behavioral data for an eBook subscription business to:
- Understand customer segments and purchasing behavior through EDA
- Identify relationships between categorical and numerical features using Dython (association/correlation analysis for mixed data types)
- Build predictive models with PyCaret to forecast **subscription likelihood** and **average monthly spend**

## Key Findings

**1. Household composition and income are the strongest predictors of both spend and subscription.**
Statistical significance and correlation analysis identified `Number of Children at Home`, `Annual Income`, and `Total Number of Children` as the top predictors for both Average Monthly Spend and eBook subscription likelihood — while `Home Owner Status`, `Education Level`, `ZipCode`, and `Age` had the least impact on spend (and `Age` showed a negative correlation with subscription likelihood, the only feature to do so).

**2. Clear demographic and occupational segments drive higher value.**
Customers in Professional, Management, and Skilled Manual occupations showed both the highest average monthly spend and the highest subscription rates. Male customers and homeowners spent more on average, and spend increased with annual income and number of cars owned — while customer location showed no meaningful effect on either spend or subscription, ruling out a feature that might otherwise seem intuitive.

**3. LightGBM was the best-performing algorithm for both prediction tasks**, selected via PyCaret's automated leaderboard comparison with 10-fold cross-validation.

*Avg Monthly Spend — Regression*

| Metric | Value |
|--------|-------|
| R² | 0.9885 |
| MAE | 2.3155 |
| RMSE | 2.9215 |
| MSE | 8.5354 |
| MAPE | 3.61% |
| RMSLE | 0.0471 |

*eBook Subscriber Flag — Classification*

| Metric | Value |
|--------|-------|
| Accuracy | 80.90% |
| AUC | 0.8871 |
| Precision | 0.8204 |
| Recall | 0.5444 |
| F1 | 0.6545 |
| Kappa | 0.5298 |
| MCC | 0.5511 |

### Model Limitations & Next Steps

- **Recall is the weak point of the classification model.** At 54.4% recall vs. 82.0% precision, the model misses nearly half of actual eBook subscribers, even though its strong AUC (0.8871) shows it ranks customers well overall. This points to a conservative default classification threshold (0.5) rather than a weak model — adjusting the decision threshold downward would trade some precision for recall, which may be the right call if the cost of missing a likely subscriber outweighs the cost of a false positive outreach.
- **The regression model's R² (0.9885) is unusually high** for a behavioral/spend prediction task. While plausible given how strongly `Annual Income` and `Number of Children at Home` track with spend, this level of fit warrants a data leakage check — confirming no predictor is functionally derived from or duplicative of the target variable — before treating the result as production-ready.
- **No external validation set or time-based holdout was used** — the data is a single snapshot (April 2024), so the model hasn't been tested on out-of-period data. A natural next step is validating performance on a later month's data to confirm the relationships are stable over time, not specific to that snapshot.
- **Class imbalance was not explicitly addressed.** Given the recall/precision gap, it's worth checking the class balance of the eBook Subscriber Flag and testing techniques like class weighting or threshold tuning rather than relying on PyCaret's default settings.

## Repository Structure

```
.
├── notebooks/
│   ├── 01_EDA.ipynb                          # Exploratory data analysis
│   ├── 02_Dython_Correlation_Analysis.ipynb  # Mixed-type correlation/association analysis
│   └── 03_PyCaret_Modeling.ipynb             # Model training & evaluation with PyCaret
├── predictions/
│   ├── avg_monthly_spend_predictions.csv     # Predicted average monthly spend per customer
│   └── subscription_predictions.csv          # Predicted subscription outcome per customer
├── presentation/
│   └── eBookStore_EDA_Nov2025.pptx    # Stakeholder-facing summary deck
└── README.md
```

## Tools & Techniques

- **Python:** pandas, numpy, matplotlib/seaborn for EDA
- **Dython:** correlation/association analysis across categorical + numerical features
- **PyCaret:** automated model comparison, training, and evaluation for classification (subscription) and regression (monthly spend)
- **Tableau:** interactive dashboard for demographic and behavioral exploration

## Key Deliverables

1. **EDA Notebook** — data cleaning, distribution analysis, and initial insights on customer demographics and behavior
2. **Dython Notebook** — correlation heatmaps and association measures (Cramér's V, correlation ratio) to identify key drivers
3. **PyCaret Notebook** — model selection, tuning, and final predictions for subscription propensity and spend forecasting
4. **Prediction Outputs** — scored CSVs ready for business use
5. **Tableau Dashboard** — interactive visual summary for non-technical stakeholders
6. **Presentation Deck** — executive summary of findings and recommendations

## Author

Udaya S. — [Tableau Public Profile](https://public.tableau.com/app/profile/udaya.s)
