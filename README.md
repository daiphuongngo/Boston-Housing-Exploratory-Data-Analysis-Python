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

### Part A: Data Exploration

### Load the dataset
```
housing_df = pd.read_csv('/content/drive/My Drive/Colab Notebooks/Projects/Boston/BostonHousing.csv') 
```
### Dimension of the dataframe
```
housing_df.shape 
```
### Check the type of all columns
```
housing_df.dtypes
```
### Print the first 20 rows
```
housing_df.head(20) 
```

```
print(housing_df)
```
### Print dataframe's description
```
print(housing_df.describe())
```

### Check datatypes of each columns
```
housing_df.dtypes
```

As the later steps will show that INDUS, NOX, DIS, PTRATIO are the 4 main columns having NaN and missing values. So I will apply techniques to make sure their types are back to float.

### Statistics of NaN values by columns

This hasn't show the other missing values yet. It will be shown later.

```
housing_df.isnull().sum()
```

### Highlight the existing NaN values
```
housing_df.style.highlight_null('yellow')
```
```
housing_df[['INDUS', 'NOX', 'DIS', 'PTRATIO']].values[4][0]
```
```
housing_df[['INDUS', 'NOX', 'DIS', 'PTRATIO']].values[5][0]
```
```
type(housing_df[['INDUS', 'NOX', 'DIS', 'PTRATIO']].values[4][0])
```
```
type(housing_df[['INDUS', 'NOX', 'DIS', 'PTRATIO']].values[5][0])
```

### Highlight both Missing Data and NaN

The above code line is not efficient to highlight both Missing values and NaN values, but only NaN in Yellow. So we wrote a function to highlight Missing values in Dark Orange and NaN values in Yellow.

As we could see above that the columns of 'INDUS', 'NOX', 'DIS', 'PTRATIO' should have had 3 types: string, float and nan instead of only string as exemplified earlier. We would use while loop to try cast the cell value with float type. If error happens, that cell will be highlighted in dark orange. Another if condition will detect NaN values for each cell. If true, it will be highlighted in yellow. Otherwise, the normal data values will be kept as usual.

```
import math
def highlight_missing_and_nan(cell_value):

    highlight_missing_values = 'background-color: darkorange;'
    highlight_nan = 'background-color: yellow;'
    default = ''
    
    while True:
        try:
            cell_value = float(cell_value)
            break
        except ValueError:
            return highlight_missing_values    
    if math.isnan(cell_value) == True:
        return highlight_nan
    return default

housing_df.style.applymap(highlight_missing_and_nan)
#housing_df[['INDUS', 'NOX', 'DIS', 'PTRATIO']].style.applymap(highlight_missing_and_nan)
```



## Part B: Handling the columns with missing data (NaN and string in numerical columns)

3 columns: INDUS, NOX, DIS have missing values and NaN. I will handle each column.

These columns have NaN on all rows. I will delete them first: Unnamed: 11	
Unnamed: 12	
Unnamed: 13	
Unnamed: 14	
Unnamed: 15	
Unnamed: 16	
Unnamed: 17	
Unnamed: 18

### B.1. Omission: Deleting columns with all NaN values

```
housing_df = housing_df.drop(housing_df.columns[[11, 12, 13, 14, 15, 16, 17, 18]], axis=1)
print(housing_df)
```
#### Replace missing values with NaN for the column INDUS

```
housing_df['INDUS'].unique().tolist()
```
I detected that the column INDUS got missing values including: nana, "****", "*****", "Sara". I will leave the exisiting NaN values there and replace these mentioned 3 strings into NaN values.

#### Using median to replace NaN

```
housing_df.INDUS.isnull()
```

Indices of column INDUS's rows having NaN

```
housing_df.loc[housing_df.INDUS.isnull(), 'INDUS'] 
```

```
housing_df['INDUS'] = housing_df['INDUS'].replace(['****', '*****', 'Sara'], np.nan)
```

```
median_INDUS = housing_df['INDUS'].median()
median_INDUS
```

```
housing_df['INDUS'].fillna(housing_df['INDUS'].median(), inplace=True)
```

```
housing_df.head(167)
```

### B.2. Imputation: Replacing

Replacing string data in the numerical columns with NaN & Replacing NaN with median values of each column. These are Outliers due to typing `non-numeric` values.

#### Handle missing values with NaN for the column NOX

```
housing_df['NOX'].unique().tolist()
```

I detected that the column NOX got missing values including: nan, '*****', '&&&'. I will leave the exisiting NaN values there and replace these mentioned 2 strings into NaN values.

```
housing_df.loc[housing_df.NOX.isnull(), 'NOX'] 
```

```
housing_df['NOX'] = housing_df['NOX'].replace(['*****', '&&&'], np.nan)
```

```
housing_df.loc[housing_df.NOX.isnull(), 'NOX'] 
```

```
median_NOX = housing_df['NOX'].median()
median_NOX
```

```
housing_df['NOX'].fillna(housing_df['NOX'].median(), inplace=True)
```

```
housing_df.head(167)
```

