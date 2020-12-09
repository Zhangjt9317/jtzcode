---
title: ML -- clustering
date: 2020-10-31 18:32:07
tags:
---

In this post I will primarily introduce some clustering techniques and algorithms people commonly see in real-life practice.

## 1.3 Complexity

Take a look at pseudocode given the condition:

- n x m (n numbers of data, each with m dimensions)
- randomly generate K numbers of m-dim points 

```
while(t):
    for (int i = 0; i < n; i++){
        for (int j = 0; j < k; j++){
            compute distance from i to cluster j
        }
    }

    for (int i = 0; i < k; i++){
        1. find all points that belong to this cluster
        2. change coordinate to the centroid coordinate of these points
    }
end
```

**Time complexity:** 

* O(tknm), t ==> number of iterations, k ==> number of clusters, m ==> numbr of dimensions

Space complexity

* O(m(n+k)), k ==> number of clusters, m ==> number of dimensions, n ==> numbre of points


## 2. Advantages 

- easy to understand, local optimal is good enough for result
- good elasticity when dealing with big data 
- when cluster approaches normal distribution, performance is pretty good 
- low complexity 

### 2.2 Disadvantages 

- manually tune K, different K gets different results 
- sensitive to centroid location, different approaches get different results 
- sensitive to abnormal values
- hard classifier (point belongs to 1 class only), not good for multiple target classification
- not good for discrete classfication, imbalance classfication, non-convex classification

## 3. Performance Improvement 

There are ways to improve the performance regarding its disadvantages, like: preprocessing data, choosing reliable K value, mapping into higher dimension, etc. 


