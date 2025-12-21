# Filter a visual by the amount range of a measure

With a DAX control measure is possible to filter an amount range using slicers, in a Power BI report.

## Situation

Have you ever had a situation where you have a visual (e.g., a Matrix) with some categorical data (customer hierarchy) and a value (customer’s position), and had to dynamically filter only for those customers with a position within a range? 

This is possible using the Power BI default filter pane; however, it is not so immediate if you want to do this using slicers. Here, we are going to see how to quickly set this up.

## 0 - Starting point

<img width="50%"  alt="image" src="https://github.com/user-attachments/assets/ad264200-3bc1-47d5-9009-d0abd652fd06" />
<br><br>

In the image above, there is our visual, and we want to set up a slicer that will let users decide which customers (at parent level) they will keep in the visual based on a range of the Customer Total Position of their choice. 

For example, if a user decides to keep only customers with a positive position at parent level we would want to display in the visual only customers with Parent_IDs P4 and P1.

## 1 - Create 2 Tables (not connected to the data model) to use in the Slicers

**MinAmount table:**

<img width="75%"  alt="image" src="https://github.com/user-attachments/assets/d3ef2480-4a83-42f6-a794-49bf2c051fe8" />

**MaxAmount table:**

<img width="75%" alt="image" src="https://github.com/user-attachments/assets/e699d2e1-be35-432c-9239-165366f6bec8" />

Set these two tables so that you have all the possible ranges From-To that you would need for your filtering. 

Also, set two big amounts for the extremes so that “all negatives” or “all positives” selection would include all possible outcomes for your measure. 

Do not forget to create a rank column that you will need to use to sort the “From” and “To” columns by this rank column. This rank should have the “1” number corresponding to the max positive amount of columns “MinAmount” and “MaxAmount.”

<img width="75%"  alt="image" src="https://github.com/user-attachments/assets/ecbb8a62-d830-457c-93f6-defc4a0c077b" />

## 2 - Set Up the Control Measure

With this measure, you need to reproduce the same setup of the visual that you want to filter but in this specific case, we are setting it up to work at the Parent ID level, as the goal is to filter based on the Total Customer Position for that level. Additional comments are provided within the measure.

```
FilterByAmount = 

VAR Maxinput = SELECTEDVALUE('MaxAmount'[MaxAmount])
VAR MinInput = SELECTEDVALUE(MinAmount[MinAmount])

VAR PARENT_TotalPosition = 
CALCULATETABLE(
    FILTER(
        SUMMARIZE(
            MyCompanyPositions,
            MyCompanyPositions[Parent_ID],
            "PARENT_TO_USE", SELECTEDVALUE(MyCompanyPositions[Parent_ID]),
            "TotalPosition", SUM(MyCompanyPositions[Position_USD])
        ),

        [TotalPosition] >= MinInput && [TotalPosition] <= MaxInput
        ),

    // we need to remove CHILD_ID from being filtered in the visual to properly filter based on parent level (as this is the lower level of hierarchy in the matrix)
    // in case we want to be able to choose the range for the child level we need to remove this ALLSELECTED.
    ALLSELECTED(MyCompanyPositions[Child_ID]) 
)

VAR PARENT_List = // create the list with selectcolumns function
    SELECTCOLUMNS(
        PARENT_TotalPosition,
        "PARENT", [PARENT_TO_USE]
    )

VAR PARENT_Filtering = // give to the IDs in the list value of 1 (later in the matrix filters, we keep only what is 1)
    IF(
        SELECTEDVALUE(MyCompanyPositions[Parent_ID]) IN PARENT_List,
        1,
        0
    )

RETURN
PARENT_Filtering
```

## 3 - Locate the Measure in the Visual Filters and set visibility when it is 1

<img width="20%" alt="image" src="https://github.com/user-attachments/assets/477915f9-ee93-4718-8348-973ded976221" />

