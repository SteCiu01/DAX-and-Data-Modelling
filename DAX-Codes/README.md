## 🧮 DAX Codes Overview

This repository is a curated collection of my most frequently used and production-tested DAX codes, together with few DAX and data modelling guidelines/best practices worth sharing.

It is designed as my reference library for building semantic models, to avoid going every time in previous models to retrieve old expressions.

Each code/use case is documented and organized by macro categories that I defined based on the code's nature and usage.

#### Formatting

DAX techiques to use when creating custom formatting logics.

| Topic | Description | Link |
|---------|-------------|------|
| Colour Measures |  Easy access to colours when building custom formatting logics | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Colour-Measures/README.md) |
| Conditional Colour Based on Slicer Selection | Simple conditional colour logics | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional-Colour-Simple-Case/README.md) |
| Traffic Light Conditional Target Markers | Create a measure to display in tables or matrix visuals traffic light style markers based on a measure's value. | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional-Target-Markers/README.md) |
| Conditional TopN Colour Rank |  Use RANKX for highlighting TOP-N or BOTTOM-N categories | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional-TOPN-Colour-RANKX/README.md) |
| Dynamic Visuals Headers |  Visual header that change based on slicers selection | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Dynamic-Visuals-Headers/README.md) |
| In-Measure SVGs | Use SVG images within measures to enhance your visualisations | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/In-Measure-SVGs/README.md) |

#### Filter Context

Guidelines on how to handlecomplex filtering with DAX.

| Topic | Description | Link |
|---------|-------------|------|
| Filter Context and Data Model Design | Leverage data modelling to handle page-level, slicer and in-measure filtering on the same column | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Data-Model-for-Filter-Contex-Measures/README.md)

#### Time Intelligence

Collection of few important time intelligence measures together with and optimised simple calendar table that guarantees them to work properly.

In fact, a pre-requisite for time intelligence functions to work is to have a calendar table with a dates column where no day is missing from day start to day end.

| Topic | Description | Link |
|---------|-------------|------|
| Create the Calendar Table | M-Code for creating a complete calendar table for the semantic model. Includes a future day for proper SPLY functions | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Create-Calendar-Table/README.md) |
| Handy Time Intelligence Functions | Some time intelligence functions, ready to be imported through DAX Query View | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Handy-Time-Intelligence-Functions/README.md) |

#### Control

Measures to use within the visuals or in control/navigation buttons. They let developers control what is displayed in the visuals or what users will be clicking based on some criteria.

| Topic | Description | Link |
|---------|-------------|------|
| Slicers Filtering Other Slicers | Control measure that removes “dead” slicer values and avoids confusing blank visuals | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Slicer-Filtering-Other-Slicers/README.md) |
| Filter by Measures | Control measure that allows users to filter visuals by quantitative measures (e.g., Total Amount) | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Filter-by-Measures/README.md) |
| Dynamic Top-N Selection| Build a slicer where you can type in a number (N) and this slicer would make you show in your visual the Top-N customers by a measure | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Top-N-Selection/README.md) |
| Page Level Security | Coming Soon | - |

#### Virtual Tables

These are examples of calculations that rely on in-code virtual tables, and that are used for complex/custom use cases, that would not be possible to successfully achieve with simple measures.

| Topic | Description | Link |
|---------|-------------|------|
| Row Level Conditional Logic with Virtual Tables | Perform a logical calculation at a defined level of hierarchical granularity | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Row-Level-Conditional-Logic-in-Virtual-Tables/README.md) |
| Virtual Tables for Counting Distinct Combinations of Items | When you have a too granular table to count items, use virtual tables to generate you desired granularity | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Distinct-Combinations-Count-with-Virtual-Tables/README.md) |
