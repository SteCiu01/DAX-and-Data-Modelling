# Traffic Light Conditional Target Markers

Create a measure to display in tables or matrix visuals traffic light style markers based on a measure's value.

Pro-Tip: Use ISINSCOPE for limiting the visualization to only desired rows (e.g., exclding totals)

```
Sales Target = 
IF(
    ISINSCOPE(
        dim_products[Product_Name]
    ),
        SWITCH(
            TRUE() ,
            [Total Units Sold] >= 600, "🟢",
            [Total Units Sold] < 350, "🔴",
            "🟠"
        ),
    BLANK()
)
```
