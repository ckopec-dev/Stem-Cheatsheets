
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
import statistics

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

~~~
