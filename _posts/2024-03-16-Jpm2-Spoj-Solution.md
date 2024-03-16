---
layout: post
title: Solution to JPM2 SPOJ Problem
date: 2024-03-16 00:00:00 +0000
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

Quoting the problem statement: "Given a positive integer N, calculate the minimum number of distinct primes required such that their sum equals to N. Also calculate the number of different ways to select these primes. Two ways are considered to be different iff there exists at least one prime in one set not existing in the other."

On the input there will be $$T$$ ($$1 \leq T \leq 500000$$) numbers $$N$$ in range ($$1 \leq N \leq 500000$$).

## 3. Observations 
It can be checked that apart from 1, 4 and 6 every number in the range [1, 500000] is a sum of at most three different prime numbers. I noted it while solving [Just Primes](https://www.spoj.com/problems/JPM/) problem. Numbers 1, 4 and 6 are not a sum of any different primes.

There are two conjunctures that somehow relate to this observation:
- [Goldbach's conjecture](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) - states that every even natural number greater than 2 is the sum of two prime numbers   
- [Goldbach's weak conjecture](https://en.wikipedia.org/wiki/Goldbach%27s_conjecture) - Every odd number greater than 5 can be expressed as the sum of three primes.

These conjectures however do not have the constraint that primes forming the numbers need to be unique e.g. 4 and 6 cannot be expressed as a sum of unique primes. 

## 3. Algorithm 
The amount of test cases $$T$$ can be as large as the number of all possible test case values $$N$$. Therefore I decided to precompute solutions for all possible numbers $$N$$ and upon reading a test case from the input to return a precomputed result. 

As the tag in the SPOJ problem description suggests, it can be solved using FFT. 

I computed primes up to 524288 using [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). 524288 ($$2^{19}$$) is the first power of two that is greater than 500000 (maximum $$N$$ value). Because I used FFT to solve the problem, size of many data structures had to be aligned to a power of two. From now on I will refer to 524288 as $$N_{max}$$.

Following three subsections discuss determining numbers that can be expressed as a sum of at most 1, 2 and 3 different prime numbers and calculating number of ways to select these primes.

### 3.1 Computing results for numbers that are a sum of one prime
This case is trivial. Prime numbers in considered range are a sum of one prime and no composite number is a sum of one prime. There is also only one way to select a given prime.

### 3.2 Computing results for numbers that are a sum of two primes
I created a polynomial with coefficient standing near $$x^p$$ equal to 1 if p is a prime number and 0 otherwise:   
$$ P(x) =  \sum_{p=1}^{N_{max}} a_px^p$$, where $$a_p=1$$ if p is a prime number, otherwise $$a_p=0$$.   
Lets consider a polynomial $$P(x)^2$$. It is easy to see that each of monomials $$a_kx^k$$ in $$P(x)^2$$ is a sum of multiplications of $$x^{p_1}$$ and $$x^{p_2}$$ from $$P(x)$$ such that $$p_1 + p_2 = k$$. $$p_1$$ and $$p_2$$ are prime numbers which goes from definition of $$P(x)$$. There are two possible cases for $$p_1$$ and $$p_2$$:  
1. $$p_1$$ and $$p_2$$ are different prime numbers. In this case pair {$$p_1$$, $$p_2$$} will add 2 to $$a_k$$. Once when $$p_1$$ comes from left (and $$p_2$$ from right) polynomial $$P(x)$$ in the product $$P(x) \cdot P(x)$$ and once when $$p_2$$ comes from left (and $$p_1$$ from right) polynomial $$P(x)$$ in the product $$P(x) \cdot P(x)$$  
2. $$p_1=p_2$$. In this case pair {$$p_1$$, $$p_2$$} will add 1 to $$a_k$$.

After above analysis deriving numbers that are a sum of two primes and counting ways of summing is easy:
1. I subtracted 1 from coefficients to which {$$p$$, $$p$$} was added. To do this I subtracted 1 from all monomials $$a_{2p}x^{2p}$$ in $$P(x)^2$$ where p is a prime number and denote the result polynomial as $$P'(x)$$. Value of coefficient $$b_k$$ in monomial $$b_kx^k$$ in $$P'(x)$$ is twice a number of different primes $$p_1$$, $$p_2$$ that sum up to k.  
2. I divided coefficients of $$P'(x)$$ by two. Lets denote the result polynomial as $$P''(x)$$. 

Value of coefficient $$c_k$$ in monomial $$c_kx^k$$ in $$P''(x)$$ is equal to the number of two different primes that sum up to k. 

### 3.3 Computing results for numbers that are a sum of three primes
Computing numbers that are a sum of three different primes is similar to calculations from the previous subsection. 

Lets consider polynomial $$W(x)=P''(x) \cdot P(x)$$. Each non-zero coefficient $$a_k$$ of monomial $$a_k \cdot x^k$$ in $$W(x)$$ is a sum of multiplications of $$b_{l}x^{l}$$ from $$P''(x)$$ and $$x^{p}$$ from $$P(x)$$ such that $$l + p = k$$. Based on previous subsections: $$b_{l}$$ is a number of different primes ($$p_1$$, $$p_2$$) that sum up to $$l$$ and $$p$$ is a prime. Lets consider all primes {$$p_1$$, $$p_2$$, $$p$$} such that $$p_1 + p_2 + p = a_k$$.
There are two possibilities:  
1. $$p$$ is not equal to any of {$$p_1$$, $$p_2$$} - in this case primes {$$p_1$$, $$p_2$$, $$p$$} will add 3 to $$a_k$$. There are three ways to create $$x^k$$ using these numbers: $$x^{p_1 + p_2} \in P''(x)$$ and $$x^{p} \in P(x)$$, $$x^{p + p_2} \in P''(x)$$ and $$x^{p_1} \in P(x)$$, $$x^{p + p_1} \in P''(x)$$ and $$x^{p_1} \in P(x)$$  

2. $$p$$ is equal to one of {$$p_1$$, $$p_2$$} (lets assume that $$p=p_2$$) - in this case primes {$$p_1$$, $$p_2$$, $$p_2$$} will add 1 to $$a_k$$: $$x^{p_1 + p_2} \in P''(x)$$ and $$x^{p_2} \in P(x)$$


To get rid of polynomial part coming from non-unique primes in the form {$$p_1$$, $$p_2$$, $$p_2$$} ($$p_1 \neq p_2$$) I created a helper polynomial $$ H(x) = \sum_{p_1 \in primes} \sum_{p_2 \in primes, p_1 \neq p_2} x^{2p_1+p_2}$$. I created it by multiplying $$H_1(x) = \sum_{p \in primes} x^{2p}$$ by $$P(x)$$ and removing polynomial part coming from primes {$$p$$, $$p$$, $$p$$} ($$1 \cdot x^{3p}$$) in the same way as I removed ($$1 \cdot x^{2p}$$) from $$P(x)$$. The resulting polynomial was equal to $$H(x)$$.  

Having $$H(x)$$ from $$W(x)$$ I computed sums of three primes and counted them in the following way:
1. I subtracted $$H(x)$$ from $$W(x)$$. Value of coefficient $$c_k$$ of monomial $$c_k \cdot x^k$$ in the result polynomial $$W'(x)$$ is the number of unique three primes {$$p_1$$, $$p_2$$, $$p_3$$} such that $$p_1 + p_2 + p_3 = k$$ multiplied by 3.  
2.  I divided coefficients of $$W'(x)$$ by three. Lets denote the result polynomial as $$W''(x)$$. 

Value of coefficient of $$d_k$$ of monomial $$d_k \cdot x^k$$ in $$W''(x)$$ is equal to the number of three different primes that sum up to k. 

## 4. Complexity
The algorithm computes prime numbers in $$O(N_{max} \cdot log\ log\ N_{max})$$ time.   
Adding and subtracting polynomials takes $$O(N_{max})$$ time.  
Multiplying polynomials implemented using fast fourier transform (FFT) takes $$O(N_{max} \cdot log\ N_{max})$$ time.  
Therefore overall algorithm complexity is $$O(N_{max} \cdot log\ N_{max})$$.  

## 5. Implementation 
Implementation is available [here](https://github.com/Caroleq/spoj-solutions/blob/main/jpm2/main.cpp)