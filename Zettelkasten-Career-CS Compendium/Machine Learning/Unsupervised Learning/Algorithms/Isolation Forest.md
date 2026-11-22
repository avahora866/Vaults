---
tags:
  - "#SECONDARY_BRANCH"
---
Isolation Forest is an anomoly detection algorithm to help find the execptions (what is abnormal)

# Process
Imagine you have a dataset of points on a sheet of paper
1. You make a random straight cut across the paper
2. You make another random cut
3. You keep making random cuts

- If a point is buried deep inside a dense cluster of "normal" points, it takes many cuts to isolate it (seperate it from the rest).
- If a point is an "anomoly" floating alone on the edge of the paper, it might take only one or two cuts to seperate it

Therfore the algorithm counts how many "cuts" it takes to isolate a point
- Few cuts = Anonmoly (Fraud, Defect, Error)
- Many cuts = Normal Data

