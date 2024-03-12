---
layout: post
title: Solution to JPM2 SPOJ Problem
date: 2024-03-11 00:00:00 +0000
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
$$ P(x) =  \sum_{k=1}^{N_{max}} a_kx^k$$, where $$a_k=1$$ if k is a prime number and $$a_k=0$$ otherwise.   
Lets consider a polynomial $$P(x)^2$$. It is easy to see that each of monomials $$x^k$$ with non-zero coefficient included in $$P(x)^2$$ is a result of multiplication of $$x^{k_1}$$ and $$x^{k_2}$$ from $$P(x)$$ such that $$k_1 + k_2 = k$$. $$k_1$$ and $$k_2$$ are prime numbers which goes from definition of $$P(x)$$. There are two possible cases for $$k_1$$ and $$k_2$$:  
1. $$k_1$$ and $$k_2$$ are different prime numbers. In this case pair {$$k_1$$, $$k_2$$} will add 2 to the coefficient of $$x^k$$ in $$P(x)^2$$. Once when $$k_1$$ comes from left (and $$k_2$$ from right) polynomial $$P(x)$$ in the product $$P(x) \cdot P(x)$$ and once when $$k_2$$ comes from left (and $$k_1$$ from right) polynomial $$P(x)$$ in the product $$P(x) \cdot P(x)$$  
2. $$k_1=k_2$$. In this case pair {$$k_1$$, $$k_2$$} will add 1 to the coefficient of $$x^k$$ in $$P(x)^2$$.

After above analysis deriving numbers that are a sum of two primes and counting ways of summing is easy:
1. I subtract 1 from all coefficients to which {$$p$$, $$p$$} was added. To do this I subtract 1 from all monomials $$x^{2p}$$ in $$P(x)^2$$ where p is a prime number. Value of coefficient of $$x^k$$ in the result polynomial $$P'(x)$$ is twice a number of different primes $$k_1$$, $$k_2$$ that sum up to k.  
2. I divide by two all coefficients of $$P'(x)$$. Lets denote the result polynomial as $$P''(x)$$. 

Value of coefficient of $$x^k$$ in $$P''(x)$$ is equal to the number of two different primes that sum up to k. 

### 3.3 Computing results for numbers that are a sum of three primes
Computing numbers that are a sum of three different primes is similar to calculations from the previous subsection. 

Lets consider polynomial $$W(x)=P''(x) \cdot P(x)$$. Each non-zero coefficient of monomial $$x^k$$ in $$W(x)$$ is a sum of products $$a_{k_1}x^{k_1}$$ from $$P''(x)$$ and $$b_{k_2}x^{k_2}$$ from $$P(x)$$ such that $$k_1 + k_2 = k$$. $$a_{k_1}$$ is a number of different primes that sum up to $$k_1$$ and $$k_2$$ is a prime. Lets consider all primes {$$p_1$$, $$p_2$$, $$k_2$$} that add 1 to $$a_{k_1}$$ coefficient.
There are two possibilities:  
1. $$k_2$$ is not equal to any of {$$p_1$$, $$p_2$$} - in this case primes {$$p_1$$, $$p_2$$, $$k_2$$} will add 3 to the coefficient of $$x^k$$ in $$W(x)$$. There are three ways to create $$x^k$$ using these numbers: $$x^{p_1 + p_2} \in P''(x)$$ and $$x^{k_2} \in P(x)$$, $$x^{k_2 + p_2} \in P''(x)$$ and $$x^{p_1} \in P(x)$$, $$x^{k_2 + p_1} \in P''(x)$$ and $$x^{k_1} \in P(x)$$  

2. $$k_2$$ is equal to one of {$$p_1$$, $$p_2$$}

## 4. Complexity