# Distinct Combination Counting via Virtual Tables

Resolve over-granularity issues by generating virtual tables at the desired level to count distinct item combinations.

Let'simagine we have the facts_sales_table as below:

| customer_id | service_id | configuration_id |
|----|---------|---------------|
| 1  | a       | xy            |
| 1  | a       | xz            |
| 1  | b       | yt            |
| 1  | b       | yf            |
| 2  | a       | xz            |
| 2  | c       | pq            |
| 2  | c       | pr            |
| 3  | a       | xy            |
| 3  | b       | yf            |
| 3  | c       | pr            |

This situation shows that a customer can purchase different services and each service has some configuarations as well. 

The management considers 1 opportunity as the combination of customer - service. So for instance, in the table above the customer 1 generates 2 ops: a and b, the customer 2 generates also 2 ops: a and c and finally customer 3 generates 3 ops: a, b and c. The total number of what is considered an oportunity is 7. 

Here virtual tables come in handy as they let you create the "ops ids" and then count them. 

Here how to proceed:

```
Opportunities =

VAR virtTable =
  SUMMARIZE(
        facts_sales_table,
        FactTable[customer_id],
        FactTable[service_id]
    )

RETURN
COUNTROWS(
   virtTable 
)
```
