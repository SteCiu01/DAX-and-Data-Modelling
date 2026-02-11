# Dimension Table Isolation Pattern

Use data modelling techniques to consistently manage page-level, visual-level, slicer-level, and in-measure filtering on the same column.

### The Problem: Direct Fact Table Filtering

Using fact table columns in slicers is common when you don't have a dedicated dimension table. However, this approach creates critical issues when those same columns appear in CALCULATE filter expressions.

**Why?** Because filters applied to fact table columns create bidirectional dependencies that corrupt your filter context logic.

Let's see what happens with an example scenario.

---

### Sample Data

Facts Table: 

facts_po_data

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-001A  | 0        |
| a          | PO-2024-002A  | 2        |
| a          | PO-2024-003A  | 4        |
| a          | PO-2024-004A  | 9        |
| a          | PO-2024-005A  | 12       |
| b          | PO-2024-006B  | 0        |
| b          | PO-2024-007B  | 1        |
| b          | PO-2024-008B  | 2        |
| c          | PO-2024-009C  | 0        |
| d          | PO-2024-010D  | 0        |

Dimension Table: 

Dim_Customers

| customer_id | Customer_name |
|------------|---------------|
| a          | cust_a        |
| b          | cust_b        |
| c          | cust_c        |
| d          | cust_d        |

**Data Model:** 

Dim_Customer [1] → [*] facts_po_data

---

### Business Requirement

We need to compare:
- **Dynamic PO Count**: Changes based on slicer selections (customers, days_old)
- **Fixed "Today" Count**: Always shows POs where days_old = 0, regardless of slicer selections, but still respects page-level filters

---

### Base Measure

```dax
Number of POs = 
COUNTROWS( facts_po_data )
```

This measure responds to all filters.

### Example: Filtering by Customer

**Slicer Selection:** 

Customer_name = cust_c

**Filtered fact table:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| c          | PO-2024-009C  | 0        |

**Result:** 

Number of POs = 1

---

### Case A: Wrong approach

```dax
Number of POs today = 
CALCULATE(
    COUNTROWS( 'facts_po_data' ),
    REMOVEFILTERS( 'facts_po_data'[days_old] ),
    'facts_po_data'[days_old] = 0
)
```

### Test 1: Simple scenario (No page filters)

**Slicer Selection:** 

days_old = 12 AND days_old = 9

**Filtered fact table for Number of POs:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-004A  | 9        |
| a          | PO-2024-005A  | 12       |

**Result:** 

Number of POs = 2

**Filtered fact table for Number of POs today:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-001A  | 0        |
| b          | PO-2024-006B  | 0        |
| c          | PO-2024-009C  | 0        |
| d          | PO-2024-010D  | 0        |

**Result:** 

Number of POs today = 4 (Works as expected)

### Test 2: Where it breaks (With page filter)

**Page-level filter:** Exclude customer_id = 'd' (equivalent to selecting customers a, b, c)

**Slicer Selection:** 

days_old = 1

**Step 1 - Page filter applied to fact table:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-001A  | 0        |
| a          | PO-2024-002A  | 2        |
| a          | PO-2024-003A  | 4        |
| a          | PO-2024-004A  | 9        |
| a          | PO-2024-005A  | 12       |
| b          | PO-2024-006B  | 0        |
| b          | PO-2024-007B  | 1        |
| b          | PO-2024-008B  | 2        |
| c          | PO-2024-009C  | 0        |

**Step 2 - Slicer filter applied for Number of POs:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| b          | PO-2024-007B  | 1        |

**Result:** 

Number of POs = 1 (Work as expected)

**Step 3 - What happens in Number of POs today (THE PROBLEM):**

The slicer on days_old = 1 means only customer b has visible rows. When CALCULATE removes the days_old filter and applies days_old = 0, it's working within the customer context that was established by the slicer.

**Filtered fact table that CALCULATE sees:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| b          | PO-2024-006B  | 0        |
| b          | PO-2024-007B  | 1        |
| b          | PO-2024-008B  | 2        |

