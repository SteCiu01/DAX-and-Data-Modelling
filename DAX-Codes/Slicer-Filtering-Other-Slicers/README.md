# Make Slicers Filter Each Other Based on Available Data

### Basic Situation: in the data model you have a fact table, and several dim tables

**Data Model**

dim_customers_details (1) ---> (*) facts_sales

dim_products_details (1) ---> (*) fact_sales

Now let's imagine in the slicer that uses customer_name column from dim_customers_details you select customer A, that buys only products A, B, C. If you try to use the slicer that contains the column product_name, you will also see products D, E, F, etc... but, when you click on one of them your visuals will return blank, since the fact table has been already filtered for customer A and there are no sales for customer A referring any product but A, B and C.

This can be very annoying for users, but it is avoidable using this measure: 

```
Filter Other Slicer = COUNTROWS(RELATEDTABLE(fact_sales))
```

Then, this measure needs to be dropped in the visual filters of each slicer, and the filter must be set as: **"is greater than or equal to" 1**

### Advanced Situation: in the data model you have 2 or more fact tables, and several dim tables

**Data Model**

dim_customers_details (1) ---> (*) facts_sales

dim_products_details (1) ---> (*) fact_sales

dim_customers_details (1) ---> (*) facts_returns

dim_products_details (1) ---> (*) fact_retuns

Now, if you select customer A, you want to see in the other slicer the products that have either sales or returns only for that customer, thus the measure needs to be expanded as follows:

```
Filter Other Slicer = COUNTROWS(RELATEDTABLE(fact_sales)) + COUNTROWS(RELATEDTABLE(fact_returns))
```

This way the measure is expandable for as many fact tables as you have.
