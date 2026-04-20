# Model training in Scikit-Learn

## RMSE, Cross Validation, Decision Trees, Random Forest

## 1. RMSE

### Intuitive Idea

What does **RMSE** measure?

How far the model's predictions are from the actual values.

- It penalizes large errors more (by squaring them)
- Result in the same units as the data
- Lower value = better model
- RMSE = 0 means perfect prediction

![alt text](RMSE.png)

### Formula and Calculation

$$
\mathrm{RMSE}(X, y, h) \;=\; \sqrt{\frac{1}{m} \sum_{i=1}^{m} \left( h\left(x^{(i)}\right) - y^{(i)} \right)^2}
$$

- m: total number of data points
- h(x): model prediction
- y: actual value

### Example Temperature Prediction

| Day 	| Real (y) 	| Prediction h(x) 	| Error 	|
|:---:	|:--------:	|:---------------:	|:-----:	|
|  1  	|    20    	|        18       	|   -2  	|
|  2  	|    25    	|        30       	|   5   	|
|  3  	|    30    	|        28       	|   -2  	|

**Step by step:**
1. Errors: -2, 5, -2
2. Squared errors: 4, 25, 4
3. Average: $\frac{(4 + 25 + 4)}{3}  = 11$
4. Square root: $\sqrt{11} = 3.3$

RMSE = 3.3
On average, the model is off by ~3 degrees


### Interpretation and Use

How to interpret RMSE?

- ✅ `0` Perfect prediction
- 👍 `Low` Accurate model
- ⚠️ `High` Inaccurate model

> Always use the same units as the data (km, °C, $, ... )

- Regression problems: Predicting continuous numerical values
- Penalizing large errors: When serious failures are costly
- Avoiding major failures: Critical systems such as pricing or diagnostics

---

