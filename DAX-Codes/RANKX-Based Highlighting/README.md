# RANKX-Based Highlighting

Use RANKX-based logic to highlight Top-N or Bottom-N categories via conditional formatting

Starting from this bar chart

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/e7e2e4d8-03ff-433b-8894-4136f6b99f7c" />

We want to have highlighted in green the top 2 categories and in red the bottom 2, by Total Sales.

First, Add the colors you need in DAX query view: 

```
DEFINE

MEASURE 'Measures Table'[ColorTransparent] = "#FFFFFF00"

MEASURE 'Measures Table'[ColorRed] = "#FF0000"

MEASURE 'Measures Table'[ColorGreen] = "#008000"

MEASURE 'Measures Table'[ColorGrey] = "#D9D9D9"
```

Then, create the formatting measure:

```
Bar Chart Formatting = 

VAR _TopN = 2

VAR _BottomN = 2

VAR BarChartSetUpDesc =
RANKX(
    CALCULATETABLE(
        SUMMARIZE(
                'Table',
                'Table'[Category]
                ),
        ALLSELECTED('Table'[Category]) -- remove in-visual row level filtering
    ),
        [Total Sales],
        ,
        DESC
    )

VAR TotalCategories = 
CALCULATE(
    DISTINCTCOUNT('Table'[Category]),
    ALLSELECTED('Table'[Category]) -- remove in-visual row level filtering
)

VAR _Top = BarChartSetUpDesc <= _TopN

VAR _Bottom = BarChartSetUpDesc > TotalCategories - _BottomN

RETURN
    SWITCH(
        TRUE(),
        _Top, [ColorGreen],
        _Bottom, [ColorRed],
        [ColorGrey]
    )
```

Finally, locate the measure in the conditional formatting option of the Bar Chart > Format > Color section

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/6bafb380-b752-4925-9075-862a1d1e8030" />

The result below:

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/f0bfe9fc-4269-4197-9468-e93fda4dd9e0" />

It dynamically adapts to the Category filter:

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/86e9d75a-e998-470d-88e3-cf2559c35b6d" />