Power BI returns rows for customers that had at least one record matching days_old = 1, then counts only the days_old = 0 rows within that customer subset.

**Final filtered table for counting:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| b          | PO-2024-006B  | 0        |

**Result:** 

Number of POs today = 1 (WRONG)

**Expected:** Should be 3 (customers a, b, c all have day 0 POs)

### What is the issue here? 

When you filter a fact table column (days_old = 1) in a slicer, it doesn't just filter the days_old values—it filters which rows exist in context. Since each row has a customer_id, you've implicitly filtered which customers are visible. Your REMOVEFILTERS only removes the explicit days_old filter, not the implicit customer filtering that resulted from selecting rows with days_old = 1.

**In a nutshell:** Fact table columns create row-level filter context that propagates across all columns in that table. You cannot isolate one column's filter manipulation from another's because they share the same row context.

---

### Case B: Correct approach

### Step 1: Create a Lookup Table

Lookup_Days

| days_old |
|----------|
| 0        |
| 1        |
| 2        |
| 4        |
| 9        |
| 12       |

### Step 2: Update Data Model

**New Model:** 

Dim_Customer [ 1 ] → [ * ] facts_po_data [ * ] ← [ 1 ] Lookup_Days

Both dimension tables connect to the fact table via one-to-many relationships.

### Step 3: Rewrite the Measure

```dax
Number of POs today = 
CALCULATE(
    COUNTROWS( 'facts_po_data' ),
    REMOVEFILTERS( 'Lookup_Days'[days_old] ),
    'Lookup_Days'[days_old] = 0
)
```

### Test: Same Scenario

**Page-level filter:** 

Exclude customer_id = 'd'

**Slicer Selection:** 

days_old = 1 (now filtering Lookup_Days table, not fact table)

**Step 1 - Page filter applied (through Dim_Customers relationship):**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-001A  | 0        |
| a          | PO-2024-002A  | 2        |
| a          | PO-2024-003A  | 4        |
| a          | PO-2024-004A  | 9        |
| a          | PO-2024-005A  | 12       |
| b          | PO-2024-006B  | 0        |
| b          | PO-2024-007B  | 1        |
| b          | PO-2024-008B  | 2        |
| c          | PO-2024-009C  | 0        |

**Step 2 - Slicer filter applied for`Number of POs (through Lookup_Days relationship):**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| b          | PO-2024-007B  | 1        |

**Result:** 

Number of POs = 1 (all good)

**Step 3 - What happens in Number of POs today (CORRECT):**

The slicer filters the Lookup_Days table, which propagates to facts through the relationship. When CALCULATE removes the Lookup_Days filter and applies days_old = 0, it operates on the Lookup_Days dimension independently from the customer filter.

**Customer filter remains intact (from Dim_Customers):**
- Customers a, b, c are in context

**Days filter is manipulated independently (from Lookup_Days):**
- REMOVEFILTERS clears the slicer
- days_old = 0 is applied

**Final filtered fact table for counting:**

| customer_id | PO_nr         | days_old |
|------------|---------------|----------|
| a          | PO-2024-001A  | 0        |
| b          | PO-2024-006B  | 0        |
| c          | PO-2024-009C  | 0        |

**Result:** 

Number of POs today = 3 (CORRECT)

---

### Summary

**Why This Works**

- Customer filters flow through: Dim_Customers → facts_po_data
- Days filters flow through: Lookup_Days → facts_po_data
- These are independent connections into the fact table
- CALCULATE operations on Lookup_Days[days_old] don't affect which customers are in context
- CALCULATE operations on customer filters don't affect which days are in context

**The key insight:** 

- Separate dimension tables create isolated filter contexts. Each dimension can be manipulated independently without creating row-level dependencies in the fact table.
- Never use fact table columns in both slicers and CALCULATE filter manipulations. Always create a dimension table to isolate the filter context.
