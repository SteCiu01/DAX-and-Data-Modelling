# Measures Controlling Slicer Values

Synchronise slicers and visuals to avoid slicers displaying values excluded by visual-level logic

#### Situation

We have a matrix visual that displays data as below:

| Products | Total Returns | Associated Sales |
| ----- | ---- | ---- |
| Product A | 125 | 560 |
| Product B | blank | 154 |
| Product C | 265 | 1,240 |

However, we want to only see those products that had returns. 

To achieve it we need to set the level visual filter for [Total Returns] as "IS NOT BLANK".

The result is:

| Products | Total Returns | Associated Sales |
| ----- | ---- | ---- |
| Product A | 125 | 560 |
| Product C | 265 | 1,240 |

However, if we have a slicer for Products or for any other Products-related categories (e.g., Product Category, Sub-Products, etc.), anything related to Product B will be still visible in the slicer/s. 

We need to replicate the operation and drop the measure [Total Returns] in the slicer/s visual filter section and set it as "IS NOT BLANK".

Now also the slicer containing Products will contain only Product A and Product B.
