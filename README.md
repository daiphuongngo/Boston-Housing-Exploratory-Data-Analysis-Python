# Boston-Housing-Exploratory-Data-Analysis

## Overview

This dataset is collected by the US Census Service regarding real estate housing 
in the city of Boston, Massachusetts. This can be found on the [StatLib archive](http://lib.stat.cmu.edu/datasets/boston) with 167 cases.

It was first published by Harrison, D., and Rubinfeld, D.L. Hedonic prices and the demand for clean air', J. Environ. Economics & Management, vol.5, 81-102, 1978.
This dataset was then converted into CSV file having 11 attributes. I could experience different imperfections including missing and outliers as NaN, numeric, string types. 

## Language:

- Python

## Achievement:

- Detected columns with missing data in the forms of NaN, float and string types

- Hilighted cells with missing data

- Handled missing data by techniques: Omission (deleting), Imputation (replacing) and Boxplot

## Exploratory Analysis:

### Load the libraries
```
import warnings
import json
import sys
import csv
import os

import pandas as pd

import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns

import scipy
import numpy as np
np.random.seed(1612)
```

