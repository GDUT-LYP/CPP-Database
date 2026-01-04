# CPP-Database

## Overview

**CPP-Database** is a curated collection of **66 industrial benchmarks** derived from real-world shipbuilding scenarios. While primarily designed for **Cutting Path Planning (CPP)** and **Skeleton Cut Line (SCL)** optimization, these instances are mathematically modeled as the **Precedence-Constrained Generalized Traveling Salesman Problem (PCGTSP)**.

Therefore, this dataset serves a dual purpose:

1. **Industrial Application**: A testbed for laser/plasma cutting path optimization with complex geometric constraints.
2. **Algorithm Benchmarking**: A standard, challenging benchmark suite for general **PCGTSP** solvers, offering a wide range of problem scales and topological structures distinct from random datasets.

## Dataset Characteristics

The dataset covers varying complexities, with material utilization rates ranging from 59.2% to 98.3%. The instances are categorized into three groups based on the number of contours (clusters):

| Scale | Instances | Contours | Nodes |
| :--- | :---: | :--- | :--- |
| **Small** | 16 | 2 - 10 | 30 - 162 |
| **Medium** | 25 | 11 - 40 | 208 - 1402 |
| **Large** | 25 | 46 - 128 | 536 - 2518 |

## File Format Specification

The data files use a **TSPLIB-extended format**, fully compatible with standard PCGTSP solvers.

Taking `Lc128v2518.txt` as an example:

### 1. Header Information

Metadata describing the problem scale and type.

```text
NAME: Lc128v2518            # Instance Name
TYPE: PCGTSP                # Explicitly defined as PCGTSP
DIMENSION: 2518             # Total number of nodes (vertices)
GTSP_SETS: 128              # Number of clusters (contours)
EDGE_WEIGHT_TYPE: EUC_2D    # Distance metric (2D Euclidean)
```

### 2. Data Sections

* **`NODE_COORD_SECTION`**: Defines the spatial coordinates (X, Y) of every vertex.
* **`GTSP_SET_SECTION`**: Defines the clusters. In the context of PCGTSP, each cluster corresponds to a single contour (either an outer outline or an inner hole) of a part.
  * Format: `<ClusterID> <NodeID_1> <NodeID_2> ... -1`
* **`GTSP_SET_ORDERING`**: Defines the precedence constraints between clusters. It specifies the topological order in which the clusters (contours) must be visited (e.g., inner contours must be cut before outer contours).
  * Format: `<Predecessor_ClusterID> <Successor_ClusterID> ... -1`

### Example Snippet

```text
NODE_COORD_SECTION
1 12300.0 2400.0
2 12300.0 2410.0
...
GTSP_SET_SECTION
1 1 2 3 4 5 -1
2 6 7 8 9 -1
...
GTSP_SET_ORDERING
1 2 -1        # Cluster 1 must be visited before Cluster 2
3 4 -1        # Cluster 3 must be visited before Cluster 4
...
EOF
