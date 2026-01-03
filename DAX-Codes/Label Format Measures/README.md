# Label Format Measures

Technique to gain maximum customization and flexibility for what displayed in data labels, getting rid of the Auto, Thousands, Million, etc.. default settings.

### Situation: 

<hr>

We have a measure selector parameter:

```
IBCS-Column-Chart-Measures-Selector =
DATATABLE (
    "Measure", STRING,
    "Order", INTEGER,
    {
        { "Total Sales ($)", 0 },
        { "Total Qty Sold", 1 }
    }
)
```
Which in the model is the table below: 

IBCS-Column-Chart-Measures-Selector Table
| Measure | Order |
| ---- | ---- |
| Total Sales ($) | 0 |
| Total Qty Sold | 1 |

Then in a visual we have the measure: 

```
Actual = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) = 0, [Total Sales $],
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) = 1, [Total Sales Qty]
)
```

When using the column 'Measure' from the parameter table in a slicer we can call either Total Sales $ or Total Sales Qty, however, one measure needs to be formattedd as whole number and the other one as currency. 

### Two Possible Solutions

<hr>

#### 🔣 Option 1: Dynamic Formatting

<img width="1040" height="219" alt="image" src="https://github.com/user-attachments/assets/6ca589ea-2d66-48a4-b4d3-7b2a10f84177" />

In the measure formatting options, choose dynamic. This let's you to write a conditional formatting for the measure, based on the type of measure you are calling using the parameter.

Here the FORMAT code:

```
SWITCH(
    TRUE(),
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) = 0, "\$#,0;(\$#,0);\$#,0" ,
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) = 1, "0"
)
```

Guidelines: 
- Considering what measure corresponds to Order 0, 1, or 2, etc.. in your measures selector, you need to adapt this code to your use case so that if Order = 0 is a currency and Order = 1 is a decimal value, you amend the code accordingly.
- In case you have other measures that need to be formatted as currency or as whole number you can reference the code below

```
SWITCH(
    TRUE(),
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) IN {0,2} "\$#,0;(\$#,0);\$#,0" ,
    SELECTEDVALUE('IBCS-Column-Chart-Measures-Selector'[Order]) IN {1,3} "0"
)
```

After you set up the dynamic formatting go to - Visual Format > Data Label > Value - here you can set up Auto, or thousands, or none, etc. as well as the number of decimal places you want to display. 

However, in some instances the Auto might not work properly or it will show you an Auto that you don't like. This is why the Labels Format Measures (option 2 below) is the one that offers greater customization and flexibility.

#### 🔡 Option 2: Labels Format Measures

Here below the Labels Format measures to create.

This measure provide a consistent formatting depending on the value, for both currency and decimal/whole number values.

```
Actual_Format = 
VAR _Order =
    SELECTEDVALUE ( 'IBCS-Column-Chart-Measures-Selector'[Order] )

VAR _Value =
    [Actual]

VAR AbsValue =
    ABS ( _Value )

VAR IsCurrency =
    _Order IN {0} -- Add all the order numbers referring to measure that need to be formatted with currency

RETURN
SWITCH (
    TRUE(),
    -- TRILLIONS
    AbsValue >= 1000000000000,
        FORMAT (
            _Value / 1000000000000,
            IF (
                IsCurrency,
                "$0.0""T"";($0.0""T"")",
                "0.0""T"";(0.0""T"")"
            )
        ),
    -- BILLIONS
    AbsValue >= 1000000000,
        FORMAT (
            _Value / 1000000000,
            IF (
                IsCurrency,
                "$0.0""B"";($0.0""B"")",
                "0.0""B"";(0.0""B"")"
            )
        ),
    -- MILLIONS
    AbsValue >= 1000000,
        FORMAT (
            _Value / 1000000,
            IF (
                IsCurrency,
                "$0.0""M"";($0.0""M"")",
                "0.0""M"";(0.0""M"")"
            )
        ),
    -- THOUSANDS
    AbsValue >= 1000,
        FORMAT (
            _Value / 1000,
            IF (
                IsCurrency,
                "$0.0""K"";($0.0""K"")",
                "0.0""K"";(0.0""K"")"
            )
        ),
    -- BELOW 1K
    FORMAT (
        _Value,
        IF (
            IsCurrency,
            "$#,0;($#,0)",
            "#,0;(#,0)"
        )
    )
)
```

Note: for the variable ```VAR IsCurrency = _Order IN {0}``` add all the order numbers referring to measure that need to be formatted with currency.

After that go to: - Visual Format > Data Label > Value - here you can switch the Value Field with this measure.

Output:

<img width="125" height="162" alt="image" src="https://github.com/user-attachments/assets/b8a292dd-42e6-417e-a673-600c469c0360" />

You can have both millions and thousands for the same measure. This is very flexible in situations where you have very big differences. 

For instance imagine one month you have 40K and all the others are on millions scale, and you set up decimal 1. 

You would have something like: 2.4M, 1.2M, etc., then: 0.0M - with this new formatting measure you would have: 2.4M, 1.2M, 40.2K, etc., ...
