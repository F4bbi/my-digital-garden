---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/advanced-algorithms/advanced-algorithms-assignment-1/","created":"2025-11-10T20:18:37.604+01:00","updated":"2026-06-06T10:42:49.939+02:00","dg-note-properties":{}}
---

# Advanced Algorithms Assignment 1

You can find the assignment description [here](/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/advalg-ass1-2025.pdf).

## Exercise 1.

### Exercise 1.1

We can observe that the Load Balancing problem with unsplittable jobs (which we know is NP-complete) is a strictly special case of this extended problem.
For example, we can consider an instance of the extended problem where the set of splittable jobs is empty. In this scenario, the input consists solely of unsplittable jobs, that is the original Load Balancing problem. Any algorithm that can solve the extended problem must also be able to solve this specific instance. Since finding the optimal makespan for an instance with only unsplittable jobs is NP-complete, the general problem containing both types must be at least as hard. Therefore, extended Load Balancing is NP-hard.
Let's prove that the problem also belongs to the class NP. To show this, we frame it as a decision problem: given a time threshold $K$, does a schedule exist such that $T \le K$? A proposed solution (certificate) is the full assignment of unsplittable jobs and the "flow" allocation for splittable jobs.
We can also verify the makespan $T$ for each machine $i$ by computing its total load $T_i$ as the sum of unsplittable processing times and the allocated fractions of splittable jobs. Since all calculations (sums and checking $T \le K$) can be performed in polynomial time relative to the number of jobs $n$ and machines $m$, the extended Load Balancing problem is in NP.
Since the problem is both NP-hard and in NP, the extended Load Balancing problem is NP-complete.

### Exercise 1.2

We can extend the smarter greedy algorithm that first sorts and re-indexes the jobs such that $t_1 \ge \dots \ge t_n$, and then applies the original greedy algorithm, to handle instances that may also contain splittable jobs.

First, we separate the jobs into unsplittable and splittable sets. Let the set of unsplittable jobs be $U$ with processing times ${t_j}_{j \in U}$ and the set of splittable jobs be $S$ with processing times ${s_k}_{k \in S}$. We then order the unsplittable jobs in descending order ($t_1 \ge t_2 \ge \dots \ge t_{|U|}$) and assign each job greedily to the machine $i$ with minimum current load $T_i$, exactly as in the original algorithm.

Let $T_i^\text{unsplit}$ denote the load of machine $i$ after all unsplittable jobs, and let $T_U = \max_i T_i^\text{unsplit}$ be the intermediate makespan.

Next, for the splittable jobs, we calculate the total splittable work $W_S = \sum_{k \in S} s_k$. We then compute the total average load for the entire instance: $L_{avg} = (\sum_i T_i^\text{unsplit} + W_S) / m$. The splittable work is then used to "fill" any machine $i$ whose load $T_i^\text{unsplit}$ is less than $L_{avg}$, raising its load up to $L_{avg}$. If a machine $j$ already has $T_j^\text{unsplit} \ge L_{avg}$, it receives no splittable work.

The final makespan $T$ is therefore the maximum of the original highest unsplittable load and this average load.

$$T = \max(T_U, L_{avg})$$

Now we have to prove that $T \le 1.5 T^*$.

First, we know the guarantee for the unsplittable part: $T_U \le 1.5 \cdot T^*_U$, where $T^*_U$ is the optimal makespan for an instance with only the jobs in $U$.

$T^* \ge T^*_U$ (since the optimum for the full problem cannot be better than the optimum for a sub-problem) and $T^* \ge L_{avg}$ (since the optimum cannot be better than the perfect average).

We can simply check the two cases for our final makespan $T$:

1. If $T = T_U$ the ratio holds, since the makespan is determined by an unsplittable load. We have $T = T_U \le 1.5 T^*_U \le 1.5 T^*$.
    
2. If $T = L_{avg}$, the makespan is determined by the average load. We have $T = L_{avg} \le T^*$. In this case, our solution is optimal.    

In both cases the final makespan $T \le 1.5 \cdot T^*$, so the approximation ratio is maintained.

## Exercise 2.

### Exercise 2.1

We can observe that the function $f(x, y) = \sum_{i=1}^n (|x - x_i| + |y - y_i|)$ is separable. We can rewrite the function by grouping the $x$ and $y$ terms independently:

$$f(x, y) = \left( \sum_{i=1}^n w_i |x - x_i| \right) + \left( \sum_{i=1}^n w_i |y - y_i| \right)$$

Let $f_x(x)$ denote the $x$-dependent term and $f_y(y)$ denote the $y$-dependent term. To minimize the total function $f(x, y) = f_x(x) + f_y(y)$, we can find the coordinate $x^*$ that minimizes $f_x(x)$ and the coordinate $y^*$ that minimizes $f_y(y)$ completely separately. This reduces the 2D problem to two independent 1D problems, that are basically the weighted median problem.

We now describe the algorithm to find the optimal $x^*$; the algorithm for $y^*$ is identical. First, we calculate the total weight $W = \sum_{i=1}^n w_i$. We then create a list of pairs $(x_i, w_i)$ and sort this list in non-decreasing order based on the $x_i$ coordinates.

