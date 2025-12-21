# Dynamic Top-N Selection

**Requirements**

- You have a visual (bar chart) with on the y-axis the column [Customer Name] and on the x-axis the measure [Total Sales]
- You need to build a slicer where you can type in a number (e.g., 10) and this slicer would make you show in your visual the Top-10 customers by sales

**How to set it up?**

**Step 1: create a numeric parameter**

For example you can create a parameter that lets users to input from 1 to 50

```
Parameter Top-N = GENERATESERIES(1, 50, 1)
```

Then you can set up the default value in the parameter as follow:

```
Parameter Top-N Value = 
SELECTEDVALUE('Parameter Top-N'[3.0 - Parameter Top-N], 10)
```

**Step 2: create the control measure to use as filter on the bar chart visual**

```
TopN_Customers_Filter = 
VAR TopNValue = 
    SELECTEDVALUE('Parameter_TopN'[Top-N Value])

// The virtual table needs to replicate the hierarchical categorical structure of the visual for which you want to set up the Top-N filtering.

VAR _virt_table =
    CALCULATETABLE(
        SUMMARIZECOLUMNS(
            'Customers'[Customer Name],
            "Sales_Amount", [Total_Sales]
        ),

        /* ALLSELECTED removes the filter context created by Customer Name being on the visual axis
            Without this, each row would only see itself and every customer would be "Top 1 of 1"
            This allows the measure to evaluate ALL customers to determine the true TopN */

        ALLSELECTED('Customers'[Customer Name])
    )
 
VAR TopN_Table =
    TOPN(
        TopNValue,
        _virt_table,
        [Sales_Amount],
        DESC
    )
 
RETURN
    IF(
        SELECTEDVALUE('Customers'[Customer Name]) IN 
        SELECTCOLUMNS(TopN_Table, [Customer Name]),
        1,
        0
    )
```

**Step 3: finalisation**

1. Use the control measure in the bar chart visual filters and set it as **"IS 1"**
2. In a slicer use the ```Parameter Top-N'[3.0 - Parameter Top-N]``` from the parameter.
3. In slicer format > slicer settings > options-style set "Single Vlaue"

Now you can input in the slicer a Top N number, from 1 to 50 and see in the bar chart the top N customers with their amount of Total Sales.

