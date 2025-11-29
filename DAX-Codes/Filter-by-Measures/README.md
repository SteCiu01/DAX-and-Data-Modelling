# Filter by Measures: a simple and replicable DAX technique

With a simple control DAX measure is possible to filter an amount range using slicers, in a Power BI report.

## Situation

Have you ever had a situation where you have a visual (e.g., a Matrix) with some categorical data (customer hierarchy) and a value (customer’s position), and had to dynamically filter only for those customers with a position within a range? This is possible using the Power BI default filter pane; however, it is not so immediate if you want to do this using slicers. Here, we are going to see how to quickly set this up.

## 0 - Starting point

<img width="50%"  alt="image" src="https://github.com/user-attachments/assets/ad264200-3bc1-47d5-9009-d0abd652fd06" />
<br><br>

In the image above, there is our visual, and we want to set up a slicer that will let users decide which customers to keep in the visual based on a range of the Customer Total Position of their choice. For example, let’s imagine we want to keep only customers with a positive position; therefore, we would want to have in the visual only customers with Parent_IDs P4 and P1.

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

With this measure, you need to reproduce the same setup of the visual that you want to filter. In this specific case, we are rolling up at the Parent ID level, as the goal is to filter based on the Total Customer Position. Additional comments are provided within the measure.

```
FilterByAmount = 

VAR Maxinput = SELECTEDVALUE('MaxAmount'[MaxAmount])
VAR MinInput = SELECTEDVALUE(MinAmount[MinAmount])

VAR PARENT_TotalPosition =  // roll-up to parent level to identify parent's IDs to be included
CALCULATETABLE(
    FILTER(
        SUMMARIZE(
            MyCompanyPositions,
            MyCompanyPositions[Parent_ID],
            "PARENT_TO_USE", SELECTEDVALUE(MyCompanyPositions[Parent_ID]),
            "TotalPositionCID", SUM(MyCompanyPositions[Position_USD])
        ),

        [TotalPositionCID] >= MinInput && [TotalPositionCID] <= MaxInput
        ),

    // we need to remove CHILD_ID from being filtered in the visual to properly roll-up to parent level (as this is the lower level of hierarchy in the matrix)
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

<img width="30%" alt="image" src="https://github.com/user-attachments/assets/314654c4-ecc7-44f3-adae-7c8c366994f7" />

Here, I would advise setting up the slicer so that it looks like one filter only, where the users can seamlessly select their range. 

Also, sort the columns in the slicers in “Ascending” order – this is why we made those operations with the ranking columns earlier.

<img width="30%" alt="image" src="https://github.com/user-attachments/assets/34ed40d5-36ed-4c36-9cb3-8d7e3d71b1d0" />

At this point, the whole filter by measure technique is done. We can now test it and see if it works.

## Let’s Test it!

Selecting the range, only the P4 and P1 customers are visible in the visual (it worked!). Note: this approach, with the control measure above, works well regardless of other slicers that are applied. In the image below, it is possible to see how the range is selected only for the positions in GBP.

<img width="50%" alt="image" src="https://github.com/user-attachments/assets/bbb5aea9-d188-401b-97b2-dfd5e5dd4374" />



