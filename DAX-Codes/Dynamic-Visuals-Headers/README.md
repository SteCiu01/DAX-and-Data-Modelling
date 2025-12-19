# Dynamic Visuals Headers or Cards

#### Template for creating dynamic visuals headers or cards that display all the selected items, or a fix sentence in case all or nothing is selected.

```
Dynamic Header = 

IF(
    ISFILTERED(
        'Table'[Country]
    ),
    CONCATENATEX(VALUES('Table'[Country]),'Table'[Country], " | ") &  " Indexes",
    "World-Wide Country Indexes"
)
```
If all or nothing is selected (image below)

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/60e4323d-9a3a-4b5b-9090-0230b10af396" />

When only few items are selected (image below)

<img width="60%" alt="image" src="https://github.com/user-attachments/assets/263e163e-31bf-4e76-87d8-cdc2ffa82d8b" />

Of course this measure is a starting point but the logic could be further enriched, depending on the use case (e.g., limiting the number of concatenated item)
