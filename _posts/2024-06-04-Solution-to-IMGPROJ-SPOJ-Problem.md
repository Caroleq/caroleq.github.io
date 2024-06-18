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
It is assumed that the reader is familiar with segment trees. [This](https://cp-algorithms.com/data_structures/segment_tree.html) article contains a thorough description of segment trees.  

## 1. Problem description
Full problem statement is provided [here](https://www.spoj.com/problems/IMGPROJ/).  

Summing up the problem description, we are given a definition of diagonal projection:  
Diagonal projection of a matrix $$I$$ with $$M$$ rows and $$N$$ columns is the vector $$(d_1, d_2, ..., d_{M+N-1})$$ where $$d_i = \sum_{x+y-1=i}$$ I(x,y).  
On the program input there is a matrix. Our task is to compute its diagonal projection. An important note: Rows of the matrix are provided in [run length encoding](https://en.wikipedia.org/wiki/Run-length_encoding). Also, the returned diagonal projection has to be run-length encoded. Through the rest of the article I will refer to i'th "{intensity-count} {intensity-value} " pair in the j'th row in run-length encoded matrix as $$\textit{rle-pair}_{j,i}$$  

Width of each image is at most $$10^9$$ and height is at most $$10^3$$. Total size of the input does not exceed 4 MB.  

## 2. Initial attempts
It is easy to spot, that $$\textit{rle-pair}_{j,i}$$ will add value $$\textit{rle-pair}_{j,i}[1]$$ to range of elements $$d_{i + p}, d_{i+p+1}, ... , d_{i+p+count-1}$$ where p is equal to $$\sum_{k < i}\textit{rle-pair}_{j,i}[0]$$.  

My initial idea was to create a vector of length M+N-1. Then iterate over all pairs and for each $$\textit{rle-pair}_{j,i}$$ add $$\textit{rle-pair}_{j,i}$$[1] to the part of digital projection the pair concerns.   

Unfortunately my submit was getting time limit exceeded status.  

Complexity of this algorithm is equal to the size of matrix ($$M \times N$$), because each matrix cell is processed exactly once in one RLE pair. Maximum width is $$10^9$$ and maximum height is $$10^3$$, so the algorithm may need number of computations of order $$10^9 \cdot 10^3=10^{12}$$. I needed to find a faster solution. 

## 3. Improved solution
The task statement guarantees that input size is at most 4MB. Each pair $$\textit{rle-pair}_{j,i}$$ occupies at least 4 bytes of input (1 byte for count part, 1 byte for value part and 2 bytes for spaces). Therefore there is no more than $$4 \cdot 10^6 / 4 = 10^6$$ pairs.   

Lets divide the vector $$(d_1, d_2, ..., d_{M+N-1})$$ into chunks where each chunk $$c = (d_{n}, d_{n+1}, ... , d_{n+m})$$ is the biggest subvector such that there is no $$\textit{rle-pair}_{j,i}$$ that adds values to some, but not all elements of c. Intuitively, lets take starting points (first elements to which a value will be added) of all $$\textit{rle-pair}_{j,i}$$ within diagonal projection and split projection in these points.


Borders within diagonal projection determined by beginnings and ends of all rle-pairs.

I came up with an idea to solve the problem using a segment tree. Each leaf holds one of the forementioned chunks. Each node contains a sum that should be added to $$d_i$$'s from chunks in its subtree. Number of leaves will be the minimum power greater than $$10^6$$. Size of the whole tree will twice as big as the number of its leaves (binary tree property). Ever node will hold the following elements: left-most and right-most diagonal projection element in its subtree and a value that should be added to all diagonal projection elements in the range. Each rle-pair will add a value to diagonal projection elements in one or more consecutive chunks. Therefore values coming from these pairs can be added into the tree as intervals using standard insertion procedure.  
After inserting all rle-values, final values of diagonal projection elements in a given chunk can be determined by summing values in the shortest path from root to the chunk.  
After computing diagonal projection values for all chunks we need to return it in run-length encoding. Obviously, computing diagonal projection in one chunk will have the same value. We cannot however directly convert each chunk into rle-pair and return it as a problem solution. It may happen that neighboring chunks will have the same diagonal projection values. RLE pairs should be as long as possible, so consecutive neighboring chunks with the same diagonal projection values need to be merged into one RLE-pair. After creating list of RLE-pairs from chunks and merging consecutive pairs with the same value, the rle-pair list can be returned as a problem solution.


Putting this all together we can compute running length encoded diagonal projection with the following algorithm: 
1. Compute start and end indexes of all $$\textit{rle-pair}_{j,i}$$ within diagonal projection   
2. Create a segment tree whose leaves hold a range from chunks. Each node in the tree holds a value that must be added to all projection elements within in the range defined by its subtree.  
3. Insert every RLE-pair into the tree using standard segment tree insertion procedure.  
4. Compute final diagonal projection values in every chunk by summing value from all nodes from root node to the chunk.  
5. Convert each chunk into RLE-pair, merge consecutive pairs with the same values and return the RLE-list.


### 3.1 Algorithm complexity  
Lets summarize time complexities of individual algorithm steps:  
1. Computing start and end indexes of RLE-pairs in may take $$O(N \cdot M)$$. Previous analysis shows that number of pairs is at most $$10^6$$ and each pair may be processed in constant time.    
2. Size of a segment tree with chunks is twice the number of leaves (aligned to a power of two). Time complexity of creating and filling with chunk range information is proportional to the size of the tree, which is $$O(N \cdot M)$$. Number of chunks is at most $$10^6$$ so tree size is at most $$2 \cdot 2 \cdot 10^6$$ (multiplied by 4 to incorporate whole tree size and align tree to the power of 2).   
3. Inserting a single element to a segment tree has time complexity $$O($$ tree size $$)$$. There are $$O(N \cdot M)$$ RLE-pairs and tree size is $$O(N \cdot M)$$. 

## 4. Implementation
I implemented the solution to this problem in Rust. The code is available [here](https://github.com/Caroleq/spoj-solutions/blob/main/imgproj/main.rs).



