## 🧮 Overview

This repository is a curated collection of my most frequently used and production-tested DAX calculations.
It is designed as a reference library for building semantic models, to avoid going every time in previous models to retrieve old expressions.

Each code/use case is documented and organized by macro categories that I defined based on the code's nature and usage.

#### Formatting

Measures to use when creating custom formatting logics

| Topic | Description | Link |
|---------|-------------|------|
| Colour Measures |  Easy access to colours when building custom formatting logics | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Colour-Measures/README.md) |

#### Control

Measures to use within the visuals' filters. They let developers control what is displayed in the visuals

| Topic | Description | Link |
|---------|-------------|------|
| Filter by Measures | Control measure that allows users to filter visuals by quantitative measures (e.g., Total Amount) | [Link](https://github.com/SteCiu01/DAX/tree/main/DAX-Codes/Filter-by-Measures) |
| Slicers filtering other slicers | Control measure that removes “dead” slicer values and avoids confusing blank visuals | [Link](https://github.com/SteCiu01/DAX/tree/main/DAX-Codes/Slicer-Filtering-Other-Slicers) |

#### Virtual Table Measures

These are calculations that rely on in-code virtual tables, and that are used for complex/custom use cases, that would not be possible to successfully handle with simple measures.

| Topic | Description | Link |
|---------|-------------|------|
| Row Level Conditional Logic in Virtual Tables | Perform a logical calculation at a defined level of hierarchical granularity | [Link](https://github.com/SteCiu01/DAX/tree/main/DAX-Codes/Row-Level-Conditional-Logic-in-Virtual-Tables) |

