## 🧮 DAX Codes Overview

This repository is a curated collection of my most frequently used and production-tested DAX calculations.
It is designed as a reference library for building semantic models, to avoid going every time in previous models to retrieve old expressions.

Each code/use case is documented and organized by macro categories that I defined based on the code's nature and usage.

#### Formatting

Measures to use when creating custom formatting logics

| Topic | Description | Link |
|---------|-------------|------|
| Colour Measures |  Easy access to colours when building custom formatting logics | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Colour-Measures/README.md) |
| Conditional Colour Based on Slicer Selection | Simple conditional colour logics | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional-Colour-Simple-Case/README.md) |
| Conditional TopN Colour Rank |  Use RANKX for highlighting TOP-N or BOTTOM-N categories | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional-TOPN-Colour-RANKX/README.md) |
|Dynamic Visuals Headers |  Visual header that change based on slicers selection | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Dynamic-Visuals-Headers/README.md) |

#### Control

Measures to use within the visuals' filters. They let developers control what is displayed in the visuals

| Topic | Description | Link |
|---------|-------------|------|
| Slicers Filtering Other Slicers | Control measure that removes “dead” slicer values and avoids confusing blank visuals | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Slicer-Filtering-Other-Slicers/README.md) |
| Filter by Measures | Control measure that allows users to filter visuals by quantitative measures (e.g., Total Amount) | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Filter-by-Measures/README.md) |

#### Virtual Table Measures

These are calculations that rely on in-code virtual tables, and that are used for complex/custom use cases, that would not be possible to successfully achieve with simple measures.

| Topic | Description | Link |
|---------|-------------|------|
| Row Level Conditional Logic with Virtual Tables | Perform a logical calculation at a defined level of hierarchical granularity | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Row-Level-Conditional-Logic-in-Virtual-Tables/README.md) |

