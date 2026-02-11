## 🧮 DAX Codes & Best Practices

A comprehensive collection of production-tested DAX codes and data modelling techniques for Power BI semantic models. 

Each code is fully documented with use cases, implementation steps, practical examples and is organized by functional category.

---

### Visual Formatting & Presentation

Advanced DAX techniques for creating dynamic, professional-looking visuals with full control over formatting and conditional styling.

| Topic | Use Case | Link |
|-------|----------|------|
| **Centralized Color Definitions** | Create reusable color palette measures to standardize conditional formatting across reports | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/tree/main/DAX-Codes/Centralized%20Color%20Definitions) |
| **Slicer-Driven Conditional Formatting** | Apply dynamic colors to visuals based on user slicer selections | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Slicer-Driven%20Conditional%20Formatting/README.md) |
| **KPI Traffic Light Indicators** | Display status indicators (🟢🟠🔴) in tables and matrices based on performance thresholds | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/KPI%20Traffic%20Light%20Indicators/README.md) |
| **RANKX-Based Highlighting** | Automatically highlight Top-N or Bottom-N items using conditional formatting | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/RANKX-Based%20Highlighting/README.md) |
| **Context-Aware Visual Titles** | Generate dynamic headers that adapt to display selected filter values | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Context-Aware%20Visual%20Titles/README.md) |
| **SVG Image Integration** | Embed vector graphics directly in DAX measures for custom icons and visual indicators | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/SVG%20Image%20Integration/README.md) |
| **Custom Data Label Formatting** | Override default label behavior with full control over K/M/B suffixes, decimals, and currency symbols | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Custom%20Data%20Label%20Formatting/README.md) |

---

### Filter Context & Data Modelling

Data model design topics for managing complex filter interactions and preventing common pitfalls when manipulating filter context.

| Topic | Use Case | Link |
|-------|----------|------|
| **Dimension Table Isolation Pattern** | Prevent filter context conflicts by using lookup tables instead of fact table columns in CALCULATE expressions | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Dimension%20Table%20Isolation%20Pattern/README.md) |

---

### Time Intelligence

Essential time-based calculation topics built on a robust calendar table foundation to ensure accurate and reliable period comparisons.

| Topic | Use Case | Link |
|-------|----------|------|
| **Optimized Calendar Table** | Power Query (M) code to generate a complete date dimension with rank columns and future date handling for SPLY calculations | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Optimized%20Calendar%20Table/README.md) |
| **Standard Time Functions Library** | Pre-built YTD, SPLY, prior period measures ready for import via DAX Query View | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Standard%20Time%20Functions%20Library/README.md) |

**⚠️ Critical Requirement:** All time intelligence topics depend on a complete calendar table with no gaps between start and end dates. Use the provided construction topic to avoid calculation errors.

---

### Interactive Controls & Dynamic Filtering

Measures and techniques for creating interactive user experiences with slicers, filters, and conditional navigation.

| Topic | Use Case | Link |
|-------|----------|------|
| **Cross-Slicer Value Filtering** | Remove invalid slicer options based on other slicer selections to prevent empty visuals | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Cross-Slicer%20Value%20Filtering/README.md) |
| **Visual-Slicer Synchronization** | Keep slicers aligned with visual-level filters to avoid showing excluded values | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Visual-Slicer%20Synchronization/README.md) |
| **Measure-Based Range Filtering** | Enable dynamic filtering by quantitative thresholds (e.g., revenue ranges, margin brackets) | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Measure-Based%20Range%20Filtering/README.md) |
| **Parameter-Driven Top-N** | Create numeric input slicers for interactive Top-N/Bottom-N filtering with user-defined thresholds | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Parameter-Driven%20Top-N/README.md) |
| **Conditional Page Navigation** | Restrict report page access based on Azure AD group membership with automatic redirection | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Conditional%20Page%20Navigation/README.md) |

---

### Virtual Tables & Advanced Calculations

In-memory table techniques for calculations that cannot be solved with standard aggregation measures.

| Topic | Use Case | Link |
|-------|----------|------|
| **Hierarchical-Locked Conditional Logic** | Apply conditional logic at a specific hierarchical level regardless of visual drill state | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Hierarchical-Locked%20Conditional%20Logic/README.md) |
| **Distinct Combination Counting** | Count unique combinations at controlled granularity using SUMMARIZE to avoid over-counting | [Link](https://github.com/SteCiu01/DAX-and-Data-Modelling/blob/main/DAX-Codes/Distinct%20Combination%20Counting/README.md) |

### 🛠️ Implementation Notes: When to Use Virtual Tables

Virtual tables are required when:
- Logic must execute **before** visual aggregation
- You need to control calculation grain independent of filter context
- Standard CALCULATE topics produce incorrect results due to row-level dependencies

