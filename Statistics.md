
# Statistics Cheatsheet

Statistics is the practice and study of collecting and analyzing data. 

## Main types of statistics

- Descriptive statistics focuses on describing and summarizing data.
- Inferential statistics uses sample data to make inferences about a larger population.

## Main types of data

- Numeric (quantitative). Sub-types are continuous (measured) and discrete (counted).
- Categorical (qualitative). Sub-types are nominal (unordered) and ordinal (ordered).

## Measures of center

~~~

import numpy as np
import statistics

# Mean: a.k.a. average. Add all values and divide by value count. Sensitive to extreme values.
np.mean()

# Median: value where 50% of other values are higher, and 50% are lower. Useful for non-symetrical data.
# Data is left-skewed when it's piled up on the right, and vice-versa. The skew is on the side of the tail.
np.median()

# Mode: the most frequent value in the data.
statistics.mode()

~~~

### Measures of spread

~~~

import numpy as np
from scipy.stats import iqr

# Variance: average distance from each data point to the data's mean.
# The higher the variance, the more spread out the data is.

# This is the manual/long way to calculate it.
dists = df["ColumnA"] - np.mean(df["ColumnA"])
sq_dists = dists ** 2
sum_sq_dists = np.sum(sq_dists
variance = sum_sq_dists / ({number_of_data_points} - 1)

# The short way.
# ddof=1 means sample variance. If it isn't specified, population variance is calculated instead.
variance = np.var(df["ColumnA"], ddof=1)

# Standard deviation: the square root of the variance.
np.std(df["ColumnA"], ddof=1))

# Mean absolute deviation
dists = df["ColumnA"] - mean(df$ColumnA)
np.mean(np.abs(dists))

# Quantiles: split up the data into a number of equal parts.
q = np.quantile(df["ColumnA"], 0, 0.25, 0.5, 0.75, 1)

# Another way of creating quantiles:
# np.linspace({start}, {stop}, {num})
np.quantile(df["ColumnA"], np.linspace(0, 1, 5))

# Interquartile range: the 0.25 - 0.75 quartile range. Height of the box in a boxplot.
iqr(df["ColumnA"])

~~~

### Outliers

An outlier is a data point "substantially" different from the others. A general rule of thumb is: data < Q1 - 1.5 x IQR OR data > Q3 + 1.5 * IQR.

~~~

import numpy as np
from scipy.stats import iqr

iqr = iqr(df["ColumnA"])
lower_threshold = np.quantile(df["ColumnA"], 0.25) - 1.5 * iqr
upper_threshold = np.quantile(df["ColumnA"], 0.75) + 1.5 * iqr

# Subset the outliers
df[(df["ColumnA"] < lower_threshold) | (df["ColumnA"] > upper_threshold)]

~~~

## Probability

The probability of an event is the number of ways it can happen divided by the total number of possible outcomes.

### Random sampling

~~~

import numpy as np

# To insure reproducibility, set random seed
np.random.seed(10)

# Get random record
df.sample()

# Get 2 random records (without replacement. i.e. don't pick the same record)
df.sample(2)

# Get 2 random records with replacement
df.sample(2, replace = True)

~~~

### Probability distribution

- Describes the probability of each possible outcome.
- The expected value is the mean.
- Discrete distributions represent probabilities with discrete outcomes (e.g. flipping a coin).
- Discrete uniform distribution: when all outcomes have the same probability (e.g. rolling a die).
- Law of large numbers: as the sample size increases, the sample mean approaches the expected value.

### The normal distribution

- Shape is a bell curve
- Symetrical
- Area beneath curve = 1
- Curve never hits 0
- Described by its mean and standard deviation
- 68% of area falls within 1 standard deviation
- 95% falls within 2 standard deviations
- 99.7% falls within 3 standard deviations

### Find percent of values less/more than given normal distribution

~~~

from scipy.stats import norm
norm.cdf({value}, {mean}, {std})
norm.ppf({percent}, {mean}, {std})

# Examples

# How many people are shorter than 150 cm?
norm.cdf(150, 162, 4)

# How many people are taller than 150 cm?
1 - norm.cdf(150, 162, 4)

# How many people are 150-153 cm?
norm.cdf(153, 162, 4) - norm.cdf(150, 162, 4)

# What height are 80% of people shorter than?
norm.ppf(0.8, 162, 4)

~~~

### The central limit theorum

- The sampling distribution approaches the normal distribution as the number of trials increases
- Only applies when samples are random and independent

### The Poisson distribution

- Events appear the happen at a certain rate, but at random
- Probability of some # of events happening over a fixed period of time
- Lambda: average number of events per time interval

~~~

from scipy.stats import poisson

# Examples

# If average number of events per week is 14, what is the probability of 6 events in a week?
poisson.pmf(6, 14)

# If average number of events per week is 14, what is the probability of 6 or fewer events in a week?
poisson.cdf(6, 14)

# If average number of events per week is 14, what is the probability of more than 6 events in a week?
1 - poisson.cdf(6, 14)

~~~

### Correlation coefficient

- Quantifies the linear relationship between two variables. Range is -1 to 1.

## Making predictioms

~~~

# The dataset
bream = fish[fish["species"] == "Bream"]
print(bream.head())

# Plot it to visually explain the relationship of mass v length
sns.regplot(x="length", y="mass", data=bream, ci=None)
plt.show()

# Create a fitted model and examine it
model = ols("mass ~ length", data=bream).fit()
print(model.params)

# Create explanatory data (in this example, all integer lengths in range of 20 - 41)
explain = pd.DataFrame({"length"; np.arange(20, 41)})

# Predict
print(model.predict(explain))

# Create predictiona (as above), but retain original columns and add new one (mass)
predict = pd.assign(mass=model.predict(explain)

# Plot the predictions along  with original scatterplot
fig = plt.figure()
sns.regplot(x="length", y="mass", data=bream, ci=None)
sns.scatterplot(x="length", y="mass", data=predict, color="red", marker="s")
plt.show()

# Extrapolating outside range of observed data (frequently does not work well)
small_bream = pd.DataFrame({"length": [10]})
predict = small_bream.assign(mass=model.predict(small_bream))
print(predict)

# Show predictions on the original dataset
print(model.fittedvalues)

# Show prediction errors (actual response values minus predicted response values)
print(model.resid)

# Show extended details of the model
print(model.summary())

~~~

### Quantifying model fit

- R-squared: the proportion of the variance in the response variable that is predictable from the explanatory variable (1 = perfect fit, 0 = no fit). This value is part of the summary function output.
- Coefficient determination: correlation squared
- Residual standard error (RSE): typical difference between a prediction and observed response
- Mean squared error (MSE): RSE squared
