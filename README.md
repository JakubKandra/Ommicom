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
After applying behavioral findings and setting Ordinary Least Squares (OLS) model, we receive results:
<img width="1042" height="675" alt="image" src="https://github.com/user-attachments/assets/6d4837d6-01f2-455d-9d5e-4489eed2ea73" />
<img width="1043" height="216" alt="image" src="https://github.com/user-attachments/assets/bd3bfdd8-a879-4ba6-b064-8e93524691eb" />

### 8. Comments to weekly model
#### Marketing:
Several variables (columns) look important (according p-value: (P > |t|) < 0.05):
* Investments - TV
* Investments - Press
* Investments - Online
* Public holidays
* Stores open
* Weather index
* Economy index
* Brand knowledge

#### Statistics
* From R-squared, 74% of sales is explained by model
* From Prob(F-statistics), the model is statistically significant (p < 0.01)
* From Prob(Ommibus) or Prob(Jarque-Bera), residuals looks normal (p > 0.05)
* From Durbin-Watson, residuals look independent (p ~ 2)
* From Conditional number, strong multicollinearity or numerical instability affects model

#### Comments
Investments in TV, press and online were highly scored in feature importance or highly correlated.
Stores opened, weather index, economy index and brand knowledge were highly scored or highly correlated.
Maybe public holidays achive less importance or not significant correlations.

### 9. Model validation
Split dataset to fitting and validation parts. At first step, fit the first part and use additional four weeks for validation. In the next step, remove earliest four week in fitting part, add four validation weeks (from previous step) into fitting part, provide fit and additional fours week use for validation. Repeat until end of dataset.

#### 9A. Predictive power 
* Check root mean square error (RMSE) as function of validated months
* Check mean absolute percentage error (MAPE) as function of validated months
* Check mean absolute error (MAE) as function of validated months
* Check coefficient of determination ($\mathrm{R}^2$) as function of validated months
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/7c906bb4-48c5-4e52-b06c-06624451c80a" />
<img width="856" height="275" alt="image" src="https://github.com/user-attachments/assets/f4ba4678-b38d-40ca-b100-e67ad0af1ba5" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/a7c9c9da-fde0-446c-8ccf-8138b44a0d68" />

#### 9B. Stability of coefficients
* Check sign of coefficients as function of fitted interval
* Check magnitude of coefficients as function of fitted interval
* Check error of magnitudes as function of fitted interval
<img width="856" height="278" alt="image" src="https://github.com/user-attachments/assets/df01ce31-5450-40cc-8ccd-ee1c6c68ec1f" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/8f4c05f4-0078-4140-ae68-6b61024e47af" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/818e2efd-67ba-47b1-b266-286e16a48fe0" />
<img width="856" height="280" alt="image" src="https://github.com/user-attachments/assets/ce83d205-ac03-43cd-aaa6-ac31f7fcb227" />
<img width="856" height="284" alt="image" src="https://github.com/user-attachments/assets/2a53e366-190d-4f01-9181-a20a58908582" />

#### 9C. Residual diagnostics
* Plot predicted values, fitted values and its residuals (very simple)
<img width="960" height="339" alt="image" src="https://github.com/user-attachments/assets/ab1d5734-4cb6-4e68-b922-aa21f6ab1c23" />

#### 9D. Contribution plausibility
* Compare contribution of marketing channel in investments and sales

#### 9E. Behavioral sanity checks
* Check if diminishing returns is realistic (expert eye?)
* Check if add memory is realistic (expert eye?)

#### 9F. Sensitivity analysis
* Remove one channel, refit and check result, any significant difference?
* Increase or decrease addMemory and diminishingReturns parameters about 10%

#### 11A. Channel contribution to sales 
<img width="435" height="284" alt="image" src="https://github.com/user-attachments/assets/5a8ec183-fc50-45ec-981d-daff3a858d85" />

It looks the main contribution on sales ~ 50% has invenstment to online, then ~ 35 % has investment to TV, following with ~ 15 % investment to press. Investment to radio is very close zero and investment to radio is negative (it can be interpret as zero with fluctuation of model fit)

#### 11B. Marketing efficiency
<img width="434" height="284" alt="image" src="https://github.com/user-attachments/assets/3915d77c-01c8-4f74-9512-49aa99c49162" />

It looks the most efficient is investment to radio more than 4, then investment to press about 2.5, following with investment to TV close to 2. The investment to online is effective about 1.5. The efficinecy of investment to banners is negative, what proving our idea, this channel is not relevant for both contribution in sales and efficiency. 

The funny observation is that contribution in sales has opposite order in compare to efficiency. Would be nice to understand why. 

### 10. What and how to present results to client (SlidesForClient.pdf)
* Describe data preparation and implementation of behavioral findings to model
* Simple statistical, mathematical view to model result
* Show marketing iteresting results

### 12. 
