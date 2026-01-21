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

### 6. Behavioral findings using diminishing returns and influencing ads immediately with memory
- For diminishing returns we are using Hill function or S-curve:
$$ f(x) = \frac{x^{\gamma}}{x^{\gamma}+k^{\gamma}} $$
**Hill function**\
$$ f(x) = \frac{x^{\gamma}}{x^{\gamma}+k^{\gamma}} $$
- For ads memory we are using "decay law":
**Decay law**\
$$ x(t) = \mathrm{Investments}(t) + \lambda \cdot x(t-1), \mathrm{where} \ 0 \ \leq \ \lambda \ \leq \ 1 $$

- Both functions requires specific parameters ($\gamma$, k, $\lambda$). These parameters are extracted from data as:
  + $\gamma$: np.percentile(series, 70); where zeros are removed from series
  + 

**The Cauchy-Schwarz Inequality**

```math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```

**The Cauchy-Schwarz Inequality**\
$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$



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
