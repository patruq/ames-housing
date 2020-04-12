# Ames, Iowa Housing Sale-Price Predictions

### The Problem: What feature set is most important in creating a model that predicts housing sale-prices with minimal errors in it’s predictions?

-------------------------------------------------

***Executive Summary***

Contents:

- <b>Notes on Data & Import</b>

  - Imported data from both the `train.csv` (Train) and `test.csv` (Test) datasets. Verified data types, input values, column lengths, and overall integrity of the data set. Notes on the raw data files:
    - <b>Train:</b>
      - This was the dataset that our machine-learning model was trained on.
      - 81 original columns, 2051 rows.
        - - The 81st column of the Train dataset was the `sale_price` data that our model was tested on for the competition.
        - The first 80 columns were designated as the `predictor` variables.
        - The 81st column was the `target` variable.
      - Mix of type object (string), integers, and floats.
      - 9822 missing values across the entire dataset.
    - <b>Test:</b>
      - This was the dataset that our machine-learning model was tested on for the competition.
      - 80 original columns, 878 rows.
        - The Test dataset did not contain the target variable `sale_price`.
      - Mix of type object (string), integers, and floats.
      - 4171 missing values across the entire dataset.

- <b>Data Cleaning</b>
  - Due to the large number of missing values (NaN) in some columns, some NaN values were imputed using custom Python functions.
    - Example:
      - For all columns relating to both `garage` and `bsmt` (basement) related data, we were able to create an automated process to check the location of a given NaN value in a column, compare the location of that NaN in the dataframe against the value at the index location of another related column, and if that value was missing as well, the function could determine what to impute the NaN to based on the required data type (dtype) of the column.
      - In columns containing a small number of NaN's (in some cases, a given column may only have been missing 1 value) the NaN values were dropped from the dataframe.

- <b>Exploratory Data Analysis</b>

    - Correlation matrices and heatmaps strongly indicated the relatively strong correlation of `gr_liv_area` and `overall_cond` of a house to its `sale_price`.
    - In the first iterations of our model, it was determined that, despite the strong correlation between just these two variables to `sale_price`, these variables did not prove to be sufficient on their to predict price.
    - Summary statistics were interpreted for data. Filters and Sorting were applied to the Train dataset to investigate relationships between metrics of the data. For example, we queried the data using conditional statements (i.e. looking for max values in a given column), to return values central to understanding relationships and trends.
      - Boxplots were used to visual the spread of a column and identify outliers in a given column that appeared to have a strong correlation.
    - ***Our key findings revealed in our exploration were***
-<b>Feature Engineering</b>
  - To incorporate more data points for our model, we converted certain nominal, categorical, or string-type columns to numerical values.
    - Binarized:
      - The `central_air` column contained values of either `Y` or `N`, and we used list comprehension to iterate through each row and convert the `Y` to `1` and `N` to `0`
      - For columns containing a small set of string descriptors for values, we created `dummies` columns to represent them numerically.
- Data Visualization

    - Distribution, scatter, and box plot charts were created to visualize findings of the data exploration.

-------------------------------------------------
### <b>Data Dictionary of Included Variables in Model Feature Set</b>:

|Feature|Type|Dataset|Description|
|---|---|---|---|
|`garage_cond`|`dummified` object|`train` and `test`|Rates the condition of the garage with values `Ex`, `Gd`, `TA`, `fa`, `Po`, 'NA'|
|`bsmt_cond`|`dummified` object|`train` and `test`|Rates the condition of the basement with values `Ex`, `Gd`, `TA`, `fa`, `Po`, `NA`|
|'condition_1'|`dummified` object|`train` and `test`|Proximity to some condition|
|`central_air_dummy`|`dummified` object|`train' and `test`|`Y` or `N` if the unit has central air|
|`overall_qual`|integer|`train` and `test`|Rating of the quality of the material and finish of the house|
|`gr_liv_area`|integer|`train` and `test`|Square footage of living area above ground|
|`1st_flr_sf`|integer|`train` and `test`|1st floor square footage|
|`parcel_age`|integer|`train` and `test`|Age of the unit|
|`age_remod_add`|integer|`train` and `test`|Age of any house additions or remodeling|

-------------------------------------------------
### ***Conclusions***

- Creating and including feature columns that calculate the age of the unit appear to have helped make our prediction model more accurate.
  - Age, in particular, has a negative correlation to sale-price (as houses get older, their sale value decreases)
- Our Model suggests that location <i>is</i> an important factor in determining price, alongside quality and age of the unit. Location to external factors (like highways or parks) are relevant.


### ***Additional Info Desired***
- The buying and selling of a home often involves an exchanging of `offerings` among interested buyers to the seller of a home. We may find it useful for future model building to have data on the initial vs. iterative vs. closing offers made on a given home, and whether initial pricing determinations affect the final sale price of the home.
