---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/advanced-algorithms/advanced-algorithms-assignment-2/","created":"2025-11-18T17:04:36.518+01:00","updated":"2026-07-09T15:08:03.543+02:00","dg-note-properties":{}}
---

# Advanced Algorithms Assignment 2

Here's the assignment description.

<iframe src="/img/user/%F0%9F%8E%93%20University%20notes%20(mostly%20in%20Italian)/%F0%9F%9A%A8%20Advanced%20Algorithms/advalg-ass2-2025.pdf" width="100%" height="900px" title="advalg-ass2-2025.pdf" style="border:1px solid #ccc;"></iframe>

## Exercise 3

First, let's recall the formal definition of the value of a flow $f$. The value $val(f)$ is the net flow leaving the source:

$$val(f) = f^+(s) - f^-(s) = \sum_{u \in V} f(s, u) - \sum_{v \in V} f(v, s) $$
where
- $\sum f(s, u)$ represents the total flow leaving the source ($f^+(s)$) and $u$ is any node to which an edge exists from $s$.
- $\sum f(v, s)$ represents the total flow entering the source ($f^-(s)$), where $v$ is any node from which an edge exists to $s$.

The strategy is to iteratively modify $f$ to reduce the incoming flows to zero while maintaining flow conservation at all other nodes.
Algorithm:
We perform the following procedure as long as there exists an edge $e = (v, s)$ entering the source with positive flow $f(e) > 0$:
- We start at node $v$ and trace the flow backwards. Since flow conservation holds at $v$, the positive flow leaving $v$ towards $s$ must have arrived at $v$ from some other node. We can recursively find an incoming edge with positive flow at the previous node. We repeat this backward tracing until we encounter one of two termination cases:
	- Case 1: We reach the source $s$, meaning we have found a cycle of positive flow that starts and ends at $s$: something like $s \to \dots \to v \to s$.
	- Case 2: We reach the sink $t$. This means we have found a path of positive flow from $t$ to $s$: like $t \to \dots \to v \to s$. Since the graph is finite, we cannot trace backwards indefinitely without forming a cycle or hitting a source/sink node.
- Let $seq$ be the sequence of edges found (either the cycle or the path). Let $\delta = \min_{e \in seq} f(e)$ be the bottleneck flow on this sequence. Since all $f(e)$ are integers, $\delta \ge 1$. We can define a new flow $f'$ by subtracting $\delta$ from the flow of every edge in $P$:

$$f'(e) = f(e) - \delta \quad \forall e \in seq$$
$$f'(e) = f(e) \quad \forall e \notin seq$$