#### Handle missing values with NaN for the column DIS

```
housing_df['DIS'].unique().tolist()
```

I detected that the column DIS got missing values including: nan, ' '. I will leave the exisiting NaN values there and replace this mentioned 1 string with NaN value.

```
housing_df.loc[housing_df.DIS.isnull(), 'DIS'] 
```

```
housing_df['DIS'] = housing_df['DIS'].replace([' '], np.nan)
```
```
housing_df.loc[housing_df.DIS.isnull(), 'DIS'] 
```

```
median_DIS = housing_df['DIS'].median()
median_DIS
```

```
housing_df['DIS'].fillna(housing_df['DIS'].median(), inplace=True)
```

```
housing_df.head(167)
```

#### Handle missing values with NaN for the column PTRATIO

##### String Outliers
```
housing_df['PTRATIO'].unique().tolist()
```
I detected that the column PTRATIO got missing values including: 'Alina', '##', 'Adam'. I will replace these mentioned 3 strings with NaN value.

```
housing_df['PTRATIO'] = housing_df['PTRATIO'].replace(['Alina', '##', 'Adam'], np.nan)
```

```
housing_df.loc[housing_df.PTRATIO.isnull(), 'PTRATIO'] 
```

```
median_PTRATIO = housing_df['PTRATIO'].median()
median_PTRATIO
```
```
housing_df['PTRATIO'].fillna(housing_df['PTRATIO'].median(), inplace=True)
```
```
housing_df.head(167)
```

##### Numeric Outliers of PTRATIO

This part is about genuine cases of outliers of PTRATIO that I will print them all out after defining a certain distance of find_boundaries().

##### Boxplot
```
plt.figure(figsize=(20,10))
sns.boxplot(data=housing_df['PTRATIO'],orient = 'h',)
```
![download (2)](https://user-images.githubusercontent.com/70437668/150657202-ca48d5da-c36e-4242-88c2-7d65a8d6f913.png)

##### Convert PTRATIO types of string and numeric into float
```
housing_df['PTRATIO'] = pd.to_numeric(housing_df['PTRATIO'], downcast="float")
```

##### Unique values
```
housing_df['PTRATIO'].unique()
```

##### Find boundaries
```
def find_boundaries(df, variable, distance):

    IQR = housing_df['PTRATIO'].quantile(0.75) - housing_df['PTRATIO'].quantile(0.25)

    lower_boundary = housing_df['PTRATIO'].quantile(0.25) - (IQR * distance)
    upper_boundary = housing_df['PTRATIO'].quantile(0.75) + (IQR * distance)

    return upper_boundary, lower_boundary
```
```
upper_boundary, lower_boundary = find_boundaries(housing_df, 'PTRATIO', 0.5)
upper_boundary, lower_boundary
```
Output
```
(22.449999809265137, 16.249999046325684)
```

```
outliers = np.where(housing_df['PTRATIO'] > upper_boundary, True,
            np.where(housing_df['PTRATIO'] < lower_boundary, True, False))
```

```
outliers_df = housing_df.loc[outliers, 'PTRATIO']
outliers_df.head(100)
```

```
outliers_df2 = outliers_df.to_frame()
outliers_df2
```

```
outliers_list = outliers_df2["PTRATIO"].tolist()
outliers_list
```

Output
```
[15.300000190734863,
 137.0,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 0.23000000417232513,
 44.0,
 15.199999809265137,
 46.0,
 47.0,
 2.109999895095825,
 15.100000381469727,
 16.100000381469727,
 16.100000381469727,
 177.0,
 14.699999809265137,
 14.699999809265137,
 51.29999923706055,
 50.29999923706055,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137,
 15.199999809265137]
```

```
print("Numer of outliers of PTRATIO with distance=0.5: ", len(outliers_list))
```

Numer of outliers of PTRATIO with distance=0.5:  29

##### Higlighting numeric Outliers of PTRATIO in housing_df with conditional formatting

Less explicitly, to work with a DataFrame rather than Series wrap the column names in another set of brackets e.g. df[['a']] instead of df['a'].
```
def color_outliers_red(val):
    color = 'red' if val < lower_boundary or val > upper_boundary else 'black'
    return 'color: %s' % color

housing_df[['PTRATIO']].style.applymap(color_outliers_red)
```

## Conclusion

I handled the missing data of INDUS, NOX, DIS by replacing string-typed missing data with NaN values. Then all exising and newly created NaN values were replaced with median values of each column. 

I deleted all the Unamed columns with all 167 NaN rows per column.

I also highlited in yellow for these 3 columns and the Unamed columns.

For the PTRATIO column, there were no NaN values but missing data so we replaced them with this column's median value. The missing values here are the first type of outliers as non-numeric value that we detected in Part A.2.(a). Then I plotted a Box Plot to see all possible outliers. After that, I wrote a function to define upper and lower boundaries for this column with distance = 0.5. This distance can be changed if necessary but I kept it at 0.5. I found 29 outiers with this distance and highlighted them on the smaller dataframe housing_df['PTRATIO'].
