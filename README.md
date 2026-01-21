# Junior Data Scientist - media & advertising Task
## Solution developed by Jakub Kandra

## 0. Introduction
Directory is composed by:
- data files: "data_monthly.csv", "data_weekly.csv",
- description of task: "homework_new_hires_v2026.html",
- solution of task in iPython notebook: "Solution.ipynb",
- slides as answer to client "SlidesForClient.pdf"
- this file "Solution.md"

## 2. Joining weekly and monthly data
- I think it is possible to join weekly and monthly data. I prepared a function ("from_weekly_to_monthly") which transfer weekly data to monthly. Idea explained in next points.
- In weeks composed by two months, estimate fraction (weight) of each month and provide a week in two rows
- Divide variables depends on required arithemtic operation for weekly data transfering to monthly:
  * variables describing money or holidays could be summed
  * variables describing how many days store are open, indices, knowledge, recognitions coulde be averaged (to provide mean of them)
- In week composed by two months, weights is multiplied by value of variable needed to be summed for both months
- Then this weekly data is grouped by months and applied required arithemtic operation to provide monthly data 
- Finally this new monthly data is concating with old monthly data together.
- Customers or sales can be affected by unemployment to possibly reduce sales and the GDP could possibly increase sales. 

## 1. Overview of client’s business
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/3c8d5094-22f1-471d-acfe-8688f16f3cf4" />


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