Since we subtracted the same amount $\delta$ from the flow entering and leaving every intermediate node in $seq$, the flow conservation is preserved at all nodes $u \neq s, t$. In addition, since we only reduced flow, $0 \le f'(e) < f(e) \le c(e)$. The capacity constraints are still satisfied.
Let's see if $val(f') \geq val(f)$
- In case 1 we reduced the flow leaving $s$ (the start of the cycle) by $\delta$ and the flow entering $s$ (the end of the cycle) by $\delta$. Looking at the formula for $val(f)$, both terms decrease by the same amount. Thus, the net value remains unchanged: $val(f') = val(f)$.
- In case 2 we reduced the flow entering $s$ by $\delta$, but we did not change the flow leaving $s$ (since the path started at $t$). In the formula $val(f) = f^+(s) - f^-(s)$, the term $f^-(s)$ decreases, while $f^+(s)$ stays constant. Then, the net value increases: $val(f') = val(f) + \delta$.

## Exercise 4

### Exercise 4.1

We must prove that for a DAG $G=(V, E)$, the condition (R) is equivalent to the condition (C), where we can formalize (C) as "every node $v \neq s$ has $d_{in}(v) \ge 1$ and every node $v \neq t$ has $d_{out}(v) \ge 1$".

#### Proof that $R \implies C$

For simplicity I will divide R in R1 (every node is reachable from $s$ on some directed path) and R2 (and $t$ is reachable from every node on some directed path).
We can use a proof by contradiction, first for $d_{in}(v) \ge 1$ and then for for $d_{out}(v) \ge 1$.

**Proof for $d_{in}(v) \ge 1$:**
Let's assume there exists a node $v \neq s$ with $d_{in}(v) = 0$ (that is, no incoming edges). Since $G$ is a DAG, any path reaching $v$ must terminate at $v$ via an incoming edge. If $v$ has no incoming edges, then $v$ cannot be reached from any other node, including the source $s$. This directly contradicts R1. Therefore, $d_{in}(v) \ge 1$ must hold for all $v \neq s$.
    
**Proof for $d_{out}(v) \ge 1$:**
Assume there exists a node $v \neq t$ with $d_{out}(v) = 0$ (that is, no outgoing edges). Since $v$ has no outgoing edges, there is no path leading from $v$ to $t$. This directly contradicts requirement R2. Therefore, $d_{out}(v) \ge 1$ must hold for all $v \neq t$.

Since both sub-conditions are necessary consequences of R, we conclude that if R holds, then C must hold.

#### Proof that $C \implies R$

Like the proof above, I will divide R in R1 and R2.
We can still use a proof by contradiction, first for R1 and then for R2.

**Proof for R1: every node $v$ is reachable from $s$**
Let $U$ be the set of nodes that are not reachable from $s$. We assume, for contradiction, that $U$ is non-empty. Since $G$ is a DAG, it admits a topological ordering. We select $u$ as the first node of $U$ that appears in the topological ordering. Since $u$ is unreachable and $u \neq s$, by constraint C, $u$ must have at least one incoming edge ($d_{in}(u) \ge 1$). Let $(n, u)$ be one such incoming edge. Because the ordering is topological, $n$ must appear before $u$ in the ordering. Since $u$ was chosen as the first unreachable node, its predecessor $n$ must be reachable from $s$.    
But if $n$ is reachable from $s$, then $u$ is reachable from $s$ via the path to $n$ followed by the edge $(n, u)$. This contradicts our initial assumption that $u$ is unreachable. Therefore, the set $U$ must be empty, and every node is reachable from $s$.

**Proof for R2: $t$ is reachable from every node $v$.**
This is the dual of the previous proof. We can consider the transposed graph $G^T$, where all edges are flipped. In $G^T$, the constraint $d_{out}(v) \ge 1$ in $G$ means that every node $v \neq t$ has $d_{in}^T(v) \ge 1$. We can now apply the exact same topological induction used for R1 on $G^T$, treating $t$ as the source. This proves that in $G^T$, $t$ can reach every node. Therefore, $t$ is reachable from every node in the original graph $G$.

Since both R and R2 are proven, we conclude that if C holds, then R holds.

Thus, a DAG $G$ satisfies property R if and only if every node $v \neq s$ has at least one incoming edge, and every node $v \neq t$ has at least one outgoing edge.

### Exercise 4.2

The original problem is to maximize the number of deleted edges. This is mathematically equivalent to minimizing the size of the required set of retained edges $|E_{m}|$. So $\max \text{Deleted Edges} = |E| - \min |E_{m}|$

By the result of Exercise 4.1, the retained set $E_{m}$ must satisfy the C condition (every node $v \neq s$ requires an incoming edge, and every node $u \neq t$ requires an outgoing edge)

We can use the constructed bipartite graph $B$, which has $|V(B)| = 2n-2$ vertices. The vertices $V(B) = V_0 \cup V_1$ explicitly represent all $2n-2$ connectivity requirements of Condition (C):

- $V_0 = \{u^0 \mid u \ne t\}$: nodes requiring an outgoing edge
    
- $V_1 = \{v^1 \mid v \ne s\}$: nodes requiring an incoming edge

A retained edge $(u, v)$ in $G$ corresponds to an edge $(u^0, v^1)$ in $B$. Retaining a set of edges satisfies C if and only if the corresponding set of edges in $B$ constitutes an edge cover.

Therefore, finding the minimum number of retained edges $|E_{m}|$ is equivalent to finding the minimum size of an edge cover, $\min |E_{c}|$, in the bipartite graph $B$.

We can now use the relation between the minimum edge cover and the maximum matching:

$$\min |E_{c}| = |V(B)| - |M|$$

Since $|V(B)|$ is a constant ($2n-2$), the formula shows that minimizing the number of retained edges $|E_{m}|$ is equivalent to maximizing the size of the matching $|M|$ in the graph $B$.

By substituting this relationship back into the max deletion problem, we get $\max \text{Deleted Edges} = |E| - (2n - 2) + |M|$

Thus, the problem is solved by finding the maximum matching in the constructed bipartite graph $B$.

Algorithm:

To obtain the set of edges to delete, we perform the following steps:

1. We compute a maximum matching $M$ in the bipartite graph $B$ (we proved how to do it during the lessons).
2. We start with $E_{m} = M$. All nodes incident to these edges satisfy condition (C).
3. For every vertex $u^0 \in V_0$ that is not covered by $M$ (unmatched), select any available edge $(u^0, v^1)$ in $B$ and add it to $E_{m}$.
4. For every vertex $v^1 \in V_1$ that is not covered by $M$ (unmatched), select any available edge $(u^0, v^1)$ in $B$ and add it to $E_{m}$.
5. The final set of edges to delete from the original graph $G$ is the set of all edges not corresponding to those in $E_{m}$, that is $E \setminus E_{m}$.

We can also verify that the number of edges retained by this procedure matches exactly the theoretical minimum derived from the edge cover theorem. The algorithm first selects the edges from the matching, contributing exactly $|M|$ edges. The number of remaining unmatched vertices is the total vertices minus the matched vertices, which is $|V(B)| - 2|M|$. Since the algorithm selects exactly one additional edge for each unmatched vertex, the total size of the retained set is $|M| + (|V(B)| - 2|M|)$. This simplifies to $|V(B)| - |M|$, which is precisely the formula for the minimum edge cover derived earlier.

### Exercise 4.3

Let $n$ be the number of nodes and $m$ the number of edges in the original graph $G$.

The total time complexity is the sum of three components:

$$T = T_{bipartite} + T_{matching} + T_{selection}$$

Let's analyze $T_{bipartite}$, that is the construction of the bipartite graph. We create $2n-2$ vertices ($V_0$ and $V_1$). This takes $O(n)$.
Then, we iterate through the $m$ edges of $G$. For each edge $(u, v)$, we add exactly one edge $(u^0, v^1)$ to $B$. This takes $O(m)$. Then $T_{bipartite} = O(n + m)$.

Then we have the Maximum Bipartite Matching ($T_{matching}$). The entire algorithm is dominated by the calculation of $M$.
To calculate $|M|$ we can reduce the problem to a maximum flow problem (as we saw during the lesson), so $T_{matching} = O(m \cdot n)$.

Finally we have the selection of retained edges, that is $T_{selection}$. In this phase, we iterate through the matching $M$, and since $|M| < n$, identifying these edges takes $O(n)$. Then we iterate through all vertices of $B$ ($2n-2$ nodes) to check if they are covered by $M$. This scan takes $O(n)$.
For each unmatched node, we select one incident edge. In an adjacency list representation, accessing the first neighbor of a node takes constant time $O(1)$. In the worst case, if we scan neighbors, the total time over all nodes is proportional to the number of edges, **$O(m)$**.
Finally, we compare the retained set with the original set to determine deletions, taking $O(m)$.
Thus, $T_{selection} = O(n + m)$.

Since $O(n + m)$ is asymptotically negligible compared to $O(nm)$ (assuming the graph is connected enough such that $m \ge n$), the total time complexity is dominated by the matching phase.
Thus, the resulting algorithm runs in $O(n \cdot m)$.