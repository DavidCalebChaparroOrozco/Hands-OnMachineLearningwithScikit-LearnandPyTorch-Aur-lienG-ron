# Create your own processing parts: Pipeline, BaseEstimator, GridSearchCV

## `fit() vs transform()`

### `fit()`
**Learns from the dataset**
Calculates and saves values ​​that depend on the data: means, standard deviations, categories, etc.

> Called only ONCE (during training)

### `transform()`
**Applies what has been learned**
Uses the already saved values ​​to modify the data. Does not recalculate anything new.

> Called during both training and test.


---

## Real Example: StandardScaler

### What does it do internally?

- **`fit(X_train):`**
calculates 
    - $\mu:$ (mean) 
    - $\alpha:$(standard deviation)

- **Saves:**
  - self.mean = $\mu$
  - self.scale = $\alpha$

- `Transform(X):`
Applies: $\frac{(X - \mu)}{\alpha}$

```python
scaler = StandardScaler()

# fit(): LEARN median and std of each feature from the training data
scaler.fit(X_train)

# Save: scaler.mean_ and scaler.scale_ contain the learned parameters (mean and std) for each feature

# transform(): APPLY the learned parameters to transform the data (subtract mean and divide by std)
X_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)

# NEVER fit the scaler on the test set! Only use the parameters learned from the training set to transform the test set scaler.fit(X_test) 
# This would be a mistake! it would leak information from the test set into the training process, leading to overly optimistic perfomance estimates. ALWAYS fit on the training data only, then transform both training and test data using the same scaler.
```

---

## FunctionTransformer

### A Quick Wrapper
Converts any Python function into a Pipeline-compatible transformer.

### *USE IT WHEN:*
- The function doesn't need to learn anything
- There are no parameters to calculate in `fit()`
- The logic is simple and stateless

```python
# Your function to apply log transformation to the data
def log_transform(X):
    return np.log1p(X)

# Return a FunctionTransformer that applies the log transformation
transformer = FunctionTransformer(log_transform, validate=False)

# Now is compatible with Pipeline and can be used to create a pipeline that first applies the log transformation and then std scaling
pipe = Pipeline([
    ("log", FunctionTransformer(log_transform)),
    ("scaler", StandardScaler()),
])
```

---

## When to use each one?

| Approach                         | Learns from the dataset? | Saves its own parameters? | Pipeline compatible? | GridSearchCV compatible? | Code complexity | Typical example |
|----------------------------------|--------------------------|----------------------------|----------------------|--------------------------|------------------|------------------|
| FunctionTransformer              | ❌ No                    | ❌ No                      | ✅ Yes               | ❌ Limited               | ⭐ Very Low       | Simple log transform, scaling a column, custom math function |
| Custom Class (BaseEstimator)     | ✅ Yes                   | ✅ Yes                     | ✅ Yes               | ✅ Yes                   | ⭐⭐⭐ Medium       | Target encoding, feature selection, custom normalization based on training data |

> Practical rule:
> - If you need a real `fit()` function → use a Class
> - Otherwise → `FunctionTransformer`

---

## Custom Transformer

A class you create yourself to transform data within a machine learning pipeline.

> Your own processing "piece" that fits into any flow

- ### Clean text:
    Normalize, remove characters, tokenize

- ### New columns:
    Custom feature engineering

- ### Business rules:
    Domain-specific logic

- ### Custom transformations:
    What scikit-learn doesn't offer out of the box

> ### Your custom logic, integrated into the pipeline

---

## Anatomy of a Custom Transformer

### Key Parts
- **`BaseEstimator`:** `get_params` / `set_params` automatic
- **`TransformerMixin:`** `fit_transform()` free
- **`_init_():`** Define your parameters
- **`fit():`** Learn from the dataset
- **`transform():`** Apply the transformation

```python
class MyTransformer(BaseEstimator, TransformerMixin):

    def __init__(self, factor=1):
        # This is a parameter that can be set when creating an instance of MyTransformer. It will be used in the transformation process.
        self.factor = factor 

    # Learning phase: calculate the mean of the data and store it in an instance variable (self.average_). This will be used later in the transformation phase.
    def fit(self, X, y=None):
        self.average_ = X.mean()
        # Return self to allow chaining of methods (e.g. in a pipeline)
        return self
    
    # Transformation phase: apply the transformation to the data. In this case, it subtracts the mean (self.average_) from each value in X and then multiplies the result by the factor (self.factor).
    def transform(self, X, y=None):
        return (X - self.average_) * self.factor
```