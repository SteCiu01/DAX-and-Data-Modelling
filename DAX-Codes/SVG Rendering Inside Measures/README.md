# SVG Rendering Inside Measures

Embed SVG images directly in DAX measures to enrich visuals with custom icons and indicators.

### Step 1: define 2 or more measures in your model where you bring in the images you need as SVG

For instance we we can create the SVG measures for arrows (Up and Down)

```
Arrow_Up = "data:image/png;base64,[your svg code]"
```
```
Arrow_Down = "data:image/png;base64,[your svg code]"
```

### Step 2: Create all the other measures that reference to your SVG measures

For instance we can compare Sales of the current month vs. sales of the past month and generate an arrow image to display in a new KPI card visual.

```
Sales_Comparison_Arrows = 

VAR _Sales_PM = [Sales_PM]

VAR _Sales_CM = [Sales_CM]

VAR Differential =
_Sales_CM - _Sales_PM

VAR Arrows = 
IF(
  VALUE(Differential) > 0, 
  [Arrow_UP],
    IF(
      VALUE(Differential) < 0, 
      [Arrow_Down], 
      BLANK()
      )
)

RETURN Arrows
```
