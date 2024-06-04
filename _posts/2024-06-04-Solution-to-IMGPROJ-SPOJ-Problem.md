---
layout: post
title: Solution to IMGPROJ SPOJ Problem
date: 2024-03-16 00:00:00 +0000
tags:
  spoj
  algorithms
  segment-trees
---

The post contains my writeup to IMGPROJ - Image Projections SPOJ problem.

## 1. Prerequisites

## 1. Problem description
Full problem statement is provided [here](https://www.spoj.com/problems/IMGPROJ/).  

Summing up the problem description, we are given a definition of diagonal projection:  
Diagonal projection of a matrix $$I$$ with $$M$$ rows and $$N$$ columns is the vector $$(d_1, d_2, ..., d_{M+N-1})$$ where $$d_i = \sum_{x+y-1=i}$$ I(x,y).  
On the program input there is a matrix. Our task is to compute its diagonal projection. An important note: Rows of the matrix are provided in [run length encoding](https://en.wikipedia.org/wiki/Run-length_encoding). Also, the returned diagonal projection also has to be run-length encoded. Through the rest of the article I will refer to i'th "{intensity-count} {intensity-value} " pair in the j'th row in run-length encoded matrix as $$\textit{rle-pair}_{j,i}$$  

Width of each image is at most $$10^9$$ and height is at most $$10^3$$. Total size of the input does not exceed 4 MB.  

## 2. Initial attempts


## 3. Improved solution
The task statement guarantees that input size is at most 4MB. Each pair "{intensity-count} {intensity-value} " in run length encoded row of the input matrix occupies at least 4 bytes. Therefore there is no more than $$4 \cdot 10^6 / 4 = 10^6$$ count-value pairs.   
It is easy to spot, that count-value pair in the i'th row of the matrix will add value to elements $$d_{i + p}, d_{i+p+1}, ... , d_{i+p+count-1}$$ where p is the sum of lengths of previous counts in count-value pairs 
 

## 4. Implementation