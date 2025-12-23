## 🧮 DAX Codes & Best Practices

Collection of my most frequently used and production-tested DAX codes, together with data modelling and filtering best practices, related to DAX.

It is designed as my reference library for building semantic models and reports, to avoid going every time in previous models to retrieve old expressions.

Each code/use case is documented and organized by macro categories that I defined based on the code's nature and usage.

---

### Formatting

DAX techniques for implementing advanced and reusable custom formatting logic in Power BI visuals.

| Topic | Description | Link |
|---------|-------------|------|
| Colour Measures | Centralised colour measures to standardise and reuse colour logic across conditional formatting scenarios | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Colour%20Measures/README.md) |
| Conditional Colour Driven by Slicer Selection | Apply dynamic colour formatting based on slicer-driven user selections | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Conditional%20Colour%20Driven%20by%20Slicer%20Selection/README.md) |
| Traffic Light Target Indicators | Create traffic-light style indicators in tables and matrix visuals based on measure thresholds | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Traffic%20Light%20Target%20Indicators/README.md) |
| Top-N / Bottom-N Conditional Colouring | Use RANKX-based logic to highlight Top-N or Bottom-N categories via conditional formatting | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Top-N%20-%20Bottom-N%20Conditional%20Colouring/README.md) |
| Dynamic Visual Headers | Generate visual titles and headers that automatically adapt to slicer selections | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Dynamic%20Visuals%20Headers/README.md) |
| SVG Rendering Inside Measures | Embed SVG images directly in DAX measures to enrich visuals with custom icons and indicators | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/SVG%20Rendering%20Inside%20Measures/README.md) |

---

### Filter Context

Patterns and guidelines for managing complex filter interactions in DAX through proper model design and measure logic.

| Topic | Description | Link |
|---------|-------------|------|
| Filter Context Handling via Data Model Design | Use data modelling techniques to consistently manage page-level, slicer-level, and in-measure filtering on the same column | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Filter%20Context%20Handling%20via%20Data%20Model%20Design/README.md) |

---

### Time Intelligence

A curated set of essential time intelligence patterns, supported by an optimised calendar table to ensure reliable and predictable results.

A complete and contiguous calendar table is a prerequisite for correct time intelligence calculations, with no missing dates from start to end.

| Topic | Description | Link |
|---------|-------------|------|
| Calendar Table Construction | Power Query (M) code to generate a complete calendar table for the semantic model, including a future date for proper SPLY calculations | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Calendar%20Table%20Construction/README.md) |
| Reusable Time Intelligence Measures | A collection of commonly used time intelligence measures, ready to be imported via DAX Query View | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Reusable%20Time%20Intelligence%20Measures/README.md) |

---

### Control

Control measures used in visuals and navigation elements to dynamically manage what users see and interact with based on defined conditions.

| Topic | Description | Link |
|---------|-------------|------|
| Slicers Filtering Other Slicers | Use control measures to remove invalid slicer values and prevent misleading or empty visuals | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Slicer-Filtering-Other-Slicers/README.md) |
| Measures Controlling Slicer Values | Synchronise slicers and visuals to avoid slicers displaying values excluded by visual-level logic | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Measures%20Controlling%20Slicer%20Values/README.md) |
| Visual Filtering by Measures | Enable users to filter visuals dynamically based on quantitative measures (e.g. revenue, margin, volume) | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Visual%20Filtering%20by%20Measures/README.md) |
| Dynamic Top-N Selection | Create a numeric input slicer that dynamically controls the Top-N entities displayed in a visual | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Dynamic%20Top-N%20Selection/README.md) |
| Page-Level Access Control (UI-Based) | Restrict navigation to specific report pages for non-authorised users and redirect them to an access request page | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Page-Level%20Access%20Control%20(UI-Based)/README.md) |

---

### Virtual Tables

Advanced calculation patterns based on in-measure virtual tables, enabling solutions that are not achievable with simple aggregation measures.

| Topic | Description | Link |
|---------|-------------|------|
| Hierarchical Conditional Logic with Virtual Tables | Apply conditional logic at a controlled hierarchical granularity using virtual tables and iterators | [Link](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Hierarchical%20Conditional%20Logic%20with%20Virtual%20Tables/README.md) |
| Distinct Combination Counting via Virtual Tables | Resolve over-granularity issues by generating virtual tables at the desired level to count distinct item combinations | [Link]([https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Distinct-Combinations-Count-with-Virtual-Tables/README.md](https://github.com/SteCiu01/DAX/blob/main/DAX-Codes/Distinct%20Combination%20Counting%20via%20Virtual%20Tables/README.md) |

