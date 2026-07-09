---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/advanced-algorithms/advanced-algorithms-assignment-3/","created":"2025-12-05T18:09:51.875+01:00","updated":"2026-07-09T15:07:59.769+02:00","dg-note-properties":{}}
---

# Advanced Algorithms Assignment 3

Here's the assignment description.

<iframe src="/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/advalg-ass3-2025.pdf" width="100%" height="900px" title="advalg-ass3-2025.pdf" style="border:1px solid #ccc;"></iframe>

## Exercise 5

### Exercise 5.1

Assumption: Without loss of generality, we assume the elements are distinct. This represents the strict worst-case scenario, as the presence of equal elements would only reduce the complexity of the decision tree.

For $n = 3$ distinct elements, there are $3! = 6$ possible total orderings. A deterministic algorithm follows a fixed path in a binary decision tree, where each comparison has two possible outcomes ($<$ or $>$).

Therefore, a deterministic comparison-based algorithm can distinguish at most:
- 2 scenarios after 1 comparison ($2^1$)
- 4 scenarios after 2 comparisons ($2^2$)
- 8 scenarios after 3 comparisons ($2^3$)

With only 2 comparisons, the algorithm can reach at most 4 distinct leaves in the decision tree. However, an adversary can force the algorithm into a state where ambiguity remains between orderings that yield different medians.

Analysis of the worst path:
1. Comparison 1 ($x$ vs $y$): we suppose the outcome is $x < y$.
2. Comparison 2: The algorithm must compare the third element $z$ with one of the others. 
    - If it compares $z$ vs $y$ and finds $z < y$, we know $y$ is the maximum. However, the relative order of $x$ and $z$ is unknown. The true order could be $x < z < y$ (median is $z$) or $z < x < y$ (median is $x$).
    - If it compares $z$ vs $x$ and finds $z > x$, we know $x$ is the minimum, but the order of $y$ and $z$ is unknown. The median could be $y$ or $z$.

In this worst-case scenario, after 2 comparisons, the set of consistent orderings still contains candidates that yield different medians. Therefore, a third comparison is strictly necessary to resolve this final ambiguity.

### Exercise 5.2

Let the set of elements be $S = \{x, y, z\}$. We perform this steps:

1. We start by selecting one element $k \in S$ uniformly at random. Let the other two elements be $a$ and $b$.  
2. We compare $a$ with $k$ and $b$ with $k$. This costs 2 comparisons.
3. Based on the results of the previous step:
    - If one element is smaller than $k$ and the other is larger than $k$ (e.g., $a < k < b$), then $k$ is the median. We can terminate. In this case we would do only 2 comparisons.
    - If both elements are smaller than $k$ (i.e., $a < k$ and $b < k$), then $k$ is the maximum. We have to perform one additional comparison between $a$ and $b$ to determine the median. In this case we would do 3 comparisons.
    - If both elements are larger than $k$ (i.e., $a > k$ and $b > k$), then $k$ is the minimum. We have to perform one additional comparison between $a$ and $b$ to determine the median. In this case we would do 3 comparisons.

We now calculate the expected number of comparisons. Let the true sorted order of the elements be $s_0 < s_1 < s_2$, where $s_1$ is the median (the algorithm does not know this order, but the randomness depends only on our choice of $k$, not on the input values).
Since we choose $k$ uniformly at random from $\{s_0, s_1, s_2\}$, there are three equally likely (each with probability $1/3$) cases for $k$:

1. Case 1: $k$ is the minimum ($k = s_0$).
    In this case, the other two elements are $s_1$ and $s_2$, and they are both are larger than $k$. The algorithm detects $k$ is the minimum (step 2) and must compare $s_1$ vs $s_2$ (step 3) to find the median. The cost is 3 comparisons.
2. Case 2: $k$ is the maximum ($k = s_2$).
    In this case, the other two elements are $s_0$ and $s_1$, both are smaller than $k$. The algorithm detects $k$ is the maximum (step 2) and must compare $s_0$ vs $s_1$ (step 3) to find the median.
    The cost is 3 comparisons.
3. Case 3: $k$ is the median ($k = s_1$).
    In this case, one remaining element is smaller ($s_0$) and one is larger ($s_2$). The comparisons in step 2 will reveal $s_0 < k$ and $s_2 > k$. The algorithm correctly identifies $k$ as the median immediately. No third comparison is needed. The cost is 2 comparisons.

So the expected number of comparisons is:

$$\frac{1}{3} \cdot 3 + \frac{1}{3} \cdot 3 + \frac{1}{3} \cdot 2 = \frac{8}{3} = 2.66 < 3$$

The bound holds for any input since the distribution of the $k$ is always uniform, regardless of the specific values or their initial arrangement.

### Exercise 5.3

