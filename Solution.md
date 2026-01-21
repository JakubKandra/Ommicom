# Junior Data Scientist - media & advertising Task
## Solution developed by Jakub Kandra

## 1. Introduction
We aim to estimate the impact of marketing investments on sales using weekly data.

## 2. Data Preparation
- Loaded data from `weekly_sales.csv`
- Created adstocked variables for TV, Online, Radio
- Normalized variables to avoid scaling issues

## 3. Model
- Fitted OLS regression: sales ~ investment_tv + investment_online + ...
- Checked assumptions: residuals, autocorrelation, multicollinearity

## 4. Results
| Variable | Coefficient | p-value | Interpretation |
|----------|------------|--------|----------------|
| investment_tv | 2.06e6 | 0.79 | Small effect |
| investment_online | 1.08e11 | 0.000 | Strong effect |

## 5. Diagnostics
- F-test p = 0.0146 → model significant
- Durbin-Watson = 1.7 → acceptable autocorrelation
- Condition number = 1.14e9 → multicollinearity warning

## 6. Visualizations
- Sales vs predicted sales plot
- Channel ROI bar chart

## 7. Conclusion
- Online advertising has the largest impact
- TV and banners are less effective
- Model is statistically valid for forecasting and ROI estimation
