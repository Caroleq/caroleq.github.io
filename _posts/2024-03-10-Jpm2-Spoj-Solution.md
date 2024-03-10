---
layout: post
title: Solution to JPM2 SPOJ Problem
date: 2024-02-12 00:00:00 +0000
tags:
  spoj
  algorithms
  fft
---

## 1. Prerequisites 
To understand the problem walkthrough it would be beneficial to understand: 
- FFT algorithm 
- applying FFT to multiply polynomials

This [article](https://faculty.sites.iastate.edu/jia/files/inline-files/polymultiply.pdf) explains above topics. 

## 2. Problem description
Full problem statement is provided [here](https://www.spoj.com/problems/JPM2/).  

Quoting problem statement: "Given a positive integer N, calculate the minimum number of distinct primes required such that their sum equals to N. Also calculate the number of different ways to select these primes. Two ways are considered to be different iff there exists at least one prime in one set not existing in the other."

In short, there are $$T$$ ($$1 \leq T \leq 500000$$) numbers N in range ($$1 \leq N \leq 500000$$).

## 3. Observations 
It can be checked that apart from 1, 4 and 6 every number in the range [1, 500000] is a sum of at most three different prime numbers. I noted it while solving [Just Primes](https://www.spoj.com/problems/JPM/) problem. Numbers 1, 4 and 6 are not a sum of any different primes.

There are two conjunctures that somehow relate to this observation:
- [Goldbach's conjecture](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) - states that every even natural number greater than 2 is the sum of two prime numbers   
- [Goldbach's weak conjecture](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) - Every odd number greater than 5 can be expressed as the sum of three primes.

These conjectures however do not have the constraint that primes forming the numbers need to be unique e.g. 4 and 6 cannot be expressed as a sum of unique primes. 

## 3. Algorithm 
The amount of test cases $$T$$ can be as large as the number of all possible test case values $$N$$. Therefore I decided to precompute solutions for all possible numbers $$N$$ and upon reading the input to return the precomputed result. 

As the tag in the description suggests, the problem can be solved using FFT. 

I computed all primes up to 524288 using [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). 524288 is the first power of two that is greater than 500000. Because I used FFT to solve the problem, size of many data structures had to be aligned to a power of two. From now on I will refer to 524288 as $$N_{max}$$.

Following three subsections discuss determining numbers that can be expressed as a sum of at most 1, 2 and 3 different prime numbers and calculating number of ways to select these primes.

### 3.1 Computing results for numbers that are a sum of one prime
This case is trivial. Prime numbers in considered range are a sum of one prime and no complex number is. There is also only one way to select a given prime.

### 3.2 Computing results for numbers that are a sum of two primes
I created a polynomial with coefficient standing near $$x^k$$ equal to 1 if k is a prime number and 0 otherwise:   
$$ P_1(x) =  \sum_{k=1}^{N_{max}} a_kx^k$$, where $$a_k=1$$ if k is a prime number and $$a_k=0$$ otherwise.


### 3.3 Computing results for numbers that are a sum of three primes


