---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/advanced-algorithms/approximation-algorithms/","created":"2025-11-07T17:45:12.559+01:00","updated":"2026-06-06T10:42:59.900+02:00"}
---

# Approximation Algorithms (or “My Problem is NP-Complete — Now What?”)

Many combinatorial problems that arise in practice are **NP-complete**, meaning that, under widely believed assumptions, they cannot be solved exactly in polynomial time. Nevertheless, these problems are ubiquitous in real-world applications, and we often need practical solutions.

One approach, particularly for **optimization problems**, is to **compute approximate solutions** efficiently, that is, solutions that are close to optimal, though not necessarily exactly optimal. Approximation algorithms provide such solutions in polynomial time. While the solution may not be perfect, it is often sufficiently good for practical purposes.

## Load Balancing

Consider the following classical optimization problem:

- We have **n jobs**, each with a processing time $t_j$ for $j = 1, \dots, n$.
    
- These jobs must be executed on **m machines**.
    
- Let $T_i$ denote the **load** of machine $i$, i.e., the sum of the processing times of the jobs assigned to it:
    
$$  
T_i = \sum_{j \in J_i} t_j,  
$$

where $J_i$ is the set of jobs assigned to machine $i$.

The **objective** is to **minimize the maximum load** among all machines:

$$  
T := \max_{i=1,\dots,m} T_i.  
$$

Here, $t_j$ and $m$ are given, and the $T_i$ values depend on the assignment of jobs to machines.

### Important Constraints

1. Each job must be assigned **completely to a single machine**; jobs cannot be split across multiple machines. These are known as **unsplittable jobs**.
    
2. The **makespan**, $T$, represents the total time until all jobs are completed if each machine executes its assigned jobs in some order starting from time $0$. Minimizing the makespan ensures the earliest possible completion time.
    

> Informally, good load balancing also ensures fairness if the machines are operated by human workers rather than automated systems.

### Complexity

Unfortunately, **Load Balancing is NP-complete**, even when $m = 2$ machines. The problem closely resembles the **Subset Sum** problem, and NP-completeness can be formally shown via a polynomial-time reduction from Subset Sum.

Thus, finding the exact optimal assignment is computationally infeasible for large instances. Instead, we turn to **approximation algorithms** that run in polynomial time and produce solutions that are provably close to optimal.

### Approximation Notation

We denote the **optimal makespan** for a given instance as $T^*$. This “star” notation is standard in optimization problems. In general, our algorithms will produce a makespan $T$ such that

$$  
T \approx T^*
$$

### Greedy Algorithm for Load Balancing

A natural idea for an algorithm is **greedy assignment**:

> Pass through the jobs one by one and assign each job to the machine that currently has the smallest load.

Due to the NP-completeness of the problem, this algorithm **cannot always produce the optimal solution**, and in fact, there exist small counterexamples.

#### Example

Consider $m = 2$ machines and $n = 5$ jobs with processing times $(3, 3, 2, 2, 2)$.

- The **optimal solution** has makespan $T^* = 6$, obtained by assigning jobs $3$ and $3$ to one machine, and $2, 2, 2$ to the other.
    
- In general, it may not be possible to achieve a perfectly balanced load of $\text{average load} = \frac{\sum_{j=1}^n t_j}{m}$ on every machine.
    

Despite this, the greedy algorithm **may perform reasonably well in practice**. But how can we analyze its performance rigorously?

#### From Intuition to Worst-Case Analysis

To evaluate the quality of the greedy algorithm, we need a precise notion of performance. A natural question is:

> How far can the greedy solution be from an optimal solution in the **worst case**, considering all possible instances?

Worst-case analysis provides **reliability guarantees**, just as worst-case time complexity provides guarantees on the running time of an algorithm.

However, we still need a **measure of distance between solutions**.

A simple difference, $T - T^*$, is not a good measure. Why? Consider scaling an instance by a factor $c$ (multiplying all job times by $c$). Then the makespan is also scaled by $c$, but the problem itself is unchanged. This implies that

$$  
\max_{\text{all instances}} (T - T^*) = \infty,  
$$

making the difference unbounded and unsuitable for analysis.

#### Approximation Ratio

