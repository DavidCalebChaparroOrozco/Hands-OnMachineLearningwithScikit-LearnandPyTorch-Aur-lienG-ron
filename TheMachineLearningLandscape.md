# The Machine Learning Landscape (IT'S NOT THE MODEL)
The data is the real problem

## Introduction: The Real Problem

A model cannot learn what the data does not show.

1. Models learn from the examples they see.
2. They assume that the training data represents all of reality.
3. They don't "reason" or "understand" the context.
4. Incorrect data results in an incorrect model.

_📌 A model is only as good as its training data._

### Problem #1: Non-Representative Training Set

What does it mean?
The training data does NOT reflect the reality where the model will be used.

The model assumes that the training data represents the real world, but it doesn't.

_⚠ If the training world is different from the real world, the model will fail._

**Remember this:**
   - The model only sees examples
   - It look for patterns in those examples
   - It generalizes from what it sees.
   - It does NOT reason or understand the context

_A well-trained model in the wrong world = A useless model_.

### Practice example: E-commerce premium
Objective: To predict the probability of purchase

Variables:
  - Estimated income
  - Product price
  - Purchase history
  - Type of distribution channel

_⚠ Data only from premium physical stores:_
High-spending and loyal customers

**Consequences 📊**
  - Low variability in income
  - Price is NOT the deciding factor
  - The "non-buying" class is a minority

**Biased dataset**
_Does not represent the overall market_

### What does the model learn?

The model learns correctly... but ONLY in the premium context.
  - Income almost never limits the purchase.
  - Price has little importance.
  - The probability of purchase is high.

**Works well:**
_In premium stores_

Training data and usage have the same distribution.

**Doesn't work well:**
_In regular stores_

- Changes in income
- Changes in price sensitivity
- Changes in user behavior

_The model was trained for a different environment_

### Problem #2: Sampling Noise
**What is it?**
The dataset is representative, but it is VERY SMALL.

The sample exhibits random variations that do not reflect reality.

Even if the data is correct, if it is **small**, it cannot capture the true pattern.

- We see a sample, not the entire population.

- Small samples exaggerate or obscure characteristics.

- The model learns product patterns randomly.

**The problem:**
It's not bias, it's the size.

_Small samples:_
Random patterns.

_Large samples:_
True patterns.

**Few observations = Unreliable conclusions**

## Analogy: The Coin

Imagine flipping a coin:

5 times > 4 heads and 1 tail. Result: 80% heads 😐
10 times > Still unstable, variable results.

1,000 times > Approaches a true 50/50. ✅ Reliable result

_Results with few samples are NOT reliable_

**That's sampling noise**

_More data = More confidence in the patterns_

### Problem #3: Sampling Bias.
The way data is collected introduces bias: certain cases are more likely to be selected. It's not the quantity, it's how the sample is chosen.

| Concept                 	| Main Problem                  	|
|-------------------------	|-------------------------------	|
| Non-representative data 	| The data is out of this world 	|
| Sampling noise          	| There's very little data      	|
| Sampling bias           	| The sample is poorly selected 	|

**Example:**
Instagram-only survey
Only survey people who:
- Are active on social media
- Follow you (affinity)
- Use that platform

_Does it represent the entire population?_
**NO**

_Only a specific audience_

### Problem #4: Outliers
**What is an outlier?**
An observation that deviates SIGNIFICANTLY from the rest of the data.

It's not simply a 'weird' value.

It's a point that breaks the overall pattern of the dataset.

They can appear due to:
- Measurement errors
- Data loading errors
- Extremely rare cases
- Real but very infrequent events.

**Why are they a problem?**
Many models assume:
- The data follows general patterns
- Extreme values ​​have little influence.

**With Outliers:**
- They can DRAG the model
- They distort statistics (mean)
- They generate incorrect boundaries.

Outliers: Examples and Warnings

Example: Average Spending Prediction:
Normal Spending: $20 - $100
Outlier Spending: $50,000 (Corporate Purchase)

That single point can:
- Raise the average
- Bias the model
- Incorrect predictions

Analogy: Salaries
99 Employees: $1k
1 Director: $100k
_The 'average' is misleading_

Important: They are not always bad
- Errors: Eliminate
- Real Events: Analyze
- Anomalies: Are the target

_Do not automatically delete!_