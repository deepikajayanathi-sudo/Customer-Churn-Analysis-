# Customer Churn Analysis

Analyzed customer data to identify churn patterns and retention insights using **Python** (cleaning, EDA, statistical tests, driver model) and **Power BI** (retention dashboard).

## Project Structure
```
data/raw/customer_churn_raw.csv        # raw, messy data (7,068 rows)
data/processed/customer_churn_clean.csv# cleaned + engineered features -> Power BI
data/processed/churn_by_segment.csv    # segment churn rates
data/processed/eda_log.txt             # full audit/EDA output
src/generate_data.py                   # builds the sample dataset
src/01_clean_eda.py                    # cleaning + EDA + model + export
images/                                # charts
powerbi/DAX_measures.md                # DAX + dashboard build guide
```
## Run
```bash
pip install -r requirements.txt
python src/generate_data.py      # skip if using your own/Kaggle Telco file at data/raw/customer_churn_raw.csv
python src/01_clean_eda.py
```
> Data note: the included dataset is **synthetic** (Telco-style schema). Drop in the real IBM/Kaggle *Telco Customer Churn* CSV (same columns) to get real-world numbers.

## 1. Data Cleaning
- Removed **25 duplicate** rows; standardized `gender` casing/whitespace.
- `TotalCharges` stored as text with **11 blanks** → converted to numeric; blanks were tenure-0 customers → set to 0.
- Imputed 45 missing `MonthlyCharges` (median by InternetService) and 30 `PaymentMethod` (mode).
- Engineered: `TenureGroup`, `ChargeBand`, `AddOnCount`, `AutoPay`, `RiskSegment`, `ChurnFlag`.

## 2. Key Findings (sample data)
- **Overall churn: 27.4%** (1,929 of 7,043); ~**$127.7K monthly revenue** lost.
- **Contract is the strongest driver:** month-to-month **43.1%** vs one-year 12.3% vs two-year 3.6%.
- **New customers churn most:** 0–12 months tenure = **47.5%**.
- **Fiber optic** 35.1% vs DSL 20.8%; **electronic check** payers 34.0% vs ~24% for others.
- No TechSupport: 31.6% vs 24.0% with it; no AutoPay 30.0% vs 23.9%.
- Month-to-month customers in first year ("High risk") churn at **52.1%**.
- Chi-square tests: all significant (p<0.05) except PaperlessBilling and Dependents.
- Random Forest (ROC-AUC **0.81**) ranks tenure, charges, and contract as top drivers; `ChurnProbability` is exported for Power BI.

## 3. Recommendations
1. Incentivize month-to-month → annual contract migration (discounts, perks).
2. Run an onboarding/retention program in the first 12 months.
3. Review fiber-optic pricing/quality; bundle TechSupport & OnlineSecurity.
4. Push customers from electronic check to AutoPay.
5. Target the highest `ChurnProbability` active customers with proactive outreach.

## 4. Power BI Dashboard
Import `data/processed/customer_churn_clean.csv`; follow `powerbi/DAX_measures.md` for measures, 3 pages (Executive Overview, Churn Drivers, At-Risk Customers) and slicers.

![Overview](images/churn_overview.png)
![Contract](images/churn_by_contract.png)
![Drivers](images/feature_importance.png)

## Resume Bullets
- Analyzed 7K+ customer records to identify churn patterns, uncovering that month-to-month contracts and first-year tenure drive 43–52% churn vs 4% on two-year plans.
- Cleaned and engineered features in Python (pandas, scikit-learn, SciPy): deduplication, imputation, chi-square testing and a Random Forest churn model (ROC-AUC 0.81).
- Built a Power BI retention dashboard with DAX KPIs, drill-downs and an at-risk customer list to support retention strategy.
