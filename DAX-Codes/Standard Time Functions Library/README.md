# Standard Time Functions Library

Here below some pre-set time intelligence functions already based on the calendar table of the previous topic.

They are from the DAX Query View and they can easily amended and imported in a model.

```
MEASURE 'Measures Tables'[Amount Sold YTD] = TOTALYTD([Amount Sold $], dates_table[Date Column])

MEASURE 'Measures Tables'[Amount Sold SPLY] = 
CALCULATE(
    [Amount Sold $],
    SAMEPERIODLASTYEAR(dates_table[Date Column])
)

MEASURE 'Measures Tables'[Amount Sold SPLY DATEADD] = 
CALCULATE(
    [Amount Sold $],
    DATEADD(dates_table[Date Column], -12, MONTH)
)

MEASURE 'Measures Tables'[Amount Sold Past Quarter] = 
CALCULATE(
    [Amount Sold $],
    DATEADD(dates_table[Date Column], -1, QUARTER)
)

MEASURE 'Measures Tables'[Amount Sold Past Month] = 
CALCULATE(
    [Amount Sold $],
    DATEADD(dates_table[Date Column], -1, MONTH)
)
```
