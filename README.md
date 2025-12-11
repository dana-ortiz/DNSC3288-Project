# DNSC3288-Project Model Card

### Basic Information
* **Group Members:** Dana Ortiz, dana.ortiz@gwu.edu
* **Date:** December 2025
* **Model Version:** 1.11
* **License:** MIT
* **Model Implementation Code:** [DNSC3288 Final Project](https://github.com/dana-ortiz/DNSC3288-Project/blob/main/DNSC3288%20final%20final%20(1).ipynb)

### Intended Use
* **Intended Uses:** This model is an educational example of a one-step-ahead time-series forecasting system that predicts next-month item sales for each (shop_id, item_id) pair. The use case mirrors retail demand forecasting for academic purposes.
* **Intended Users:** DNSC3288 Professors and teaching assistants for evaluation.
* **Out-of-Scope Use Cases:** Any commercial deployment, financial decision-making, inventory planning or real-world operational use. This model is strictly for educational demonstrations.

### Training Data
* **Data Dictionary:**

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
| **date_block_num** |	input	| int	| Integer month index (0–32) used in Kaggle dataset |
| **shop_id**	| input |	int |	Unique shop identifier |
| **item_id** |	input |	int |	Unique item identifier |
| **item_category_id** |	input |	int |	Category assigned to each item |
| **month** |	input |	int |	Calendar month (0–11) derived from date_block_num |
| **year** |	input |	int	| Calendar year derived from date_block_num |
| **item_cnt_month** |	target |	float |	Monthly sales count (clipped 0–20) |
| **item_cnt_month_lag_1** |	input |	float |	Sales count one month prior for item–shop pair |
| **item_cnt_month_lag_2** |	input |	float |	Sales count two months prior |
| **item_cnt_month_lag_3** |	input |	float |	Sales count three months prior |
| **item_cnt_month_lag_6** |	input |	float |	Sales count six months prior |
| **item_price_month** |	input |	float |	Average monthly item price |
| **item_price_month_lag_1** | input |	float |	Previous month’s average price |
| **price_trend** |	input |	float |	Difference between current and previous price |
| **shop_rev** |	input |	float |	Total shop revenue for a given month |
| **shop_rev_lag_1** |	input |	float |	Previous month shop revenue |
| **item_mean** |	input |	float	| Average monthly sales for the item |
| **shop_mean** |	input |	float |	Average monthly sales for the shop |
|**cat_mean** |	input |	float |	Average monthly sales for the item category |
| **item_first_sale_block** |	input |	int |	First month the item appeared |
| **item_age** |	input |	int |	Time since first recorded sale (in months) |

* **Source of Training Data:** [Kaggle Competition Dataset](https://www.kaggle.com/competitions/competitive-data-science-predict-future-sales/data)
* **How training data was divided into training and validation data:** Months 0–31 were used as the training set (86.7%). Month 32 was used as the validation set (6.6%). Month 33 (constructed separately) is used to generate predictions for the Kaggle test set (6.6%).
* **Number of rows in training and validation data:**
  * Training rows: 6,797,997
  * Validation rows: 308,782
 
### Test Data
* **Source of test data**: [Kaggle Submission 11 File](https://github.com/dana-ortiz/DNSC3288-Project/blob/main/submission11.csv))
* **Number of rows in test data**: 214,200
* **State any differences in columns between training and test data**: Test data contains only: ID, shop_id, item_id. Training data (full) includes: date_block_num, shop_id, item_id, item_cnt_month (target) and all engineered features listed above.

### Model details
* **Columns used as inputs in the final model**: 'date_block_num', 'shop_id', 'item_id', 'item_category_id', 'month', 'year', 'item_cnt_month_lag_1', 'item_cnt_month_lag_2', 'item_cnt_month_lag_3', 'item_cnt_month_lag_6', 'item_price_month', 'item_price_month_lag_1', 'price_trend', 'shop_rev', 'shop_rev_lag_1', 'item_mean, shop_mean, cat_mean, item_first_sale_block, item_age
* **Column(s) used as target(s) in the final model**: 'item_cnt_month'
* **Type of model**: XGBoost Regressor (gradient-boosted decision trees)
* **Software used to implement the model**: Python, XGBoost, pandas, NumPy, scikit-learn
* **Version of the modeling software**: XGBoost.3.0.2
* **Hyperparameters or other settings of your model**:
```
XGBRegressor(
    max_depth=8,
    min_child_weight=10,
    n_estimators=1600,
    learning_rate=0.02,
    subsample=0.9,
    colsample_bytree=0.9,
    reg_alpha=0.2,
    reg_lambda=1.2,
    objective='reg:squarederror',
    n_jobs=4,
    tree_method='hist'
)
```

### Quantitative Analysis
* **Metrics Used to Evaluate**: RMSE; see details below:

| Training RMSE | Validation RMSE | Test RMSE |
| ------ | ------- | -------- |
| **0.4706**| **0.3820**  | **1.23643** |

### Ethical Considerations
* **Potential negative impacts of using this model:**
  * *Math or Software Problems:* Because of the overlapping training/validation months (block 32 is included in both), model performance on validation may be over-optimistic. The model heavily relies on historical lags meaning that items with little or no history may receive poor predictions. If future data distributions shift, the model may degrade in performance.
  *  *Real World Risks:* (If someone tried to deploy this model in a real retail context) Overestimating sales may lead to overstocking, tying up capital and causing waste. Underestimating sales may lead to stockouts, lost revenue and customer dissatisfaction. Smaller shops or items with sparse data could be systematically mispredicted, potentially disadvantaging them.
* **Uncertainties relating to the impacts of using the model:**
 * *Math or Software Uncertainties:* The model does not incorporate external factors like holidays, promotions or macroeconomic variables. The simple feature set (monthly lags, mean-encodings, price & revenue trends) may not fully capture seasonality and promotion effects.
  *  *Real World Uncertainties:* In real deployments, data quality issues (delayed reporting, missing records) could degrade performance. Organizational decisions could invalidate historical patterns the model depends on.
* **Unexpected Results:** There was a large discrepancy between validation RMSE and test RMSE. The model achieved a very low validation RMSE of 0.3820, but the Kaggle public leaderboard score (test RMSE) was 1.23643, indicating substantially weaker generalization to unseen data. Upon reviewing the workflow, this discrepancy is explained by the fact that the training partition included rows from date_block_num 32, which is the same month used for validation evaluation. This resulted in data leakage, where information from the validation month influenced the model during training, artificially improving validation performance. 
