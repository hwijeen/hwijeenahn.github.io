---
layout: post
title: "Theory of NP"
description: "P, NP, NP-complete, NP-hard"
comments: true
categories: [기타]
tags:
- Algorithm
---



From Foundations of Algorithms by Richard E.Neapolitan.

## Intractability

- **Polynomial-time algorithm** is one whose worst-cast time complexity is bounded by a polynomial function of its input size.
- A problem is **intractable** if it is impossible to solve it with a polynomial-time algorithm.
- Three feneral categories of problems as far as intractability is concerned:
  - Problems for which polynomial-time algorithms have been found
  - Problems that have proven to be intractable
  - Problems that have not been proven to be intractable, but for which polynomial-time algorithms have never been found

- All problems that to this date have been proven intractable have also been proven not to be in the set NP. 

- Most problems that appear to be intractable are in the set NP.



## Theory of *NP*

- We restrict ourselves to decision problems. 
  - Each optimization problem has a corresponding decision problem.
  - A solution to an optimization problem produces a solution to the correspnding decision problem.
  - For many decision problems, it has been shown that a polynomial-time algorithm for the decision problem would yield a polynomial-time algorithm for the corresponding optimization problem.

### The sets *P* and *NP*

- ***P*** is the set of all decision problems that can be solved by polynomial-time algorithms.
  - For some problems, we simply do not know if there exists an polynomial-time algorithm. 
- **Nondeterministic algorithm** simply produces some string S. The string can be though of a guess at a solution to the instance. However, it could just be a string of nonsense. 
  - It is called nondeterministic because unique step-by-step instructions are not specified for it.
  - It is simply a defitional device for the purpose of obtaining the notion of polynomial time verifiability.  It is not a realistic method for solving a decision problem. 
- **Polynomial time verifiability**
- **A polynomial-time nondeterminsitic algorithm** is a nondeterministic algorithm whose verification stage is a polynomial-time algorithm.
- ***NP*** is the set of all decision problems that can be solved by polynoial-time nondeterministic algorithms.
  - This does not mean that problems in this set can necessarily be solved in polynomial time.
  - The purpose of introducing the concepts of nondeterministic algorithm and NP is to classify algorithms.
- There are thousand of problems that no one has been able to solve with polynomial-time algorithms but that have been proven to be in NP because polynomial-time nondeterministic algorithms have been developed for them.
- Trivially, every problem in P is also in NP.
- The only decision problems that have been proven not to be in NP are the same ones that have been proven to be intractable. e.g) Halting problem.
- No one has ever proven that there is a problem in NP that is not in P. If P = NP, we would have polynomial-time algorithms for most known decision problems (in NP).

### *NP*-complete problems

- **CNF-satisfiability problem**
  - This is NP problem.
  - We do not know if it is in P.
  - If this is in P, then P = NP.
- **Transfomation algorithm** creates an instance y of problem B from every instance x of problem A such that an algorithm for problem B answers "yes" for y if and only if the answer to problem A is "yes" for x.![image-20190904151828561](/Users/hwii/Library/Application Support/typora-user-images/image-20190904151828561.png)

- If there exists a polynomial-time transformation algorithm from decision problem $$A$$ to decision problem B, problem A is **polynomial-time many-one reducible** to problem $$B$$.
  - If the decision probem B is in P and A $$\propto$$ B, then decision problem A in in P.

- A problem B is called ***NP*-complete** if both of the following are true:
  - B is in NP.
  - For every other problem in A NP, A $$\propto$$ B.
- If we could show that any NP-complete problem is in P, we could conclude that P = NP.
- CNF-satisfiabilty problem is proven to be NP-complete.
- A problem C is NP-complete if both of the following are true:
  - C is in NP.
  - For some other NP-complete problem B, B $$\propto$$ C.

### The state of NP

![image-20190904153207321](/Users/hwii/Library/Application Support/typora-user-images/image-20190904153207321.png)

- P is a proper subset of NP. It may be true that they are the same.
- NP-complete is a subset of NP by definition.
- A decision problem that is in NP and is not NP-complete is the trivial decision problems that answers "yes" for all instances.
  - It is not NP-complete because it is not possible to transform a nontrivial decision problem to it.
- If P $$\neq$$ NP, P $$\cap$$ NP-complete = $$\emptyset$$. This is so because if some problem in P were NP-complete, it is implied that we could sove any problem in NP in polynomial time.
- If P $$\neq$$ NP, NP - (P $$\cup$$ NP-compete ) is not empty.

### *NP*-Hard

- If problem A can be solved in polynomial time using a **hypothetical** polynomial time algorithm for problem B, the problem A is **polynomial-time Turing reducible** to problem B. 
- If A $$\propto$$ B, then A $$\propto_{T}$$ B.
- A problem B is called **NP-hard**, if for some NP-complete problem A, A $$\propto_{T}$$ B.
- NP problem A $$\propto_{T}$$ NP-complete problem B $$\propto_{T}$$ NP-hard problem C.
  - All problems in NP reduce to any NP-hard problem.
  - If a polynomial-time algorithm exists for any NP-hard problem, then P = NP.
- Every NP-complete problem is NP-hard.
  - The optimization problem corresonding to any NP-complete (decision) problem is NP-hard.
- If we were to prove that some problem was not NP-hard, we would be proving that P $$\neq$$ NP.
  - If P = NP, all problems in NP can be solved with a polynomial-time algorithm. Then, all problems would be NP-hard.
- If we were to prove that some problem for which we had a polynomial-time algorithm was NP-hard, we would be proving the P = NP.
  - NP $$\propto_{T} $$ NP-hard이니까 NP 문제를 polynomial time에 풀어버리니까

![image-20190904155131859](/Users/hwii/Library/Application Support/typora-user-images/image-20190904155131859.png)



![File:P np np-complete np-hard.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/P_np_np-complete_np-hard.svg/800px-P_np_np-complete_np-hard.svg.png)