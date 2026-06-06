---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/advanced-algorithms/advanced-algorithms-home-exam/","created":"2026-01-01T18:15:25.987+01:00","updated":"2026-06-06T10:42:57.257+02:00"}
---

# Advanced Algorithms Home Exam

You can find the assignment description [[advalgexam2025.pdf|here]].

## Solution

This problem bears a structural resemblance to the classic Traveling Salesperson Problem, as it requires finding a minimum-cost path that covers a specific set of required elements. However, instead of visiting single vertices, we are required to traverse at least one internal edge within each cluster. Additionally, unlike the standard TSP which requires a closed tour, this problem seeks an open path with unspecified start and end points.
Despite these structural differences, the core complexity of ordering the visits remains.

We first prove that the problem is NP-hard and then discuss the complexity of its decision version.

### Proof of NP-Hardness

To prove the NP-Hardness of our problem, we perform a reduction from the Open Path Traveling Salesperson Problem (or Open Loop TSP). Unlike the classic TSP which seeks a closed tour, the Open Path TSP seeks a minimum-cost open path that visits every vertex exactly once. This problem is known to be NP-Hard.[^15]
Let $G = (V, E)$ be an instance of Open Path TSP with edge weights $w$. We can construct an instance of our problem on a graph $G' = (V', E')$ as follows:

1. For every node $i$ in the Open Path TSP, create a cluster $C_i$ in $G'$.
    
2. Inside each cluster $C_i$​, introduce two vertices connected by a single internal edge $e_i$ of cost 0.
    
3. For every edge $(i, j)$ in the Open Path TSP graph with weight $w_{ij}$, create an (external) edge connecting a vertex of cluster $C_i$ to a vertex of cluster $C_j$ with the same cost $w_{ij}$.

Any valid solution to the constructed instance must traverse the unique internal edge of every cluster. Since all internal edges have zero cost, minimizing the total cost depends entirely on the external edges used to move between clusters. Consequently, any minimum-cost path that visits all clusters in $G'$ corresponds directly to a minimum-cost path visiting all vertices in the original Open Path TSP instance.
Since this construction can be performed in polynomial time, the problem is NP-hard.

However, since the problem is an optimization problem (requiring the minimization of a cost function), it does not belong to NP (and consequently neither NP-Complete), as there is no known polynomial-time method to verify that a given candidate solution is indeed the global optimum. However, as with the Open Path TSP, the corresponding decision version of the problem is NP-Complete, as detailed below.

### NP-Completeness of the Decision Version

We now consider the decision version of the problem: given a bound $K$, determine whether there exists a path of total cost at most $K$ that traverses at least one internal edge in each cluster.

A candidate solution can be represented as a sequence of vertices  $P = (v_0, v_1, \dots, v_k)$.

Given such a path, verification can be performed in polynomial time by:

1. Summing the weights of the edges in $P$ to check whether the total cost is at most $K$.
    
2. Checking that, for every cluster $C_i$​, the path contains at least one edge $(u,v)$ with $u, v \in C_i$.

Therefore, the decision version of the problem belongs to NP. Since the problem is NP-hard, the decision version is NP-complete.

### Algorithm for Exact Solution

We can now devise an algorithm that yields an exact solution, regardless of its computational efficiency. We can leverage a dynamic programming approach similar to the one used in Exercise 6 of the third assignment. Similarly, a transition can lead from a state with a subset of visited clusters to a new state with the same subset (introducing cycles into the state space). However, since we no longer have turn costs, the state space no longer needs to include the predecessor node.

The state is now defined as a tuple $(S, v)$, where $S$ is a bitmask of length $n$ (the number of clusters) representing the set of clusters that have already been satisfied (i.e., an internal edge within them has been traversed), and $v$ represents the current vertex in the graph $G=(V, E)$. Thus, we can employ Dijkstra’s Algorithm over this state space to guarantee finding the global minimum.

