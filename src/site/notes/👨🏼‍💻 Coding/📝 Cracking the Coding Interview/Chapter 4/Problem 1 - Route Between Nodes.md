---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-4/problem-1-route-between-nodes/","created":"2023-01-27T16:46:33.838+01:00","updated":"2023-01-29T19:06:31.041+01:00"}
---

# Chapter 4 - Problem 1 - Route Between Nodes
## Problem
Given a directed graph, design an algorithm to find out whether there is a route between two nodes.

#### Solution in C++
This problem can be solved by just simple graph traversal, such as depth-first search or breadth-first search.
We start with one of the two nodes and, during traversal, check if the other node is found. We should mark
any node found in the course of the algorithm as "already visited" to avoid cycles and repetition of the
nodes.

>[!info] Which is better?
>Depth-First Search is a bit simpler to implement since it can be done with simple recursion. Breadth-First Search can also be useful to find the shortest path, whereas Depth-First Search may traverse one adjacent node very deeply before ever going onto the immediate neighbours.

##### Solution using recursive DFS
```cpp
class Graph {
    private:
        vector<vector<int> > graph;
        int numVertices;
        
        bool DFS(int start, int end, vector<bool>& visited) {
            if(start == end) return true;
            visited.at(start) = true;
            for(int adj : graph.at(start)) {
                if(!visited.at(adj)) {
	                /* If i find the end node 'true' will be passed up in the stack */
                    if(DFS(adj, end, visited))
                        return true;
                }
            }
            return false;
        }
    public:
        bool routeBetweenNodes(int start, int end) {
            vector<bool> visited(numVertices, false);
            return DFS(start, end, visited);
        }
};
```
- **Time complexity:** $O(M + N)$ (where _N_ is the number of nodes, _M_ the number of edges)
- **Space complexity:** $O(N)$
##### Solution using BFS
```cpp
bool routeBetweenNodes(int start, int end) {
            if(start == end) return true;

            queue<int> queue;
            vector<bool> visited(numVertices, false);
            visited.at(start) = true;
            queue.push(start);
            while(!queue.empty()) {
                int v = queue.front();
                queue.pop();
                for(int adj : graph.at(v)) {
                    if(!visited.at(adj)) {
                        if(adj == end) return true;
                        visited.at(adj) = true;
                        queue.push(adj);
                    }
                }
            }
            return false;
        }
```
- **Time complexity:** $O(M + N)$ (where _N_ is the number of nodes, _M_ the number of edges)
- **Space complexity:** $O(N)$