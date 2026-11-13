### Topological Data Analysis & Microbiome: The Shape of the Gut Microbiome

This project aims to analyze the geometric and topological structure of the gut microbiome of 446 adults from Colombian cities, to find out whether it is organized as **separate groups** or as a continuum, and what changes most along that structure: the cardiometabolic profile or diet.

The full report and slides (in Spanish, developed as a final project for a Geometry and Topology for Data Science course) can be found in the repo, but the main points are described here.

The microbiome is high-dimensional (thousands of bacteria per person) and sparse (each person carries only a small subset of the genera detected in the study), which makes classical methods hard to apply. Knowing whether the population forms clusters or a gradient matters, because the analysis, and what can be done with it, is different in each case.

The data came in three tables: clinical metadata (sex, age, glucose, BMI, cholesterol, etc.), OTUs and taxonomy. OTUs were aggregated to genus level, which reduces noise and is easier to interpret at the cost of resolution, and converted to relative abundances to correct for differences in sequencing depth. Each person is a point.

The main geometry was Bray-Curtis distance on relative abundances, with CLR (log-ratios) as a control. PCoA was used to visualize it, although the first two components explain only ~39% of the variance. The point cloud shows no clearly separated groups. Persistent homology (Vietoris-Rips, β0 and β1) was applied to both geometries to compare their global shape.

Mapper was then used to build a graph of the data. The lens was PCoA1 + local density, the space was split into 10 regions with 35% overlap, and DBSCAN was run inside each region, giving a graph of 40 nodes and 89 edges. The graph is not fragmented: it looks like one continuous structure. To measure how well each variable follows the graph, we combined a spatial autocorrelation measure with a permutation test. The cardiometabolic score had the strongest alignment (0.92, p = 0.003), while the diet score was much weaker. Comparing the two ends of the graph, people with a lower cardiometabolic score had higher adiponectin, lower waist circumference, better HDL and more *Akkermansia*, while the opposite end had more *Prevotella*, *Escherichia* and *Haemophilus*.

Comparison of geometries

| | Bray-Curtis | Jaccard | CLR |
|---|---|---|---|
| Based on | Relative abundances | Presence / absence | Log-ratios between genera |
| Shape | Continuous | Central core with branches | Continuous, no branches |
| Link to cardiometabolic score | Strong | Moderate | Good |

The signal was checked under three geometries and several lenses (PCoA1 + PCoA2, local + global density, UMAP + density). The cardiometabolic score stayed the most aligned variable in all of them, but its strength varied.

Bray-Curtis contains a fair amount of noise, which may partly explain the high alignment score. The extremes analysis is exploratory, not a full differential analysis, and would benefit from input from biologists or other domain experts. Age showed an interesting geometric relationship, but it did not reach statistical significance.

We concluded that the microbiome of this population behaves as a continuum rather than as groups, and that the cardiometabolic profile is the variable that changes most coherently along it. This is an association: we cannot say the microbiome causes the metabolic state or vice versa, nor that one end of the continuum is "healthy" and the other is not. As next steps, this structure could be used to train machine learning models for public health prevention, to build better scores by combining classical methods with TDA, and to investigate variables such as age and sex in more depth.

Note: the dataset was not included in this repo.
