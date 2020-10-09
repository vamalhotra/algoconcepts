---
layout: post
title: Sweep Line Algorithm
date: 2020-01-05 23:48:29 +0000
---

Sweep line algorithm

The idea behind this algorithm is to imagine a line sweeping across the plane, from one end to another, and operations are limited to objects which intersect or are in vicinity of where the line stops. The complete solution is available when the line has passed over all objects.

Problems:

1. [Line segment intersection problem](https://en.wikipedia.org/wiki/Line_segment_intersection): You're given a list of line segments and task is to list all the intersections or to find if there are any intersections. Whether there are any intersections can be solved in O(N log N) where N is number of line segments. Also, all K intersections can be found in O((N+K) log N)
2. 