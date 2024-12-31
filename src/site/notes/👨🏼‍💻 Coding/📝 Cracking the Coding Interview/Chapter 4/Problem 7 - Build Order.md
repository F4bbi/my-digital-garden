---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-4/problem-7-build-order/","created":"2023-02-23T23:49:49.717+01:00","updated":"2023-02-23T23:49:49.717+01:00"}
---

# Chapter 4 - Problem 7 - Build Order
## Problem
You are given a list of projects and a list of dependencies (which is a list of pairs of projects, where the second project is dependent on the first project). All of a project's dependencies must be built before the project is. Find a build order that will allow the projects to be built. If there is no valid build order, return an error.
#### Solution in C++ using Topological Sort
As the title says, we can simply use topological sort, that is linearly ordering the vertices in the graph such that for
every edge $(a, b)$, $a$ appears before $b$ in the linear order. 
However, we should think about the issue of cycles, since the graph isn't acyclic.
A cycle will happen if, while doing a DFS on a node, we run back into the same path. What we need therefore
is a signal that indicates "I'm still processing this node, so if you see the node again, we have a problem."
What we can do for this is to mark each node as a "VISITING" state just before we start the DFS on it. If we see any node whose state is VISITING, then we know we have a problem. When we're done with this node's DFS, we need a state to indicate "We've already processed/built this node" so we don't re-build it. Our state therefore can have three options: COMPLETED, VISITING, and BLANK.

```cpp
class Project {
    public:
        enum class State { COMPLETE, VISITING, BLANK };
        Project(string name) { this->name = name; }
        string getName() { return this->name; }
        vector<shared_ptr<Project>> getChildren() { 
            vector<shared_ptr<Project>> shared_vec(children.begin(), children.end());
            return shared_vec;
        }
        void addChildren(shared_ptr<Project> project) { children.push_back(project); }
        State getState() { return state; }
        void setState(State state) { this->state = state; }
    private:
        //weak_ptr to avoid errors because of circular references, in case of cyclic graphs
        vector<weak_ptr<Project>> children;
        string name;
        State state = State::BLANK;
};

class Graph {
    private:
        vector<shared_ptr<Project>> projects;
        unordered_map<string, shared_ptr<Project>> graph;

        bool DFS(shared_ptr<Project> project, stack<shared_ptr<Project>>*& result) {
            project->setState(Project::State::VISITING);
            for(shared_ptr<Project> adj : project->getChildren()) {
                if(adj->getState() == Project::State::VISITING)
                    return false;
                else if(adj->getState() == Project::State::BLANK) {
                    if(!DFS(adj, result))
                        return false;
                }
            }
            result->push(project);
            project->setState(Project::State::COMPLETE);
            return true;
        }
    public:
        Graph(vector<string> projects, vector<pair<string, string> > dependencies) {
            for(string name : projects) {
                shared_ptr<Project> node = make_shared<Project>(name);
                this->projects.push_back(node);
                graph.insert(make_pair(name, node));
            }

            for(pair<string, string> dependency : dependencies)
                addEdge(dependency.first, dependency.second);
        }
        
        void addEdge(string src, string dest) {
	        graph.at(src)->addChildren(graph.at(dest));
        }

        stack<shared_ptr<Project>>* buildOrder() {
            stack<shared_ptr<Project>>* result = new stack<shared_ptr<Project>>();
            // Topological sort, we do a DFS for every node
            for(shared_ptr<Project> project : this->projects) {
                if(project->getState() == Project::State::BLANK) {
                    if(!DFS(project, result)){
                        delete result;
                        return NULL;
                    }
                }
            }
            return result;
        }
};
```
- **Time complexity:** $O(P + D)$ (where _P_ is the number of projects and _D_ the number of dependency pairs.)
- **Space complexity:** $O(P)$

#### Solution in C++ using "reversed" Topological Sort with array
Another approach is to keep track of the number of dependencies of every node while we are building the graph.
After, we start pushing the projects in an array in order of number of dependencies.
Obviously nodes with no incoming edges (no dependencies) will be inserted immediately since they don't depend on anything.
At this point, for every node in the array, we decrement the dependencies of its children and we push its children with 0 dependencies at the end of the array.
If there is a cycle there won't be nodes with 0 dependencies and an error will be returned.

```cpp
class Project {
    public:
        Project(string name) { this->name = name; }
        string getName() { return name; }
        vector<shared_ptr<Project>> getChildren() { 
            vector<shared_ptr<Project>> shared_vec(children.begin(), children.end());
            return shared_vec;
        }
        void addChildren(shared_ptr<Project> project) { children.push_back(project); }
        void incrementDependencies() { dependencies++; }
        void decrementDependencies() { dependencies--; }
        int getNumberDependencies() { return dependencies; }
    private:
        //weak_ptr to avoid errors because of circular references, in case of ciclic graphs
        vector<weak_ptr<Project>> children;
        string name;
        int dependencies = 0;
};

class Graph {
    private:
        vector<shared_ptr<Project>> projects;
        unordered_map<string, shared_ptr<Project>> graph;

        int addNonDependencies(vector<shared_ptr<Project>>*& result,
						       const vector<shared_ptr<Project>>& projects,
						       int index)
		{
            for(auto project : projects) {
                if(project->getNumberDependencies() == 0) {
                    result->at(index) = project;
                    index++;
                }
            }
            return index; 
        }
    public:
        Graph(vector<string> projects, vector<pair<string, string> > dependencies) {
            for(string name : projects) {
                shared_ptr<Project> node = make_shared<Project>(name);
                this->projects.push_back(node);
                graph.insert(make_pair(name, node));
            }
            for(pair<string, string> dependency : dependencies) {
                addEdge(dependency.first, dependency.second);
            }
        }
        
        void addEdge(string src, string dest) {
	        graph.at(src)->addChildren(graph.at(dest));
            graph.at(dest)->incrementDependencies();
        }

        vector<shared_ptr<Project>>* buildOrder() {
            vector<shared_ptr<Project>>* result = new vector<shared_ptr<Project>>(projects.size());
            // First scan to get nodes with 0 dependencies
            int endOfVector = addNonDependencies(result, this->projects, 0);
            int toBeProcessed = 0;
            for(; toBeProcessed < result->size(); toBeProcessed++) {
                shared_ptr<Project> current = result->at(toBeProcessed);
                if(current == NULL) {
                    delete result;
                    return NULL;
                }
                for(shared_ptr<Project> project : current->getChildren())
                    project->decrementDependencies();
                endOfVector = addNonDependencies(result, current->getChildren(), endOfVector);
            }
            return result;
        }
};
```
- **Time complexity:** $O(P + D)$ (where _P_ is the number of projects and _D_ the number of dependency pairs.)
- **Space complexity:** $O(P)$