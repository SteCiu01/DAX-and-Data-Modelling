# Conditional Colour based on slicer selection

## Simple Scenarios

### Hide/unhide a button or an object

```
Transparent_or_Blue =
IF(
  ISFILTERED(
            Dim_Table[ColumnName]
            ),
            [ColorBlue],
            [ColorTransparent]
  )
```
### Color based on a category selection

```
Transparent_or_Blue =
IF(
  SELECTEDVALUE(Dim_Table[ColumnName]) = "Category_A",
            [ColorGrey],
            [ColorTransparent]
  )
```

## Complex Full Scenario

Starting from this bar chart

<img width="50%" alt="image" src="https://github.com/user-attachments/assets/e7e2e4d8-03ff-433b-8894-4136f6b99f7c" />

First, Add the colors you need in DAX query view: 

```
DEFINE

MEASURE 'Measures Table'[ColorTransparent] = "#FFFFFF00"

MEASURE 'Measures Table'[ColorRed] = "#FF0000"

MEASURE 'Measures Table'[ColorGreen] = "#008000"

MEASURE 'Measures Table'[ColorGrey] = "#D9D9D9"
```