To find the minimum of $n=3$ elements deterministically we need 2 comparisons in the worst case.
Let the set of elements be $S = \{x, y, z\}$. We can use a simple algorithm: we compare $x$ vs $y$, then compare the smaller of those two with $z$. After these two comparisons we know the minimum. We cannot do it with only one comparison in the worst case because one comparison only gives the order of two elements and leaves the third element unconstrained.
Randomization cannot reduce the expected number of comparisons below 2.
This is because a Las Vegas algorithm must always return the correct minimum for every possible input ordering. Let's consider the first comparison the algorithm makes: it compares two elements, we call them $a$ and $b$. There are two possible outcomes, $a<b$ or $b<a$.
Let's analyze the condition $a<b$. Given only this information, the third element $c$ could still be:
- smaller than $a$, so the minimum is $c$
- between $a$ and $b$, so the minimum is $a$
- or larger than $b$, so the minimum is $a$

Thus the single outcome $a<b$ is consistent with at least two different orderings that have different minimum. Therefore the algorithm cannot safely stop and declare the minimum after only this one comparison, because that would make it incorrect on some input consistent with the observed outcome. The same argument applies to the other outcome $b<a$.
Hence every run uses 2 comparisons, so the expected number of comparisons is $2$.

### Optional

The case of $n=3$ can be generalized to arbitrary $n$. For the problem of finding the minimum, $n-1$ comparisons are necessary. We can view the set of candidates for the minimum as a pool of size $n$. To reduce this pool to a single element (the true minimum), $n-1$ elements must be eliminated. Since a single pairwise comparison eliminates exactly one candidate (the larger of the two), and no comparison can eliminate two candidates simultaneously, any algorithm must perform exactly $n-1$ comparisons to guarantee a correct result. Unlike the median problem, the problem of finding the extremum is rigid: the progress rate is fixed at one elimination per step. 

If we consider the problem of selecting the median for general $n$, efficient linear-time algorithms exist for both deterministic and randomized approaches. The randomized approach is exemplified by Quickselect, which achieves an expected time complexity of $O(n)$. Although its worst-case performance can degrade to $O(n^2)$, it is highly efficient (due to the low constant factors) and simpler to implement in practice. A deterministic counterpart is the Median-of-Medians, that guarantees finding the median in $O(n)$ worst-case time.
From a theoretical standpoint, any comparison-based algorithm that selects the exact median must perform $\Omega(n)$ comparisons, as every element must be processed to determine its rank relative to the pivot. Therefore, randomization in this context does not lower the asymptotic complexity class below linear (it cannot beat $\Omega(n)$). Instead, its primary advantage lies in simplifying the algorithm and achieving excellent expected-time bounds without the high constant factors and implementation complexity associated with the deterministic Median-of-Medians algorithm.

### Exercise 6

The problem bears a structural resemblance to the classic Travelling salesman problem, as it requires finding a minimum-cost tour covering a set of vertices. However, two distinct characteristics fundamentally alter the approach. First, the turn costs means the cost of entering a node depends on the edge used to reach it. Consequently, the standard TSP state $(S, v)$, where $S$ is the set of visited nodes and $v$ is the current node, is insufficient because it lacks the "memory" of the previous location required to calculate the turn cost $t((u, v), (v, w))$.
Second, the requirement to visit every node **at least once** (rather than exactly once) fundamentally changes the topological structure of the state space. In a standard TSP, the set of visited nodes $S$ strictly grows with every step, forming a Directed Acyclic Graph. In this variation, the optimal path allows revisiting nodes to perform cheaper turns or traverse lower-cost edges. This means a transition can lead from a state with subset of visited nodes $S$ to a new state with the same subset $S$, introducing cycles into the state space. Because the graph of states contains cycles but all edge and turn costs are non-negative, we can model the problem as a shortest-path problem on a big weighted graph. We can therefore employ Dijkstra’s Algorithm over the state space $(S, u, v)$ to guarantee finding the global minimum, where $u$ represents the predecessor node.

#### Pseudocode of the algorithm
```python
function solution(V, E, c, t):
	Dist = HashMap() # Where key = (subset, previous, current) and value = cost
	PQ = PriorityQueue() # We will push (cost, (subset, previous, current))
    Parent = HashMap() # To reconstruct the path if needed

	# Initialization: The path can start with any single edge in the graph.
	# An edge (u, v) implies nodes u and v are visited.
	for each (u, v) in E:
	    # In an actual implementation, we would use bitmasking for efficiency
	    # e.g., subset = (1 << u) | (1 << v)
	    subset = {u, v}
	    cost = c(u, v)
	    state = (subset, u, v)
	    Dist[state] = cost
        Parent[state] = None
	    PQ.push(cost, state)
	
	while PQ is not empty:
	    current_cost, current_state = PQ.pop()
        subset, u, v = current_state
	
	    # Deletion for outdated entries in PQ
	    if current_cost > Dist[current_state]:
	        continue
	
	    # Termination condition: All nodes visited
	    if subset == V:
	        return Parent, current_state, current_cost
	
	    for each neighbor w of v:
	        # Calculate total step cost: Turn (u->v->w) + Edge (v->w)
	        step_cost = t((u, v), (v, w)) + c(v, w)
	        new_cost = current_cost + step_cost
	        
	        # Update visited set
	        # In an actual implementation, we would use bitmasking for efficiency
	        # e.g., new_subset = subset | (1 << w)
	        new_subset = subset ∪ {w}
	        new_state = (new_subset, v, w)
			
			# Relaxation step
	        # If we find a cheaper way to reach new_state, update and push to PQ
	        if new_cost < Dist[new_state]:
	            Dist[new_state] = new_cost
                Parent[new_state] = current_state
	            PQ.push(new_cost, new_state)
	
	return HashMap(), Null, Infinity # Should not be reached if graph is connected
```

