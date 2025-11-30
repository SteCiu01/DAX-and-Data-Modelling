# Leverage in-code virtual tables for complex row-level calculations

Let's imagine you need to present in a matrix the following customers hierarchy:
- Parent ID
- Child ID
- Customer Name
- Currency

<img width="25%" alt="image" src="https://github.com/user-attachments/assets/4fb0e87b-43f4-4270-93d4-a6811c6b8a90" />

Then you have 2 simple measures to add: 

```
Total Position = SUM(MyCompanyPositions[Position_USD])
```

```
Total Position Old = 
CALCULATE(
                [Total Position], 
                MyCompanyPositions[Position_Age] IN {"Old", "Very Old"}
            )
```

This is the result: 

<img width="40%" alt="image" src="https://github.com/user-attachments/assets/674903e6-cf5f-4ead-96fd-863f8b8e4969" />

In this situation, if we drill up or drill down the hierarchy the measures would automatically adapt and the numbers would always be the desiredd ones.

Now let's immagine you need to create a measure for what the team considers NET Old Positions and the logic would be:

```
Logic =
    IF(
        [Total Position] < 0, BLANK(),
        IF(
            [Total Position Old] < 0, BLANK(),
            IF(
                [Total Position Old] > [Total Position], [Total Position],
                [Total Position Old]
            )
        )
    )
```
However, **the requirement in this scenario is that the logic needs to be evaluated for each customer at currency level.** 

Using the measure **Logic** above would not work when drilling up or down the hierarchy, as it would be evaluated each time for the lower available level of the hierarchy (currency, child id, etc.). 

Therefore, there is the need to create a virtual table for that, where the logic depends and it's locked on the grain of the virtual table, not the filter context. DAX measures normally work at the group level after filters, but the logic here needs to work:
- Before aggregation
- At a controlled grain
- Using a synthesised dataset

The correct measure to use is: 

```
Net Old Position (Correct) = 

VAR VirtTable =
SUMMARIZE(
    MyCompanyPositions,
    MyCompanyPositions[Parent_ID],
    MyCompanyPositions[Child_ID],
    MyCompanyPositions[Customer_Name],
    MyCompanyPositions[Currency],
    "Logic",
    IF(
        [Total Position] < 0, BLANK(),
        IF(
            [Total Position Old] < 0, BLANK(),
            IF(
                [Total Position Old] > [Total Position], [Total Position],
                [Total Position Old]
            )
        )
    )
)

VAR NetOldVeryOld =
SUMX(
    VirtTable,
    [Logic]
)
        
RETURN
NetOldVeryOld
```

Let's see the difference between Logic and Net Old Position (Correct) in the two images below: 

Image 1: currency level shown

<img width="55%" alt="image" src="https://github.com/user-attachments/assets/f734a257-963d-4032-8c39-bcea49defab2" />

Image 2: currency level not shown, matrix rolled up at child id level

<img width="55%" alt="image" src="https://github.com/user-attachments/assets/919c3eeb-b159-41c7-ac2f-776535ec0cae" />

If until the currency level is there the measure Logic works as intended (although no totals are displayed), when we roll up at child id level it does not return the intended number, while the Old Position (Correct) does. 

This because the latter still calculates the net position at currency level and then sums up these positions for each level of the hierarchy. 