Next, we iterate through this sorted list, maintaining a cumulative weight sum $S$, initialized to zero. For each point $(x_k, w_k)$ in the sorted list, we add its weight $S = S + w_k$. The first coordinate $x_k$ for which this cumulative sum reaches or exceeds half the total weight (i.e., $S \ge W/2$) is the weighted median. We set $x^* = x_k$ and terminate the 1D search. The final optimal point is $(x^*, y^*)$.

Correctness and time bound:
The correctness of separating the problem follows from the additive nature of the Manhattan distance.
The correctness of the algorithm relies on $f_x(x)$ being a convex function. Let's study the derivative to find the minimum.

We begin by expressing the derivative of the sum as the sum of the derivatives, noting that $\frac{d}{dx}|x - x_i| = \text{sgn}(x - x_i)$:

$$f_x'(x) = \sum_{i=1}^n w_i \cdot \text{sgn}(x - x_i)$$

For any point $x$ not equal to any $x_i$, the function is differentiable. We divide the sum into two groups:

- Group 1 ($x > x_i$): then $\text{sgn}(x - x_i) = +1$.
    
- Group 2 ($x < x_i$): then $\text{sgn}(x - x_i) = -1$.

Substituting the signs:

$$f_x'(x) = \sum_{x_i < x} w_i \cdot (+1) + \sum_{x_i > x} w_i \cdot (-1)$$

$$f_x'(x) = \sum_{x_i < x} w_i - \sum_{x_i > x} w_i$$

The function $f_x(x)$ is minimized at the point $x^*$ where the difference is zero, meaning the total weight "pulling" to the left equals the total weight "pulling" to the right. This transition occurs precisely at the weighted median $x_k$ because, by definition, $x_k$ is the first point where the cumulative weight $S$ reaches $W/2$.

For the time bound, the algorithm is dominated by the sorting step. Let $n$ be the number of points. Calculating $W$ takes $O(n)$ time. Sorting the $n$ pairs takes $O(n \log n)$ time. The final pass to find the median takes $O(n)$ time. Since this procedure is run twice, the total time complexity is $O(n \log n) + O(n \log n) = O(n \log n)$.

Actually, this bound can be improved to $O(n)$ using a Weighted Quickselect algorithm. To guarantee this linear performance and avoid the $O(n^2)$ worst case, a non-unbalanced pivot is chosen in linear time using the Median of Medians algorithm as a pivot selection strategy. This reduces the total time complexity for the 2D problem to $O(n) + O(n) = O(n)$, which is asymptotically optimal, although the two algorithms are not so easy to implement.

### Exercise 2.2

Let:
- $p = (x, y)$ be any point in the plane
- $p_i = (x_i, y_i)$ be the $i$-th site
- $f_e(p) = \sum_{i=1}^n w_i \cdot \sqrt{(x - x_i)^2 + (y - y_i)^2}$ the true Euclidean cost function
- $f_m(p) = \sum_{i=1}^n w_i \cdot (|x - x_i| + |y - y_i|)$ be the Manhattan cost function
- $p_{e_*}$ be the point that truly minimizes the Euclidean cost $f_e(p)$
- $p_{m_*}$ be the point found by our algorithm, which minimizes the Manhattan cost $f_m(p)$.

Our goal is to prove that the Euclidean cost of our algorithm's solution is at most $\sqrt{2}$ times the Euclidean cost of the true optimal solution, that is, $f_e(p_{m_*}) \le \sqrt{2} \cdot f_e(p_{e_*})$.

We must use two fundamental geometric inequalities that relate the Euclidean distance ($d_e$) and Manhattan distance ($d_m$) between any two points: 
1. $d_e \le d_m$, or $\sqrt{\Delta x^2 + \Delta y^2} \le \Delta x + \Delta y$
2. $d_m \le \sqrt{2} \cdot d_e$, or $\Delta x + \Delta y \le \sqrt{2} \cdot \sqrt{\Delta x^2 + \Delta y^2}$

By the definition of $f_e$, we have $f_e(p_{m_*}) = \sum_{i=1}^n w_i \cdot d_e(p_{m_*}, p_i)$. Applying inequality 1 ($d_e \le d_m$) to every term in the sum, this is less than or equal to $\sum_{i=1}^n w_i \cdot d_m(p_{m_*}, p_i)$, which is precisely the definition of $f_m(p_{m_*})$. This gives us $f_e(p_{m_*}) \le f_m(p_{m_*})$.

Next, we can use the key property of our algorithm, that is $p_{m_*}$ is the point that minimizes $f_m$. Therefore, $f_m(p_{m_*})$ must be less than or equal to the Manhattan cost of any other point, including the true Euclidean optimal point $p_{e_*}$. This is $f_m(p_{m_*}) \le f_m(p_{e_*})$.

Now, we apply inequality 2 ($d_m \le \sqrt{2} \cdot d_e$) to every term in the sum $f_m(p_{e_*}) = \sum_{i=1}^n w_i \cdot d_m(p_{e_*}, p_i)$. This yields $\sum_{i=1}^n w_i \cdot \sqrt{2} \cdot d_e(p_{e_*}, p_i)$. By factoring out the $\sqrt{2}$ constant, we get $\sqrt{2} \cdot \sum_{i=1}^n w_i \cdot d_e(p_{e_*}, p_i)$, which is by definition equal to $\sqrt{2} \cdot f_e(p_{e_*})$.