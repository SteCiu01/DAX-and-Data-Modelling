# Conditional Colour Based on Slicer Selection

### Simple Scenarios

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
            [ColorBlue],
            [ColorGrey]
  )
```
