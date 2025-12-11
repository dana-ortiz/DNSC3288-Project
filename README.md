# DNSC3288-Project Model Card

### Basic Information
* **Group Members:** Dana Ortiz, dana.ortiz@gwu.edu
* **Date:** December 2025
* **Model Version:** 1.1
* **License:** MIT
* **Model Implementation Code:** [DNSC3288 Final Project](https://github.com/dana-ortiz/DNSC3288-Project/blob/main/DNSC3288%20final%20final%20(1).ipynb)

### Intended Use
* **Intended Uses:** This model is an educational example of a one-step-ahead time-series forecasting system that predicts next-month item sales for each (shop_id, item_id) pair. The use case mirrors retail demand forecasting for academic purposes.
* **Intended Users:** DNSC3288 Professors and teaching assistants for evaluation.
* **Out-of-Scope Use Cases:** Any commercial deployment, financial decision-making, inventory planning, or real-world operational use. This model is strictly for educational demonstrations.

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
| **item_cnt_month_lag_12** |	input |	float |	Sales count twelve months prior |
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
  * Training rows: ~2.95 million
  * Validation rows: ~214,000
 
### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 214,200
* **State any differences in columns between training and test data**: None
