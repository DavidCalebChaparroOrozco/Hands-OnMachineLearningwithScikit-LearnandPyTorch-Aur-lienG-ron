# How to prepare your variables so that models learn better

## What is Feature Scaling?

A mathematical transformation that adjusts the scale of numerical variables without altering their information.

### Why?
To prevent variables with large ranges from dominating others in model calculations.

> The problem arises when different features have very different orders of magnitude.

### **Scale-Sensitive Algorithms**
- KNN: Euclidean distances
- K-Means: Centroids
- Gradient Descent: Convergence
- Neural Networks: Activations
- PCA: Variance

---

## The Scaling Problem with the `California Housing Dataset`

### Highly Disparate Ranges
Total_rooms reaches 39,320 while median_income only reaches 15.

_Without scaling, the model gives more weight to large variables... even if they aren't necessarily more important._

> Median_income is the strongest predictor of price, but it has the smallest range!

```python
min           max         range
HouseAge      1.000000     52.000000     51.000000
AveRooms      1.130435    141.909091    140.778656
AveBedrms     0.333333     34.066667     33.733333⚠️
Population    3.000000  35682.000000  35679.000000⚠️
AveOccup      0.692308   1243.333333   1242.641026
Latitude     32.540000     41.950000      9.410000
Longitude  -124.350000   -114.310000     10.040000
MedInc        0.499900     15.000100     14.500200✅
```

---

## Normalized Min-Max Scaling

Rescales each variable to a fixed range [0, 1]

$$X' = \frac{(x- min)}{(max - min)}$$
> The result is always between 0 and 1

**Properties**
- ✅ Preserves the shape of the distribution
- ✅ Guaranteed range [0, 1] or [-1, 1]
- ⚠️ Sensitive to extreme outliers
- 🌟 Ideal for neural networks and KNNs


![alt text](MinMaxScaling.png)

---

## Min-Max: Step-by-Step Example

1. Original Data: (10, 20, 30, 40)

2. Calculate min and max
- min = 10
- max = 40
- range = 30

3. Apply the formula
$$x' = \frac{(x- min)}{(max - min)}$$

$$x' = \frac{(x- 10)}{(30)}$$


![alt text](MinMaxScalingExample.png)

> Minimum: 0
> 
> Maximum: 1
> 
> Same shape, new 

---

