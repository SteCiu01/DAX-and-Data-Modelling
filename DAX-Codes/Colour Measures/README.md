# Quick Colour Measures

Here below some standard colour measures that can be used to have easy access to colours when building custom formatting logics.

With the code below you can create them in bulk in the DAX query view. You just need to amend them to your colour needs. A piece of advice: always keep a transparent colour in your model as it can come handy.

```
DEFINE

MEASURE 'Measures Table'[ColorTransparent] = "#FFFFFF00"

MEASURE 'Measures Table'[ColorBlack] = "#333333"

MEASURE 'Measures Table'[ColorWhite] = "#f2f2f2"

MEASURE 'Measures Table'[ColorGrey] = "#B3B3B3"

MEASURE 'Measures Table'[ColorBlue] = "#86C4E6"
```