A more robust measure is the **relative error**, or **approximation ratio**:

$$  
\text{Approximation ratio} := \frac{T}{T^*}.  
$$

- Ideally, we want this ratio to be **as close to $1$ as possible**.
    
- Analyzing the exact ratio $T / T^*$ for every instance is usually too complex, as it depends on many small details of each instance.
    

Instead, we aim to find **upper bounds** on $T / T^*$.

#### Upper and Lower Bounds

Even though $T^*$ is generally unknown, we can still bound $T / T^*$ by:

1. Finding a **lower bound** on $T^*$ (based on properties of the problem, independent of any algorithm).
    
2. Finding an **upper bound** on $T$ (based on the output of the specific algorithm).
    

Then we can assert:

$$  
\frac{T}{T^*} \leq \frac{\text{upper bound on } T}{\text{lower bound on } T^*}.  
$$

> ⚠️ Important: The situation is **not symmetric**. The lower bound relates to optimal solutions (a property of the problem), while the upper bound relates to the algorithm's output (a property of the algorithm).

#### Lower Bound Analysis

We start by analyzing the **optimal makespan** $T^*$. This is the easier part, as it depends only on properties of the problem, not on the algorithm.

Two **obvious lower bounds** follow directly from the problem:

1. The **largest job length**:
    
$$  
T^* \ge \max_{j=1,\dots,n} t_j  
$$

2. The **average load per machine**:
    
$$  
T^* \ge \frac{1}{m} \sum_{j=1}^{n} t_j  
$$

While these bounds are correct, the averaging bound can be **too optimistic**. For example, if there are many small jobs and one very large job, dividing the total load by $m$ underestimates the minimal possible makespan.

#### Upper Bound Analysis

Any **upper bound** on $T$ must account for the **behavior of the algorithm**. A natural question is:

> What specifically causes the output to be “bad”?

To answer this, we examine the **last job that finishes**:

- Let $i$ be the machine that finishes **last**, so $T = T_i$.
    
- Let $j$ be the **last job on machine $i$**.
    

> ⚠️ Note: Job $j$ is not necessarily the last job in the list of jobs, because jobs may be scheduled on other machines afterward without increasing the makespan.

A schematic representation:

```
i |----|--|---|---|-----j-------|
```

##### Why was job $j$ assigned to machine $i$?

Job $j$ was assigned to machine $i$ because **machine $i$ had the smallest load** at that moment. Let the load of $i$ before assigning $j$ be $T - t_j$.

By the greedy rule, all other machines $k$ must satisfy:

$$  
T_k \ge T - t_j \quad \text{for all } k = 1, \dots, m  
$$

If we sum the loads of all machines, we get:

$$  
\sum_{k=1}^{m} T_k \ge m (T - t_j)  
$$

Why? Because each machine $k$ contributes at least $T - t_j$, and there are $m$ machines.

Dividing both sides by $m$ and using the **averaging lower bound** gives:

$$  
T - t_j \le \frac{1}{m} \sum_{k=1}^{m} T_k \le T^*  
$$

Since also $T^* \ge t_j$, we finally obtain:

$$  
T \le T^* + t_j \le 2 T^*  
$$

Thus, the **approximation ratio** of the greedy algorithm satisfies:

$$  
\frac{T}{T^*} \le 2  
$$

This is a remarkably simple result: using only **elementary reasoning**, we have proven that the greedy algorithm **never produces a makespan more than twice the optimal**.

#### Tightness of the Bound

The factor $2$ may seem disappointing, the solution could be **twice as long as optimal**. Can the algorithm be improved? Or is it actually better than this analysis suggests?

It turns out the analysis is essentially **tight**. There exist instances where $T$ is **arbitrarily close to $2 T^*$**. A typical “nasty” instance consists of:

- Many short jobs, which the greedy algorithm assigns in a balanced way.
    
- Followed by **one very long job**, which must be assigned to a single machine, destroying the balance.
    

With the appropriate number and lengths of short jobs, the approximation ratio can approach $2$. The precise construction is skipped here, but the **key idea** is simple: the last, long job dominates the makespan.

---

> “I’m so proud: I have finished my puzzle in only 2 years.  
> On the package it says 3–4 years.”

---