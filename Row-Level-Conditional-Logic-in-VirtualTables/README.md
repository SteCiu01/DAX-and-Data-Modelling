# Leverage in-measures virtual tables for complex calculations

Let's imagine you work in finance and you need to present in a matrix the following customers hierarchy:

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