Let $D(S, v)$ denote the minimum cost to reach vertex $v$ having satisfied the set of clusters $S$. The value of $D(S, v)$ is determined by the optimal transition from a previous state $(S', u)$ via an edge $(u, v) \in E$.

The transitions depend on whether the traversed edge is internal or external:

1. If $(u,v)$ is an internal edge of cluster $C_i$​, then $C_i$​ becomes satisfied and is added to the current set of clusters.
    
2. If $(u,v)$ is an external edge, the set of satisfied clusters remains unchanged.

This logic is summarized by the recurrence:

$$D(S, v) = \min_{(u, v) \in E} \left\{ D(S', u) + w(u, v) \right\}$$

Where the predecessor state $S'$ is defined as:

$$S' = \begin{cases} S \setminus \{C_i\} & \text{if } (u, v) \text{ is the internal edge of cluster } C_i \text{ that completed } S \\ S & \text{otherwise (external edges or redundant internal traversals)} \end{cases}$$

The algorithm also initializes $D(\emptyset, v) = 0$ for all $v$ and sets all other distances to infinity. Using a priority queue, it relaxes edges according to the recurrence above.

The final solution is the minimum value among all states where the cluster set is fully satisfied:

$$\text{Optimal Cost} = \min_{v \in V} D(\{C_1, \dots, C_k\}, v)$$

For the sake of completeness, the detailed pseudocode is provided in Appendix A.1 at the end of the document.

#### Proof of Correctness

The correctness of this approach rests on the reduction of the original problem to a Shortest Path problem on a weighted directed graph. Let $G$ be a graph where every vertex is a valid state $(S, v)$ and edges represent valid physical moves in the original graph.

The transition logic in $G$ is as follows: an edge connects state $(S, u)$ to state $(S', v)$ if $(u, v)$ is an edge in the original graph. The weight of this transition corresponds exactly to the physical cost of traversing edge $(u, v)$. The new mask $S'$ is defined as $S \cup \{k\}$ if $(u, v)$ is an internal edge of cluster $C_k$, and simply $S$ otherwise (if the edge is external).

Since all edge costs are given as positive numbers, there are no negative cycles in this state graph $G$. Dijkstra’s algorithm is proven to find the shortest path from a source set to a destination in any graph with non-negative edge weights. The relaxation property ensures that for every state extracted from the priority queue, the cost is the global minimum to reach that specific configuration of satisfied clusters and current location.

The algorithm terminates the first time it extracts a state where the mask $S$ corresponds to the set of all clusters $\{C_1, \dots, C_n\}$. Because Dijkstra extracts states in strictly non-decreasing order of total cost, the first time this condition is met represents the cheapest possible cost to satisfy the requirement for every cluster. The ability to revisit nodes and traverse edges multiple times (where the mask $S$ might not change) is handled correctly because such transitions are simply edges in $G$; infinite cycles of re-visitation are avoided because they would strictly increase the cost without adding new satisfied clusters to the mask, meaning they would never be prioritized over the simpler path.

#### Time Complexity

The time complexity is determined by the number of states and transitions in the expanded graph. The number of states is $|V| \cdot 2^n$, where $|V|$ is the number of vertices and $n$ is the number of clusters. Each state has an outgoing degree equal to the degree of the corresponding node in the original graph. Therefore, the total number of edges in the state graph is $|E| \cdot 2^n$. Using Dijkstra’s algorithm with a binary heap, the complexity is $O(|E| \cdot \log(|V|))$. Substituting our state graph parameters, the running time is $O(|E| \cdot 2^n \cdot \log(|V| \cdot 2^n))$. This confirms that the algorithm is efficient only for a small number of clusters, consistent with the problem's NP-Hard nature.

### Proposed Heuristic Strategies

We propose three distinct heuristic strategies to approximate the solution to this problem. The first two approaches are grounded in Frederickson’s algorithm[^7], an approximation algorithm that guarantees a 1.5-approximation for the Rural Postman Problem[^4], a problem that seeks a minimum-cost tour covering a specified subset of edges in a graph. However, they differ in the heuristic criteria used to select the single representative internal edge for each cluster, which Frederickson’s algorithm then connects to construct a complete solution.
Finally, the third approach leverages a TSP solver, specifically the Lin–Kernighan–Helsgaun (LKH)[^8] algorithm, which is widely regarded as one of the most effective state-of-the-art heuristics for solving the Traveling Salesman Problem.

> [!question] Why constant-factor approximation algorithms are not applicable
> It is crucial to clarify why the approaches proposed in the following sections are classified as heuristics rather than approximation algorithms with a guaranteed global performance bound. The primary obstacle to finding a strict approximation ratio lies in the structure of the generalized problem itself.
> 
> Firstly, approaches that separate the Selection and Routing phases cannot theoretically guarantee a constant approximation ratio. As we will demonstrate through worst-case scenarios, a locally optimal selection (e.g., picking the cheapest internal edge) can impose an arbitrarily large penalty on the global connection cost, leading to an unbounded worst-case error.
> 
> Secondly, transformation-based approaches, although effective in practice, do not directly yield approximation guarantees. Reductions such as the Noon–Bean[^9] transformation allow the problem to be expressed as a single TSP instance, enabling the use of powerful local-search solvers. However, these transformations typically produce asymmetric instances and may violate the triangle inequality. As a result, classical approximation algorithms for metric TSP (e.g., Christofides’[^1] algorithm) are no longer applicable.
> 
> However, a weak approximation guarantee can still be established. We show in the following section that a simple heuristic selecting the cheapest internal edge per cluster yields a solution whose cost is at most $O(n)$ times the global optimum, where $n$ is the number of clusters.

#### Proof of Weak Approximation Guarantee

Let's consider the following simple constructive algorithm:
  
1. Select an arbitrary cluster as a root, say $C_j$, and choose a vertex $r \in C_j$.
    
2. For every other cluster $C_i$ with $i = 1,\dots ,n$ and $i \neq j$:
    
    - Choose a minimum-cost internal edge $e_i = u_i v_i$​. Let $d_i = \text{cost}(e_i)$.
        
    - Compute $\min\{\operatorname{dist}(r,u_i), \operatorname{dist}(r,v_i)\}$.
        
    - Move from $r$ to the closer endpoint using the shortest path.
    - Traverse the internal edge $e_i$.
    - Return to $r$ along the same external path.
        
3. Concatenate all these walks (and also traverse the internal edge chosen for $C_j$) to obtain the final walk $P$.

The resulting walk $P$ is feasible; it contains at least one internal edge from every cluster. Repeated visits of nodes and edges are allowed, hence the construction is valid.

Let $C^*$ be the cost of an optimal walk satisfying the requirement.
Every feasible solution must traverse at least one internal edge in each cluster. Therefore $C^* \ge \sum_{i=1}^{n} d_i$
Now, let $H$ be the subgraph consisting of all edges that are used at least once in $P^*$.  
Since $P^*$ is connected, $H$ is connected. Let $T \subseteq H$ be any spanning tree of $H$.  
Then its weight satisfies $w(T) \le C^*$.

In our construction, for each cluster $C_i$ with $i\neq j$ the path between $r$ and $C_i$ is traversed twice (go and return). This is analogous to performing a DFS of a tree connecting all clusters, where each tree edge is used exactly twice.
Thus, the total cost of all edges used by the heuristic is at most $2(n-1) \cdot w(T) + \sum_{i=1}^{n} d_i$, because in the worst case each of the $n-1$ tree edges may appear in the round trips to the clusters.
The total cost of the heuristic path P is therefore $C \le 2(n-1) \cdot w(T) + \sum_{i=1}^{n} d_i$.
Using the previous bounds in this last inequality we obtain:

$$C  
\le 2(n-1) \cdot C^* + C^*  
= O(n) \cdot C^*$$

It's important to note that this bound does not provide any information on the actual cost of our solution in practice: the algorithm could traverse edges with very high individual costs. Nevertheless, the algorithm produces a feasible path whose cost is at most a linear factor in the number of clusters times the optimum.

In the next section, we put the idea of selecting the minimum-cost internal edge for each cluster into practice in our first heuristic, and describe in detail how to construct a path that connects these chosen edges.

#### Heuristic Strategy \#1

To address the computational intractability of finding an exact solution, we propose a polynomial-time heuristic based on a decomposition of the problem into three distinct phases: Greedy Selection, Metric Closure, and the application of Frederickson’s Heuristic.

##### Algorithm Description

In the first phase, we deterministically select a single "representative" internal edge for each cluster. To minimize the immediate cost, we choose the internal edge $e_i$ with the minimum weight within every cluster $C_i$ (here's the greedy choice). Let $E_R$ be this set of required edges. The problem is thus reduced to finding a minimum-cost tour that traverses every edge in $E_R$.

Before proceeding, it is important to highlight that Frederickson’s 1.5-approximation guarantee strictly holds only for metric graphs. While the original physical graph may not satisfy this property, the problem statement explicitly permits revisiting nodes and traversing edges multiple times. This allows us to work on the Metric Closure of the graph, obtained by computing the shortest-path distances between all pairs of nodes (e.g., using the Floyd-Warshall algorithm), thereby artificially satisfying the metric requirement and validating the use of the heuristic.

Frederickson’s Heuristic is then applied to connect the selected edges $E_R$, producing the approximate solution. For the sake of completeness and to clarify the algorithmic flow, we summarize the three key steps of Frederickson’s procedure below:

1. We first aim to connect the disjoint edges in $E_R$ into a single connected structure with minimal cost. To do this, we construct a complete meta-graph with $K$ vertices, where each vertex represents one of the selected edges in $E_R$. The weight of an edge connecting two meta-vertices, $u$ and $v$, is defined as the shortest path distance in the original graph between the physical components they represent (specifically, the minimum distance between any endpoint of edge $u$ and any endpoint of edge $v$). We then compute the Minimum Spanning Tree of this meta-graph. The edges of the MST represent the shortest physical paths required to bridge the gaps between the service edges.
2. A connected graph allows for an Eulerian Tour (a closed loop traversing every edge) if and only if every vertex has an even degree. We superimpose the edges from $E_R$ and the paths identified by the MST. In this intermediate structure, we identify the set of physical vertices $V_{odd}$ that have an odd degree. By the Handshaking Lemma[^10], $|V_{odd}|$ is guaranteed to be even. To correct the parity, we compute a Minimum Weight Perfect Matching[^11] on $V_{odd}$ using the metric distances. The edges selected by the matching correspond to additional shortest paths that must be traversed (or duplicated) to make every node's degree even.  
3. Finally, we combine the edge sets from the three phases: the required edges, the connection paths, and the parity correction paths. The union of these sets forms a connected Eulerian Multigraph, where every node has an even degree. We can then apply Hierholzer’s algorithm[^12] (or a simple circuit stitching procedure) to trace a continuous cycle that visits all required edges and returns to the start, constituting our final solution.

The pseudocode for the first heuristic is provided in Appendix A.2. Note that the input format differs slightly from that of the exact solution algorithm described previously. Here, the vertices $V$ and edges $E$ are encapsulated within a graph object $G$, and edges are explicitly grouped by cluster objects. This structural change is adopted to facilitate the edge-based selection phase, without altering the underlying problem definition.

A natural question arises regarding the topology of the solution: why does the algorithm produce a closed tour rather than an open path?

The reason lies in the definition of the heuristic itself. Frederickson’s algorithm is designed to solve the Rural Postman Problem, which by definition seeks a tour that traverses all required edges and returns to the origin. While a tour is theoretically a valid solution for our problem, it is suboptimal if the problem statement permits an open route. Forcing a return to the starting point inevitably incurs unnecessary travel costs. However, we can adapt the heuristic to produce a generic open path using a standard graph theory transformation before the Parity Correction phase, the "Dummy Nodes" technique[^13].

To allow the path to start and end at optimal, distinct locations, we can modify the Minimum Weight Perfect Matching step as follows:

1. Instead of computing the matching solely on the set of odd-degree vertices $O$, we introduce two additional auxiliary "dummy" nodes, $d_1$ and $d_2$, to the set.
    
2. We define the matching cost between these dummy nodes and every original odd-degree node $v \in O$ as 0. (The cost between the two dummy nodes themselves is set to $\infty$ to prevent them from matching with each other).
    
3. When the Minimum Weight Perfect Matching is computed on the set $O \cup \{d_1, d_2\}$, it will naturally pair two physical nodes from $O$ with the two dummy nodes (since the cost is 0, which is the cheapest possible option).
    
4. The topological consequence is the following:
    
    - The two physical nodes matched with the dummies will remain with an odd degree in the final multigraph (since the "matching edge" is dummy and doesn't exist in the physical graph).
        
    - All other nodes in $O$ will be matched with each other via real paths, becoming even-degree nodes.

According to Euler’s Graph Theorem, a connected graph with exactly two odd-degree vertices admits an Eulerian Path that starts at one odd node and ends at the other. This modification allows the algorithm to determine the optimal start and end points of the route automatically, effectively transforming the closed TSP tour into an open path and eliminating the cost associated with returning to the starting node.

For completeness, the Frederickson algorithm, modified using the dummy technique, is provided in Appendix A.3.

##### Time Complexity

We analyze the computational complexity of the proposed heuristic with respect to the number of vertices $|V|$, edges $|E|$ in the original graph, and the number of clusters $K$ (which corresponds to the number of required edges $|E_R|$).

Let's analyze the three distinct phases:

1. Greedy Selection
	- This phase involves iterating through all edges in the graph to find the minimum weight internal edge for each cluster. Since every edge is visited exactly once, this step is linear with respect to the graph size.
	- Complexity: $O(|E|)$.

2. Metric Closure
	- To enable the routing heuristic, we must compute the All-Pairs Shortest Paths.
	- Using Floyd-Warshall, the complexity is $O(|V|^3)$.  
	- Alternatively, using Repeated Dijkstra (with a Fibonacci heap) for each of the $|V|$ nodes, the complexity is $O(|V| \cdot (|E| + |V|\log|V|))$. For dense graphs, this step is dominated by $O(|V|^3)$.
3. Frederickson’s Routing
	- This phase is also divided in three additional phases:
	1. We compute weights between all pairs of the $K$ selected edges. This requires $O(K^2)$ lookups in the pre-computed distance matrix.
	2. Constructing the MST on a complete graph with $K$ nodes takes $O(K^2)$ using Prim’s algorithm.
	3. Computing Minimum Weight Perfect Matching is the most complex part of the routing phase. The Blossom algorithm[^14] generally runs in $O(K^3)$ time.
	4. Traversing the final multigraph is linear in the size of the solution tour, roughly $O(K)$.

The total time complexity is the sum of the components:

$$O(|E|) + O(|V|^3) + O(K^3)$$

Since $K \le |V|$, the complexity is bounded by $O(|V|^3)$.

##### How Well Does It Perform?

While this heuristic offers a polynomial-time solution and guarantees a 1.5-approximation for the routing phase (thanks to Frederickson’s algorithm operating on a metric closure), we emphasize again that the overall strategy does not constitute an approximation algorithm with a constant guarantee.
By selecting the internal edge with the absolute minimum weight locally, the algorithm ignores the global topology and the cost of connecting these edges.

We demonstrate this limitation using the specific linear graph instance provided below.

![Pasted image 20260108194420.png](/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/_images/Pasted%20image%2020260108194420.png)
<center><i>Figure 1: An adversarial graph instance with 8 nodes, 7 edges, and 2 clusters.</i></center>

In this example our heuristic will strictly select edges $(A, B)$ and $(G, H)$ because they represent the cheapest option within their respective clusters. This decision forces the routing algorithm to traverse the high-cost "trap" edges to connect the two extremities, resulting in a path $A \to H$ with a total cost of $2016$.

Conversely, the global optimum would select edges $(C, D)$ and $(E, F)$. Although these edges are locally more expensive, they are adjacent to the central bridge, allowing for a path $D \to E$ with a total cost of only $14$.
While this is an extreme worst-case scenario and we expect better average performance on well-connected graphs, it proves that the error ratio is unbounded.

This limitation motivates our second heuristic strategy, which attempts to mitigate this issue by incorporating the distance to the nearest cluster into the selection logic.

#### Heuristic Strategy \#2

To address the limitation of the previous strategy, we propose a second heuristic that incorporates a "penalty" term. Instead of selecting edges solely based on their weight $w(u,v)$, we evaluate a hybrid cost that balances the internal weight with the proximity to the rest of the network (specifically, to the nearest clusters).

We define the selection metric $NewCost$ for an internal edge $e=(u,v)$ as follows:

$$NewCost(e) = w(u,v) + \alpha \cdot \left( \min_{k \neq i} dist(u, C_k) + \min_{k \neq i} dist(v, C_k) \right)$$

Where:

- $w(u,v)$ is the physical weight of the edge.
    
- $dist(x, C_k)$ is the shortest path distance from node $x$ to the nearest node belonging to cluster $C_k$.
    
- $\alpha$ (alpha) is a tunable parameter ($\alpha \ge 0$) that weights the importance of connectivity versus edge cost.

##### Time Complexity

Unlike strategy \#1, which could perform the greedy selection before calculating global distances, this strategy requires knowledge of the graph's geometry during the selection phase. Therefore, the All-Pairs Shortest Path computation is performed first, without affecting the time complexity.
Let's instead analyze the complexity of calculating the hybrid cost for all edges:

1. We iterate through all edges $e=(u, v)$ in the graph: $|E|$ iterations.
    
2. For each endpoint, we search for the distance to the nearest cluster. In the worst-case implementation, this involves checking distances to all other nodes or clusters: $O(|V|)$ or $O(K)$.
    
3. The selection complexity is thus $O(|E| \cdot |V|)$.

Since the number of edges $|E|$ is at most $|V|^2$ (in a complete graph), it holds that:

$$O(|E| \cdot |V|) \subseteq O(|V|^2 \cdot |V|) = O(|V|^3)$$

Thus, the enhanced intelligence of the heuristic comes at no extra asymptotic cost, as the procedure is still bounded by the initial All-Pairs Shortest Path calculation.

The pseudocode for the modified selection phase is provided in Appendix A.4.

##### How Well Does it Perform?

It is immediately evident that this connectivity-aware heuristic successfully resolves the adversarial "trap" instance that caused the failure of the previous strategy.

By applying the hybrid formula (with $\alpha=1$), the edge costs are recalculated as shown below:

![Pasted image 20260109143558.png](/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/_images/Pasted%20image%2020260109143558.png)
<center><i>Figure 2: An adversarial graph instance with 8 nodes, 7 edges, and 2 clusters, with recalculated costs</i></center>

Based on these new values, the algorithm minimizes the hybrid score and selects the edges with a weight of 24 in both clusters. This decision aligns with the global optimum, avoiding the high-cost edges.

However, this heuristic does not guarantee optimality in all scenarios. A notable limitation arises in symmetric topologies where multiple candidate edges possess identical physical weights and are equidistant from other clusters. In such cases, the algorithm is forced to make an arbitrary selection based on internal indexing. Since the heuristic cannot foresee the full global tour construction, this arbitrary choice may prevent the formation of the optimal circuit.

This limitation suggests that relying on a single "one-shot" constructive heuristic is insufficient for complex instances. Instead of attempting to engineer a perfect selection formula, we propose a Local Search strategy. This approach accepts an initial solution and iteratively refines it to converge towards a high-quality local optimum.

#### Heuristic Strategy \#3

The fundamental limitation of the previous heuristics stems from the separation of the problem in two: first select edges and then find a route between them. This sequential approach invariably leads to suboptimal decisions.

To overcome this, we propose a different approach: reducing our problem to a Generalized Traveling Salesman Problem and solving it using a TSP solver. Unlike constructive heuristics that build a solution step-by-step, state-of-the-art TSP solvers (such as LKH-3) rely on Local Search. Typically, they start with a rough, non-optimal initial solution and iteratively refine it by swapping edges to converge towards a local optimum. If the improvement stalls, they employ perturbation mechanisms to escape local minima and explore different areas of the search space.

Thus, the main difficulty lies in mapping our problem, where we select only one edge per cluster and allow physical re-visits, to a standard TSP, which requires visiting every node exactly once. To achieve this, we modify the structure of the graph so that the TSP solver is not forced to visit all nodes, but can traverse some of them at zero cost. In particular, this requires transforming the problem into an asymmetric TSP instance.

> [!info] Note
> This reduction should be regarded as a heuristic approach inspired by the structure of the problem and by classical TSP-based techniques described in the literature. While the reduction itself is not original and does not guarantee optimality, it enables the use of highly optimized state-of-the-art solvers such as LKH-3. Leveraging decades of research on TSP heuristics, this approach provides an effective practical strategy despite the lack of formal approximation guarantees.

##### Algorithm Description

We now describe the algorithm, which consists of three conceptual steps:

1. As in the previous strategies, we compute the shortest path distances between all relevant pairs of physical nodes in the graph.
    
2. We construct a virtual graph $G'$ where nodes represent undirected edges.
   In particular, for every undirected candidate edge $e=(u, v)$ in each cluster, we create two virtual nodes:
	- $e_{uv}$ representing traversing the edge from $u \to v$.
	- $e_{vu}$ representing traversing the edge from $v \to u$.
  
   We now define the cost of moving from one virtual node to another virtual node.
   Let node $i$ be the traversal of edge $(u,v)$ and node $j$ be the traversal of edge $(a,b)$. The cost $c(i, j)$ in the matrix is the physical shortest path from the end of the first edge $v$ to the start of the second edge $a$, plus the traversal cost of the second edge $w_{ab}$: $cost(i, j) = ShortestPath(v, a) + w(a, b)$
    
1. As discussed above, standard TSP solvers require visiting every node exactly once, whereas our objective is to traverse at least one internal edge per cluster. To enforce this constraint, we apply a topological transformation that introduces a zero-cost cycle among all virtual nodes belonging to the same cluster. This technique, originally proposed by Noon and Bean in 1993, forces the solver to visit all virtual nodes within a cluster at zero cost once it has entered that cluster, while charging the full cost only for the transition from a different cluster. As a result, the solver effectively selects exactly one internal edge per cluster, while operating on a standard asymmetric TSP instance.

After this transformation, the resulting cost matrix is passed to a TSP solver that supports asymmetric instances (such as LKH), which computes a minimum-cost tour over the transformed graph.

Again, the solution returned by the TSP solver is a cycle, since the TSP is inherently a tour problem. To resolve this, we can apply the dummy node technique (similarly to the approach used in the previous heuristics):

1. We add a virtual node $z$ into the solver matrix.
    
2. Connections to/from $z$ have a cost of 0.
    
3. Since the TSP constraint enforces exactly one visit per node, the position of $z$ in the optimal tour effectively determines the cut between the start and end of the path, allowing the solver to return an optimal open path without incurring additional cost.

We now proceed to analyze several characteristic scenarios of the problem.

### What if internal edges are much more expensive than external edges?

When internal edge costs are significantly larger than external costs, the total objective function becomes dominated by the cost of internal edges. In shorts, $cost_{total} \approx cost_{internal}$.

Mathematically, this acts as a smoothing factor for the approximation ratio. Any suboptimal decision made regarding the routing part has a diminishing impact on the percentage error of the total solution.

Let $C^*$ be the cost of the optimal solution and $C$ be the cost of the heuristic solution, the approximation ratio is $\rho = \frac{C}{C^*}$.

Let's decompose the costs into routing $R$ and service $S$:

$$\rho = \frac{R + S}{R^* + S^*}$$

If we assume that $S$ is very large (a large constant $K$) compared to $R$, and that our heuristic selects similarly priced edges as the optimal solution:

$$\lim_{K \to \infty} \frac{R + K}{R^* + K} = 1$$

Thus, the larger the internal costs, the closer the ratio $\rho$ drives towards 1. The heavy service costs "drown out" the relative inefficiency of the routing.

However, this improvement holds only if the heuristic selects the correct set of edges.

If the heuristic (specifically the heuristic 1) blindly chases a slightly cheaper internal edge but incurs a massive travel penalty, the ratio degrades.

We can quantify the worst-case bound for the greedy heuristic in this scenario as being bounded by the ratio of service costs:

$$\rho_{greedy} \approx \frac{\max(cost_{internal})}{\min(cost_{internal})}$$

If the variance in internal costs is low, the ratio is excellent. If the variance is high, the greedy heuristic can perform poorly.

This analysis naturally leads to another interesting scenario, that is what happens when all internal edges within the same cluster have similar costs. We analyze this scenario next.

### What if internal edges within the same cluster have similar costs?

We now consider the case where internal edges within the same cluster have similar costs. More formally, for each cluster $Cl_i$, let $\Delta_i$ be the difference between the most expensive and the cheapest internal edge:

$$\max_{e \in Cl_i} w(e) - \min_{e \in Cl_i} w(e) = \Delta_i$$

where $\Delta_i$ is small relative to the total cost. When internal edge costs within a cluster exhibit low variance, the specific choice of which internal edge to serve becomes largely interchangeable from a strictly cost perspective. Consequently, the approximation error introduced by selecting a non-optimal internal edge is bounded by a small additive term.

Let $C^*$ denote the total cost of the optimal solution and $C$ denote the total cost of the heuristic solution. We decompose the costs into the service component $C_{serv}$ and the travel component $C_{trav}$.

If the heuristic selects an internal edge whose cost differs from the optimal one by at most $\Delta_i$ per cluster, the service cost satisfies:

$$C_{serv} \le C^*_{serv} + \sum_{i=1}^n \Delta_i$$

Therefore, the total cost $C$ satisfies:

$$C = C_{serv} + C_{trav} \le C^*_{serv} + \sum_{i=1}^n \Delta_i + C_{trav}$$

Consequently, the global approximation ratio can be expressed as:

$$\frac{C}{C^*} \le 1 + \underbrace{\frac{\sum_{i=1}^n \Delta_i}{C^*}}_{\text{Selection Error}} + \underbrace{\frac{C_{trav} - C^*_{trav}}{C^*}}_{\text{Routing Error}}$$

It is evident that the error introduced by the selection process is additive and depends on the cumulative variance of the clusters ($\sum \Delta_i$). In this scenario, where internal edges have similar costs, this term tends to zero relative to the total cost.

Thus, the approximation ratio becomes dominated by the second term. This confirms that when internal costs are similar, the problem effectively reduces to optimizing the Routing Error, which depends exclusively on the geometric configuration of the entry and exit points. Unfortunately, this does not trivially simplify the problem. As the number of external edges increases, finding the optimal path through the specific entry/exit nodes remains computationally challenging.

One might consider adopting simple heuristics for external edge selection, such as deterministically choosing the external edge with the minimum weight or approximating a cluster as a single centroid for a TSP solver. However, these approaches often fail because they ignore the internal topology of the cluster.

![Pasted image 20260110181315.png|600](/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/_images/Pasted%20image%2020260110181315.png)
<i>Figure 4: An adversarial graph instance with 3 clusters and 3 external edges</i>

Consider the scenario depicted in Figure 4 with three clusters $C_1, C_2, C_3$. There are two external edges connecting $C_1$ to $C_2$: a "cheap" edge with cost 1 and an "expensive" edge with cost 10. A greedy approach might deterministically select the edge with cost 1 to minimize the immediate travel cost. However, this decision ignores the subsequent requirement to reach $C_3$. If the cheap edge leads to an entry point in $C_2$ that is far from the connection to $C_3$, the path is forced to traverse the entire length of the cluster to reach the exit point. This internal traversal could incur a travel cost significantly higher than the initial savings. Conversely, choosing the "expensive" edge might lead to a node directly adjacent to the exit for $C_3$, thereby eliminating the need for internal traversal and resulting in a lower global cost.

### Conclusion

In conclusion, we demonstrated the NP-hardness of the problem and the impossibility of achieving a constant-factor approximation in the general case. To address this complexity, we explored three distinct strategies:

1. A greedy heuristic based on minimum cost edge selection, which offers a polynomial-time solution but suffers from topological myopia;
    
2. A parametric heuristic designed to mitigate this myopia by weighting the selection logic with the distance relative to neighboring clusters;
    
3. A local search based heuristic, which reduces the problem to a Generalized TSP solved via a TSP solver based on local search.

Furthermore, our analysis reveals that no single heuristic is universally optimal; the effectiveness of each approach is strictly dependent on the specific topology and cost structure of the instance. Although the first two heuristics guarantee polynomial time complexity, the extensive research in TSP optimization allows the third strategy to achieve superior quality results in modest computational times.

## Appendix A - Pseudocode

### A.1 Exact Solution

```python
# cluster_map maps every node u to its cluster index (0 to n-1)
# n is the number for clusters
function OptimalSolution(V, E, cluster_map, n):
    Dist = HashMap() # Where key = (mask, current_node) and value = cost
    PQ = PriorityQueue()# We will push (cost, mask, current_node)

    # The path can start at any node in the graph.
    # At the start, cost is 0 and no clusters are satisfied (mask = 0).
    for v in V:
        Dist[(0,v)] = 0
        PQ.push(0, 0, v) # (cost, mask, node)

    target_mask = (1 << n) - 1
    min_total_cost = Infinity

    while PQ is not empty:
        current_cost, mask, u = PQ.pop()

        # Lazy deletion for outdated entries
        if current_cost > Dist.get((mask, u), Infinity):
            continue

        # Termination condition: All clusters visited
        if mask == target_mask:
            return current_cost

        # Transitions: Explore all adjacent edges (u, v)
        for each neighbor v of u:
            edge_cost = cost(u, v)
            new_cost = current_cost + edge_cost
            
            # Determine if this edge satisfies a new cluster
            u_cluster = cluster_map[u]
            v_cluster = cluster_map[v]
            
            # If nodes are in the same cluster, update the mask
            if u_cluster == v_cluster:
                # Add the cluster index to the set of satisfied clusters
                new_mask = mask | (1 << u_cluster)
            else:
	            new_mask = mask
            
            # Relaxation step
            new_state = (new_mask, v)
            if new_cost < Dist.get(new_state, Infinity):
                Dist[new_state] = new_cost
                PQ.push(new_cost, new_mask, v)

    return Infinity # Should not be reached if graph is connected
```

### A.2 Heuristic Strategy \#1

```python
function HeuristicStrategy1(G, Clusters):
    # 1. Greedy Selection
    # Select the cheapest internal edge for each cluster
    E_R = Set() 
    
    for each cluster C in Clusters:
        min_weight = Infinity
        best_edge = NULL
        
        # Iterate to find the minimum weight edge in the current cluster
        for each edge e in C:
            if weight(e) < min_weight:
                min_weight = weight(e)
                best_edge = e
        
        E_R.add(best_edge)

    # 2. Metric Closure
    # Compute All-Pairs Shortest Paths to allow virtual travel between any nodes.
    # D[u][v] stores the shortest distance between physical nodes u and v.
    D = FloydWarshall(G) 

    # 3. Routing (Frederickson)
    final_tour = FredericksonRouting(E_R, D)
    
    return final_tour


function FredericksonRouting(E_R, D):
    # 1. MST on Components
    # Create a complete graph where each node represents a chosen edge from E_R
    MetaGraph = Graph()
    
    for each edge e_i in E_R:
        MetaGraph.addNode(e_i)

    # Calculate weights between "edge-nodes" based on physical shortest paths
    for each pair (e_i, e_j) in E_R:
        # dist(u, v) is retrieved from D
        u1, u2 = endpoints(e_i)
        v1, v2 = endpoints(e_j)
        
        # Find the minimum distance to connect edge i to edge j
        w = min(D[u1][v1], D[u1][v2], D[u2][v1], D[u2][v2])
        MetaGraph.addEdge(e_i, e_j, w)

    # Compute MST on this abstract graph to find connection paths
    T_abstract = MinimumSpanningTree(MetaGraph)

    # 2. Parity Correction (Matching)
    # We now look at the physical graph formed by required edges and connection paths
    # Note: T_physical represents the actual paths corresponding to edges in T_abstract
    IntermediateGraph = Union(E_R, T_physical)
    
    OddNodes = List()
    for each node v in IntermediateGraph:
        if degree(v) is odd:
            OddNodes.add(v)

    # Find minimum weight matching to pair up odd nodes using shortest paths
    M = MinimumWeightPerfectMatching(OddNodes, D)

    # 3. Eulerian Tour Generation
    # Combine all sets of edges into a multigraph
    # H allows multiple edges between nodes (revisiting edges)
    H = MultiGraph()
    H.addEdges(E_R)        # Service edges
    H.addPaths(T_abstract) # Connection paths from MST
    H.addPaths(M)          # Correction paths from Matching

    # Since H is connected and all degrees are even, an Eulerian tour exists
    tour = FindEulerianTour(H)

    return tour
```

### A.3 Dummy Node Technique in Frederickson's Algorithm

```python
function FredericksonPath(E_R, D):
    # 1. MST on Components
    MetaGraph = Graph()
    for each edge e_i in E_R:
        MetaGraph.addNode(e_i)

    for each pair (e_i, e_j) in E_R:
        u1, u2 = endpoints(e_i)
        v1, v2 = endpoints(e_j)
        w = min(D[u1][v1], D[u1][v2], D[u2][v1], D[u2][v2])
        MetaGraph.addEdge(e_i, e_j, weight=w)

    T_abstract = MinimumSpanningTree(MetaGraph)

    # 2. Parity Correction (Matching)
    # We look at the physical graph formed by required edges and connection paths
    IntermediateGraph = Union(E_R, T_physical)
    
    OddNodes = List()
    for each node v in IntermediateGraph:
        if degree(v) is odd:
            OddNodes.add(v)
    # Introduce 2 Dummy Nodes to absorb two odd parities
    d1 = Node()
    d2 = Node()
    
    NewSet = OddNodes + [d1, d2]
    
    # Define special weights for the matching
    NewWeights = Copy(D) 
    for each node v in OddNodes:
        NewWeights[v][d1] = 0
        NewWeights[v][d2] = 0
    NewWeights[d1][d2] = Infinity

    # Compute Matching on the augmented set
    M_aug = MinimumWeightPerfectMatching(NewSet, NewWeights)

    # Keep only edges that connect two real nodes.
    # The edges (v, d1) and (u, d2) are discarded. 
    # This leaves nodes v and u with odd degrees -> They become Start and End.
    M_real = List()
    for each edge (u, v) in M_aug:
        if u is not dummy AND v is not dummy:
            M_real.add((u, v))

    H = MultiGraph()
    H.addEdges(E_R)        
    H.addPaths(T_abstract) 
    H.addPaths(M_real)     # Add only the real matching paths

    # H now has exactly 2 nodes with odd degree.
    # Find the path starting at one odd node and ending at the other.
    path = FindEulerianPath(H)

    return path
```

### A.4 Heuristic Strategy \#2

```python
function HeuristicStrategy2(G, D, Clusters, alpha):
    # D is the pre-computed Distance Matrix (Floyd-Warshall)
    
    E_R = Set() 

    for each cluster C_i in Clusters:
        best_edge = NULL
        min_score = Infinity

        for each edge e=(u, v) in C_i:
            w = weight(e)

            # Find min distance from u (and v) to any node in any other cluster C_k
            dist_u = Infinity
            dist_v = Infinity
            
            for each cluster C_k in Clusters (where k != i):
                for each node n in C_k:
                    dist_u = Min(dist_u, D[u][n])
                    dist_v = Min(dist_v, D[v][n])

            score = w + (alpha * (dist_u + dist_v))

            if score < min_score:
                min_score = score
                best_edge = e
        
        E_R.add(best_edge)

    return E_R
```

## Bibliography

[^1]: https://en.wikipedia.org/wiki/Christofides_algorithm
[^4]: https://www.sciencedirect.com/science/article/abs/pii/030505489400070O
[^7]: https://dl.acm.org/doi/10.1145/322139.322150
[^8]: https://en.wikipedia.org/wiki/Lin%E2%80%93Kernighan_heuristic
[^9]: https://en.wikipedia.org/wiki/Travelling_salesman_problem#:~:text=Noon%20and%20Bean%20demonstrated%20that%20the%20generalized%20travelling%20salesman%20problem%20can%20be%20transformed%20into%20a%20standard%20TSP%20with%20the%20same%20number%20of%20cities%2C%20but%20a%20modified%20distance%20matrix.
[^10]: https://en.wikipedia.org/wiki/Handshaking_lemma
[^11]: https://www.math.uwaterloo.ca/~bico/papers/match_ijoc.pdf
[^12]: https://en.wikipedia.org/wiki/Eulerian_path#Hierholzer's_algorithm
[^13]: Gutin, G., & Punnen, A. P. (Eds.). (2002). _The Traveling Salesman Problem and Its Variations_. Springer.
[^14]: https://en.wikipedia.org/wiki/Blossom_algorithm
[^15]: https://ieeexplore.ieee.org/document/8949664