The algorithm treats the problem as a search for the shortest path in a "state graph." A specific node in this abstract graph is defined by the triplet $(S, u, v)$, representing a partial path that covers the set of nodes $S$, currently ends at node $v$, and arrived there immediately from node $u$. We begin by identifying all possible starting moves. Since the path is undirected and has no prescribed start, any edge $(u, v)$ in the original graph is a valid beginning of a tour. We initialize the priority queue with states corresponding to traversing every single edge, setting the initial visited subset to include both endpoints.
The core of the algorithm proceeds via Dijkstra's method. In each iteration, we extract the state with the lowest accumulated cost found so far. From the current node $v$, we explore all outgoing edges to neighbors $w$. For every transition, we calculate the cost to extend the path, which is the sum of the edge cost $c(v, w)$ and the turn cost $t((u, v), (v, w))$ associated with the sequence $u \to v \to w$. We then determine the new state: the current node becomes $w$, the previous node becomes $v$, and the visited subset is updated. In an actual implementation, we would use bitmasking and the bitwise OR operation to ensure that if $w$ was already in the set $S$, the mask would remain unchanged. This handles the "at least once" constraint, allowing the algorithm to revisit nodes without breaking the logic. If this new path to the new state is cheaper than any previously found path to that same state, we update the distance and push the new state into the priority queue. The process repeats until we extract a state where the mask $S$ contains all nodes in the graph.

In case the sequence of nodes corresponding to the minimum cost is needed, we maintain a Parent map (that is a predecessor array). Whenever a state $(S', v, w)$ is updated during the relaxation step (i.e., a cheaper path is found), we record the predecessor state $(S, u, v)$ that led to it (`Parent[(S', v, w)] = (S, u, v)`).
Once the target state $(V, u_{last}, v_{last})$ is reached, we backtrack using these pointers. Starting from the final state, we iteratively look up the parent state until we reach the initial edge. This sequence of states is then reversed to produce the optimal ordered list of visited nodes. Crucially, storing the full state (subset and previous node) as the parent, rather than just the node index, allows us to correctly unroll cycles in the physical graph into a simple path in the state space.

#### Proof of Correctness

The correctness of this approach rests on the reduction of the original problem to a Shortest Path problem on a weighted directed graph. Let $\mathcal{G}$ be a graph where every vertex is a valid state $(S, u, v)$ and edges represent valid physical moves in the original graph. The weight of an edge in $\mathcal{G}$ connecting state $(S, u, v)$ to $(S \cup \{w\}, v, w)$ corresponds exactly to the physical cost of traversing edge $(v, w)$ plus the specific turn cost incurred at $v$.

Since all edge costs and turn costs are given as non-negative numbers, there are no negative cycles in this state graph $\mathcal{G}$. Dijkstra’s algorithm is proven to find the shortest path from a source set to a destination in any graph with non-negative edge weights. The relaxation property ensures that for every state extracted from the priority queue, the cost is the global minimum to reach that specific configuration of visited nodes and orientation. The algorithm terminates the first time it extracts a state where the mask $S$ corresponds to the set of all vertices $V$. Because Dijkstra extracts states in strictly non-decreasing order of total cost, the first time this condition is met represents the cheapest possible cost to visit all nodes. The ability to revisit nodes (where the mask $S$ does not change size) is handled correctly because such transitions are simply edges in $\mathcal{G}$; infinite cycles of re-visitation are avoided because they would strictly increase the cost without changing the state, meaning they would never be prioritized over the simpler path.

#### Time Complexity

To determine the time complexity, we analyze the size of the state graph $\mathcal{G}$ and the number of edges within it. The number of possible subsets $S$ is $2^n$, where $n$ is the number of nodes. For each subset, the path tracks the current node $v$ ($n$ possibilities) and the previous node $u$ (at most $n$ possibilities, bounded by the degree of $v$). Thus, the total number of vertices in the state graph is bounded by $|V_{\mathcal{G}}| = O(n^2 2^n)$.

From any given state $(S, u, v)$, the algorithm attempts to transition to every neighbor $w$ of $v$. In the worst case (a complete graph), there are $n-1$ neighbors. Therefore, the number of edges in the state graph is $|E_{\mathcal{G}}| = O(n \cdot |V_{\mathcal{G}}|) = O(n^3 2^n)$. Dijkstra’s algorithm implemented with a binary heap has a complexity of $O(E \log V)$. Substituting our values, we get $O(n^3 2^n \cdot \log(n^2 2^n))$. Since $\log(n^2 2^n) = 2\log n + n \approx O(n)$, the strictly polynomial factors are dominated by the exponential term. The path reconstruction step would take time linear in the length of the optimal path, thus it would not affect the overall asymptotic time complexity. Using the $O^*$ notation, the time complexity is $O^*(2^n)$.