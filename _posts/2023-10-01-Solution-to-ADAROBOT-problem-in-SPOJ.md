---
layout: post
title: Solution to ADAROBOT SPOJ problem (spoiler)
date: 2023-10-01 00:00:00 +0000
tags:
  spoj
  algorithms
  rust
---
The post contains a writeup to my solution to ADAROBOT - Ada and CAPTCHA SPOJ problem. 

## 1. Problem description
Full problem statement is provided [here](https://www.spoj.com/problems/ADAROBOT/).  
In short we have two functions:   
$$ T(N) = F(N \cdot  (N-1))^3 + T(N - 2), T(0) = 0, T(1) = 0 $$,   
$$ F(a) $$ - least significant 1-bit of $$a$$ indexed from 1   
On input we get a series of positive, even numbers not greater than $$ 2 \cdot 10^8 $$. For each number $$N$$ we need to print $$T(N)$$ on stdout.  

## 2. Observations  
For the rest of the post I will denote $$x$$ to be an arbitrary odd number.  
### 2.1 Expansion of T(N)
It is not difficult to spot that:   
$$ T(N) = F(N \cdot (N-1))^3 + T(N-2) = F(N \cdot (N-1))^3 + F((N-2)\cdot(N-3))^3 + T(N-4) $$  

Expanding this recursion further and leveraging the fact that $$N$$ is always even we get :   
$$T(N) = \sum_{k=1}^{k=N/2} F((2k) \cdot (2k-1))^3 $$

Above equation can be proved by trivial induction.

### 2.2 F(N * (N-1)) = F(N)
From the previous section we know that $$T(N)$$ can be decomposed into a sum consisting of $$ F((2k) \cdot (2k-1))^3 $$.  
Lets take a closer look at the product $$2k \cdot (2k-1)$$. We want to find its least significant 1-bit. If we write $$2k \cdot (2k-1)$$ as $$2^m \cdot x $$, the least significant 1-bit is equal to $$m+1$$.  
Lets write $$2k$$ as $$2^l \cdot x$$. Its least significant 1-bit is $$l+1$$. 
$$2k-1$$ is odd, so $$x \cdot (2k-1)$$ is also odd and least significant 1-bit of $$2^l \cdot x \cdot (2k-1) $$ is also $$l+1$$. Therefore $$ F((2k) \cdot (2k-1)) = F(2k) $$.



### 2.3 F(2) = F(6) = ... = F(4i - 2)
Say we have a number $$m=2^n \cdot x$$. What will be the smallest $$m' \gt m$$ such that $$m'=2^n \cdot x'$$, $$x'$$ is odd? If we add anything that is not a multiple of $$2^n$$ to $$m$$ the result number will not be a multiple of $$2^n$$. If we add exactly $$2^n$$ to $$m$$ we will get $$m + 2^n = 2^n \cdot x + 2^n = 2^n \cdot (x + 1)$$. $$x$$ is odd so $$x + 1$$ is even. Therefore $$m + 2^n = 2^{n+1} \cdot y$$, $$y$$ is odd or even. If we add $$2 \cdot 2^n$$ to $$m$$ we will get $$m + 2 \cdot 2^n = 2^n \cdot x + 2 \cdot 2^n = 2^n \cdot (x + 2) $$. $$x$$ is odd so $$x + 2$$ is also odd. So $$m + 2 \cdot 2^n$$ is the smallest number greater than m which has a form of $$2^n \cdot x'$$.  
$$m$$ and $$m'$$ have the same least significant 1-bit equal to $$(n+1)$$.  
For the fixed $$n$$ we can create a sequence consisting of all positive numbers in the form of $$2^n \cdot x$$. i-th element of this sequence can be written as $$2^n + (i - 1) \cdot 2 \cdot 2^n$$. With easy transformations we can check that this sequence is an arithmetic progression.

Lets consider the case when $$n=1$$.
First term of the arithmetic progression equals $$2 = 2^1$$, the next numbers in the form of $$2^1 \cdot x$$ will be 6, 10, 14 etc. So $$F(2) = F(4) = F(10)$$. i-th element of this progression can be written as $$2^1 + (i - 1) \cdot 2 \cdot 2^1 = 4i - 2$$. 

## 3. Solution
Applying the property (holding for even numbers) $$F(N \cdot (N-1)) = F(N)$$ to equation $$T(N) = \sum_{k=1}^{k=N/2} F((2k) \cdot (2k-1))^3 $$ we can simplify it to $$T(N) = \sum_{k=1}^{k=N/2} (F(2k))^3 $$.  
Every even number equals to some $$2^n \cdot x$$. If for every $$n$$ we counted how many numbers in the form of $$2^n \cdot x$$ are there up to $$N$$, we could compute $$T(N)$$ as:  
$$T(N) = \sum_{n=1}^{n=\lceil log(N) \rceil} F(2^n)^3 \cdot $$ (number of numbers not greater than $$N$$ in the form of $$2^n \cdot x$$).  
In the previous section we observed that for a fixed $$n$$ positive numbers in the form of $$2^n \cdot x$$ form an arithmetic progression. We would like to count number of terms of this progression not greater than $$N$$. This number is the biggest integer i satisfying the inequality $$2^n + (i-1) \cdot 2 \cdot 2^n \leq N$$ - we use the arithmetic progression formula from the previous section. This can be transformed into $$i \leq 1 + \frac{N - 2^n}{2^{n+1}}$$.

Putting this all together we can compute $$T(N)$$ with the following algorithm: 
1. Initialize $$S$$ to 0
2. Initialize $$n\_pow$$ to 2 
3. Compute $$(F(n\_pow))^3$$  
4. For an arithmetic progression $$n\_pow + (i-1) \cdot 2 \cdot n\_pow$$ count number of terms not greater than $$N$$. Lets denote the result by $$I$$.
5. Add $$I \cdot (F(n\_pow))^3$$ to S  
6. Assign $$n\_pow = 2 \cdot n\_pow$$.
7. If $$n\_pow$$ is greater than $$N$$, return $$S$$. Otherwise go to step 3.

Algorithm complexity  
Main algorithm loop will be executed $$log(N)$$ times. Each step of the algorithm is computed in constant time. Therefore the overall complexity of the algorithm is $$O(log(N))$$.

## 4. Implementation
I implemented the solution to this problem in Rust. The code is available [here](https://github.com/Caroleq/spoj-solutions/blob/main/adarobot/main.rs). Implementation roughly follows the steps of the algorithm described in the previous section, so I will not discuss it here. The only difference is in step 3. The problem statement says that $$N$$ does not exceed $$2 \cdot 10^8 $$. So $$2^n$$ will never exceed $$ 2 \cdot 10^8 $$. Starting from $$n = 28$$ $$2^n$$ will always be greater than $$ 2 \cdot 10^8 $$. I precomputed $$(F(2^n))^3$$ for $$n < 28$$ and pasted them into the code as an array. Instead of computing $$(F(2^n))^3$$ for every $$N$$ at runtime I get this value from the array. This makes program slightly faster.


