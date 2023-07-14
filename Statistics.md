
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
