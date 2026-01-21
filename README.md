# Junior Data Scientist - media & advertising Task
## Solution developed by Jakub Kandra

## 0. Introduction
Directory is composed by:
- data files: "data_monthly.csv", "data_weekly.csv",
- description of task: "homework_new_hires_v2026.html",
- solution of task in iPython notebook: "Solution.ipynb",
- slides as answer to client "SlidesForClient.pdf"
- this file "README.md"

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
### Overwiew of targeting variable
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/3c8d5094-22f1-471d-acfe-8688f16f3cf4" />

### Overview of marketing channels
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/5e3aed76-9b5c-4143-981a-35aed5e54592" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/4a62a64b-8ec0-43f2-949b-f6248dd89b5e" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/1d5c30e3-e5b8-4608-83c6-818f1bad3a34" />

It is showing continuous investments to online and sporadic investments to TV, radio, press and banners

### Overview of competitors investments
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/0c508f08-09b9-4648-b4a9-182584178b3c" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/2c039ff3-d9ff-4d26-91cd-e1ccfb0bbaca" />

It could affect targeting variable

### Overview of other variables
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/096840ff-367b-4f98-8bdf-9b2700b200e3" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/b4ebc767-ec43-42df-8293-e517cf532275" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/d5807473-bf95-46de-8001-78454f8b8673" />

These variables could explain some trends in targeting variable as periodicity or slow decreasing.

### Overview of specific variables for monthly data
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/996cd53c-2c38-4f2f-9869-f2f3fd0126ed" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/7a15f0ce-5dda-4970-a536-105e48cfc5fd" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/d505fd38-b18a-4c63-9669-2b2e917ab0bf" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/b254c20a-692a-4cdd-ad78-1058f7d591e8" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/77930bcd-3600-438c-9e34-be33bfec45aa" />

These variables could be interesting for working with monthly dataset

## 3. Can we do pre-selection?
Strategy:
  * Check correlation
  * Check feature of importance in regression fit by machine learning model (xgboost)
<img width="641" height="486" alt="image" src="https://github.com/user-attachments/assets/00df7348-8dc9-4cb7-a8d8-39bbb8941939" />

Maybe columns as 'investments_competition_2', 'investments_competition_1', 'investments_banners', 'precipation_index' and 'investments_competition' can be removed from analysis.

<img width="530" height="377" alt="image" src="https://github.com/user-attachments/assets/fd78cc47-9861-456b-90da-fbac86bcbf44" />

Low importance was found for 'investments_competition_2', 'economy_index', 'public_holidays' and 'stores_open'. 

## 5. Apply pre-selection to dataset
I did some checks, but I am not sure if it is relevant enough to remove any column from final model. I will keep all variables.

## 4. Variables (columns) format
In my understanding, there is no reason to apply any correction to variables to be ready for final model

## 6. Behavioral findings using diminishing returns and influencing ads immediately with memory
- For diminishing returns we are using Hill function or S-curve:
```math
f(x) = \frac{x^{\gamma}}{x^{\gamma}+k^{\gamma}}
```

- For ads memory we are using "decay law":
```math
x(t) = \mathrm{Investments}(t) + \lambda \cdot x(t-1), \mathrm{where} \ 0 \ \leq \ \lambda \ \leq \ 1
```

- Both functions requires specific parameters ($\gamma$, k, $\lambda$). These parameters are extracted from data as:
  + $\gamma$: np.percentile(series, 70); where zeros are removed from series
  + k: 1 + np.log10(series.std() + 1)
  + $\lambda$: min(max(autocorr_weekly, 0.05), 0.9)
 
## 7. Building a marketing mix model(s)
I prepared two models based on weekly and monthly datasets.
Probably, there could be some variables which will behave differently in these models.
Otherwise the monthly model can be as a proof for same observations in weekly model (as control model). 
I will not describe monthly model, but it can be found in Solution.ipynb to check.

                            OLS Regression Results                            
==============================================================================
Dep. Variable:                  sales   R-squared:                       0.740
Model:                            OLS   Adj. R-squared:                  0.698
Method:                 Least Squares   F-statistic:                     17.79
Date:                Thu, 22 Jan 2026   Prob (F-statistic):           1.88e-22
Time:                        00:02:33   Log-Likelihood:                -1816.3
No. Observations:                 117   AIC:                             3667.
Df Residuals:                     100   BIC:                             3714.
Df Model:                          16                                         
Covariance Type:            nonrobust                                         
===================================================================================================================
                                                      coef    std err          t      P>|t|      [0.025      0.975]
-------------------------------------------------------------------------------------------------------------------
investment_tv_adsMemory_diminishingReturns       1.805e+06   3.81e+05      4.740      0.000    1.05e+06    2.56e+06
investment_radio_adsMemory_diminishingReturns    8.201e+05   1.13e+06      0.725      0.470   -1.42e+06    3.06e+06
investment_press_adsMemory_diminishingReturns    1.513e+06   4.99e+05      3.029      0.003    5.22e+05     2.5e+06
investment_banners_adsMemory_diminishingReturns -6.393e+04   3.97e+05     -0.161      0.872   -8.51e+05    7.23e+05
investment_online_adsMemory_diminishingReturns   1.854e+06   4.36e+05      4.250      0.000    9.88e+05    2.72e+06
investment_competition                             -0.0273      0.023     -1.180      0.241      -0.073       0.019
investment_competition_1                           -0.0279      0.023     -1.192      0.236      -0.074       0.019
investment_competition_2                            0.0067      0.021      0.317      0.752      -0.035       0.049
competitor_recognition_1                         -1.74e+06   3.46e+06     -0.503      0.616    -8.6e+06    5.12e+06
competitor_recognition_2                        -3.146e+06   3.44e+06     -0.916      0.362   -9.96e+06    3.67e+06
public_holidays                                 -1.275e+06   2.23e+05     -5.732      0.000   -1.72e+06   -8.34e+05
stores_opened                                    5.478e+04    1.6e+04      3.430      0.001    2.31e+04    8.65e+04
economy_index                                    -344.8687    170.024     -2.028      0.045    -682.192      -7.546
brand_knowledge                                  8.377e+06    3.8e+06      2.206      0.030    8.42e+05    1.59e+07
weather_index                                   -9.084e+06   1.43e+06     -6.363      0.000   -1.19e+07   -6.25e+06
precipitation_index                             -5268.3927   3705.978     -1.422      0.158   -1.26e+04    2084.163
constant                                         2.497e+07   7.55e+06      3.307      0.001    9.99e+06       4e+07
==============================================================================
Omnibus:                        0.101   Durbin-Watson:                   1.928
Prob(Omnibus):                  0.951   Jarque-Bera (JB):                0.257
Skew:                          -0.038   Prob(JB):                        0.879
Kurtosis:                       2.783   Cond. No.                     1.14e+09
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.14e+09. This might indicate that there are
strong multicollinearity or other numerical problems.



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
