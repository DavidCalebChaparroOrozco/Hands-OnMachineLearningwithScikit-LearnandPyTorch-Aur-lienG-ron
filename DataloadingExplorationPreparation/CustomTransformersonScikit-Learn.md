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