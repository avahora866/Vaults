---
tags:
  - "#SECONDARY_BRANCH"
---
Clustering is the process of grouping data points so that points in the same group (cluster) are more similar to each other than to those in other groups.

The goal of K-Means is to partition the dataset into $k$ distinct, non-overlapping subgroups (clusters).

# Process
1. Initialisation: We arbitraily select $k$ points in the data space to serve as the initial "Centroids" (centrer points)
2. Assignment: Every data point looks at the $k$ centroids and joins the group of the closest one (usually mesured by straight-line Euclidean distance)
3. Update: Once all points are assigned, we calculate the average position of all points in a cluster and move the centrodi to that new centre
4. Iteration: We repeat steps 2 and 3. As the centroids move, points may switch clusters. We stop when the centroids stop moving (convergence)

# Finding the optimal $k$ (number of clusters) - The Elbow Method
The algorithm has one major flaw: the human not the algorithm has to choose the number of clusters to use.

To find the optimal $k$ we use a metric called intertia (or Within-Cluster Sum of Squares)
- Inertia mesures how "tight" the clusters are. It calculates the sum of distances between every data point and its assigned centroid
- Lower Inertia is better (it means that the data points are vvery close to their cluster centre)

However the above statement of lower inertia being better comes with a cavet, it being that if we keep increasing $k$ (number of clusters), inertia always decreases. If we have 10,000 data points and set $k$ to 10,000 we will have 10,000 groups. As each group has one data points the inertia is 0 as distance between the data point and the centroid is 0. Although inertia is 0 the data is useless as we have custom tailoring for every single data point, so we have simplified nothing.

The solution to this is to look for diminishing returns, We plot inertia against $k$ (number of clusters). The graph usually looks like an arm bending. We want the "Elbow" of the curve. The point where adding more clusters no longer gives us a significant drop in inertia.

Analyzing the Plot:
- The Steep Drop: Going from 1 to 2 clusters usually drops Inertia massively. This is good.
- The Plateau: Eventually, the line flattens out.
- The Elbow: The optimal $k$ is the vertex where the steep drop transitions into a flat line.

#todo graph example
https://www.google.com/search?q=inertia+vs+k+graph&oq=inertia+vs+k+graph&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRigATIHCAIQIRigATIHCAMQIRiPAtIBCDUzNTZqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8

