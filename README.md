# Calculating 5th and 95th Percentiles and Winsorization Example: A Comprehensive Guide

This document provides a step-by-step explanation of how to calculate the 5th and 95th percentiles of a dataset and apply Winsorization to handle outliers. Winsorization is a technique used to limit extreme values in a dataset by replacing them with values closer to the mean or median, typically at a specified percentile.

**Outliers identified in this dataset are:** *60*, *100*, and *-20*.

***

## Table of Contents

1.  [Introduction](#calculating-5th-and-95th-percentiles-and-winsorization-example-a-comprehensive-guide)
2.  [Step 1: Sort the Data Ascending](#step-1-sort-the-data-ascending)
3.  [Step 2: Calculate the Ranks (Positions) for 5th and 95th Percentiles](#step-2-calculate-the-ranks-positions-for-5th-and-95th-percentiles)
    * [5th Percentile Rank](#5th-percentile-rank)
    * [95th Percentile Rank](#95th-percentile-rank)
4.  [Step 3: Interpolate Values for Percentiles](#step-3-interpolate-values-for-percentiles)
5.  [Step 4: Winsorization (Clip Data to These Bounds)](#step-4-winsorization-clip-data-to-these-bounds)
6.  [Step 5: Apply to Your Data](#step-5-apply-to-your-data)
7.  [Summary](#summary)
8.  [Python Source Code for Winsorization](#python-source-code-for-winsorization-of-the-example-data)

***

## Step 1: Sort the Data Ascending

First, arrange the given data points in ascending order:

-20, 3, 3, 3, 5, 6, 7, 8, 60, 100

There are **10** data points in this set ($$N = 10$$).

***

## Step 2: Calculate the Ranks (Positions) for 5th and 95th Percentiles

The formula for calculating the rank (position) of the Pth percentile is:

$$R = \left(\frac{P}{100}\right) \times (N - 1) + 1$$

Where:
* $P$ = desired percentile (e.g., 5 or 95)
* $N$ = number of data points (10 in this example)

### 5th Percentile Rank:

Using the formula for $P = 5$:

$$R_5 = \left(\frac{5}{100}\right) \times (10 - 1) + 1 = 0.05 \times 9 + 1 = 0.45 + 1 = 1.45$$

The 5th percentile is located between the 1st and 2nd data points.

### 95th Percentile Rank:

Using the formula for $P = 95$:

$$R_{95} = \left(\frac{95}{100}\right) \times (10 - 1) + 1 = 0.95 \times 9 + 1 = 8.55 + 1 = 9.55$$

The 95th percentile is located between the 9th and 10th data points.

***

## Step 3: Interpolate Values for Percentiles

Since the calculated ranks are not whole numbers, we need to interpolate to find the exact percentile values. The general interpolation formula is:

$$\text{Value} = \text{Value at Rank } i + (R - i) \times (\text{Value at Rank } (i+1) - \text{Value at Rank } i)$$

Where $i$ is the integer part of the rank $R$.

* The value at rank 1 = *-20*
* The value at rank 2 = *3*

**5th percentile value:**

For $R_5 = 1.45$, we use $i=1$:

$$= -20 + (1.45 - 1) \times (3 - (-20))$$$$= -20 + 0.45 \times 23$$$$= -20 + 10.35$$
$$= -9.65$$

* The value at rank 9 = *60*
* The value at rank 10 = *100*

**95th percentile value:**

For $R_{95} = 9.55$, we use $i=9$:

$$= 60 + (9.55 - 9) \times (100 - 60)$$$$= 60 + 0.55 \times 40$$$$= 60 + 22$$
$$= 82$$

***

## Step 4: Winsorization (Clip Data to These Bounds)

Based on our calculations, the Winsorization bounds are:

* Replace values **below -9.65** with *-9.65*.
* Replace values **above 82** with *82*.

***

## Step 5: Apply to Your Data

Let's apply these rules to the original dataset:

| Original | Winsorized | Explanation |
| :------- | :--------- | :---------- |
| -20      | -9.65      | Clipped to 5th percentile |
| 3        | 3          | Within bounds |
| 5        | 5          | Within bounds |
| 8        | 8          | Within bounds |
| 6        | 6          | Within bounds |
| 7        | 7          | Within bounds |
| 3        | 3          | Within bounds |
| 60       | 60         | Within bounds |
| 100      | 82         | Clipped to 95th percentile |

***

## Summary:

* The outlier *-20* was clipped to *-9.65*.
* The outlier *100* was clipped to *82*.
* The value *60* remains unchanged as it falls within the calculated bounds.

***

## Python Source Code for Winsorization of the Example Data:

```python
import numpy as np

# Original data
data = np.array([3, 5, 3, 8, 6, 7, 3, 60, 100, -20])

# Define the percentiles for Winsorization
lower_percentile = 5
upper_percentile = 95

# Calculate the lower and upper bounds using numpy's percentile function
lower_bound = np.percentile(data, lower_percentile)
upper_bound = np.percentile(data, upper_percentile)

# Apply Winsorization using numpy's clip function
winsorized_data = np.clip(data, lower_bound, upper_bound)

# Print the results
print("Original data:      ", data)
print(f"{lower_percentile}th percentile:", lower_bound)
print(f"{upper_percentile}th percentile:", upper_bound)
print("Winsorized data:    ", winsorized_data)