## 4 - Use the columns “From” and “To” from the MinAmount and MaxAmount tables in 2 slicers  

<img width="40%" alt="image" src="https://github.com/user-attachments/assets/314654c4-ecc7-44f3-adae-7c8c366994f7" />

Here, I would advise setting up the slicer so that it looks like one filter only, where the users can seamlessly select their range. 

Also, sort the columns in the slicers in “Ascending” order – this is why we made those operations with the ranking columns earlier.

<img width="40%" alt="image" src="https://github.com/user-attachments/assets/34ed40d5-36ed-4c36-9cb3-8d7e3d71b1d0" />

At this point, the whole filter by measure technique is done. We can now test it and see if it works.

## Let’s Test it!

Selecting the range [0, All Positive AMounts], only the P4 and P1 customers are visible in the visual (it worked!). 

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/9d10bfc7-feac-4193-b502-aaab2afc1e67" />

Note: this approach, with the control measure above, works well regardless of other slicers that are applied. In the image below, it is possible to see how the range is selected only for the positions in GBP.

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/03823325-85c0-4cc3-b53a-f82c8d6486ee" />

## Possible Enhancement

An enhancement that is possible to implement would be to let the user the possibility to decide which level of hierarchy to use for filtering the amount range. 

In our example, the range filter is applied only at the Parent_ID level. However, by adding a simple disconnected table (table below), we could create a button slicer next to the range filter that allows switching between hierarchy levels and applying the amount range filter accordingly.

Table name: hierarchy_control

| Hierarchical_Level | ID |
|---------|-------------|
| Parent_ID | 1 |
| Child_ID | 2 |

Of course the control measure would need to  be amended as follows, considering the new disconnected table:

```
FilterByAmount = 

VAR Maxinput = SELECTEDVALUE('MaxAmount'[MaxAmount])
VAR MinInput = SELECTEDVALUE(MinAmount[MinAmount])

VAR PARENT_TotalPosition =  
CALCULATETABLE(
    FILTER(
        SUMMARIZE(
            MyCompanyPositions,
            MyCompanyPositions[Parent_ID],
            "PARENT_TO_USE", SELECTEDVALUE(MyCompanyPositions[Parent_ID]),
            "TotalPosition", SUM(MyCompanyPositions[Position_USD])
        ),

        [TotalPosition] >= MinInput && [TotalPosition] <= MaxInput
        ),
    ALLSELECTED(MyCompanyPositions[Child_ID]) 
)

VAR PARENT_List =
    SELECTCOLUMNS(
        PARENT_TotalPosition,
        "PARENT", [PARENT_TO_USE]
    )

VAR PARENT_Filtering = 
    IF(
        SELECTEDVALUE(MyCompanyPositions[Parent_ID]) IN PARENT_List,
        1,
        0
    )

VAR CHILD_TotalPosition =
FILTER(
        SUMMARIZE(
            MyCompanyPositions,
            MyCompanyPositions[Parent_ID],
             MyCompanyPositions[Child_ID], -- added child id here
            "CHILD_TO_USE", SELECTEDVALUE(MyCompanyPositions[Child_ID]),
            "TotalPosition", SUM(MyCompanyPositions[Position_USD])
        ),

        [TotalPosition] >= MinInput && [TotalPosition] <= MaxInput
)

VAR CHILD_List = // create the list with selectcolumns function
    SELECTCOLUMNS(
        CHILD_TotalPosition,
        "CHILD", [CHILD_TO_USE]
    )

VAR CHILD_Filtering = // give to the IDs in the list value of 1 (later in the matrix filters, we keep only what is 1)
    IF(
        SELECTEDVALUE(MyCompanyPositions[CHILD_ID]) IN CHILD_List,
        1,
        0
    )


RETURN
IF(
    SELECTEDVALUE(hierarchy_control[ID]) = 1,
    PARENT_Filtering,
    CHILD_Filtering
)
```

