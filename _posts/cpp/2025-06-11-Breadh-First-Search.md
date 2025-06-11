# Breadth-First Search (BFS) Algorithm

**Date:** 2025-06-11  
**Categories:** [C++]  
**Tags:** [Graph, Algorithm, BFS]  

🧠 **Breadth-First Search (BFS) Algorithm**  
Breadth-First Search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph, sometimes referred to as a 'search key') and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. BFS is a level-order traversal and is often used to find the shortest path in an unweighted graph.

## Problem Statement
Given a graph (which can be represented as an adjacency list or matrix) and a starting node (source), the Goal is to:

1. Traverse all reachable nodes from the starting node.
2. Find the shortest path (in terms of number of edges) from the source node to all other reachable nodes in an unweighted graph.

BFS explores the graph layer by layer, ensuring that all nodes at a given distance k from the source are visited before any nodes at distance k+1.

## 🧮 BFS Strategy
The BFS algorithm typically uses a queue data structure to manage the order of node visits.

Here are the general steps:

### Initialization:
- Create a queue and add the starting (source) node to it.
- Mark the starting node as visited.
- (Optional, for shortest path) Initialize distances of all nodes to infinity and the source node's distance to 0.

### Traversal:
While the queue is not empty:
1. Dequeue a node (let's call it `current_node`).
2. Process `current_node` (e.g., print it, check if it's the target node).
3. For each unvisited neighbor (`neighbor`) of `current_node`:
   - Mark `neighbor` as visited.
   - Enqueue `neighbor`.
   - (Optional, for shortest path) Set `distance[neighbor] = distance[current_node] + 1`.

### Termination:
The algorithm terminates when the queue becomes empty, indicating that all reachable nodes have been visited.

## 💻 C++ Code Example

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>

// Function to perform Breadth-First Search on a graph
void bfs(const std::map<int, std::vector<int>>& adj, int startNode) {
    // Check if the startNode exists in the graph
    if (adj.find(startNode) == adj.end()) {
        std::cout << "Start node " << startNode << " not found in graph." << std::endl;
        return;
    }

    std::queue<int> q;
    std::map<int, bool> visited; // To keep track of visited nodes

    // Mark all nodes as not visited initially
    for (auto const& [node, neighbors] : adj) {
        visited[node] = false;
    }

    // Enqueue the starting node and mark it as visited
    q.push(startNode);
    visited[startNode] = true;

    std::cout << "BFS Traversal starting from node " << startNode << ":\n";

    while (!q.empty()) {
        int currentNode = q.front();
        q.pop();

        std::cout << currentNode << " "; // Process the current node (e.g., print it)

        // Iterate through all neighbors of the current node
        // Ensure that the current node actually has neighbors in the map
        if (adj.count(currentNode)) {
            for (int neighbor : adj.at(currentNode)) {
                // If the neighbor has not been visited, mark it and enqueue it
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
    }
    std::cout << std::endl;
}

int main() {
    // Representing the graph using an adjacency list (map of vectors)
    std::map<int, std::vector<int>> graph1 = {
        {0, {1, 2}},
        {1, {0, 3, 4}},
        {2, {0, 4}},
        {3, {1, 5}},
        {4, {1, 2, 5}},
        {5, {3, 4}}
    };

    std::cout << "--- Graph 1 ---" << std::endl;
    bfs(graph1, 0); // Perform BFS starting from node 0
    bfs(graph1, 3); // Perform BFS starting from node 3

    std::map<int, std::vector<int>> graph2 = {
        {1, {2, 3}},
        {2, {1, 4}},
        {3, {1, 5}},
        {4, {2}},
        {5, {3}}
    };

    std::cout << "\n--- Graph 2 ---" << std::endl;
    bfs(graph2, 1); // Perform BFS starting from node 1
    bfs(graph2, 6); // Try BFS with a non-existent start node

    return 0;
}

---
## Example Input and Output

### Input 1 (Graph 1):
```plaintext
Graph:
0: {1, 2}
1: {0, 3, 4}
2: {0, 4}
3: {1, 5}
4: {1, 2, 5}
5: {3, 4}
Start Node: 0

## Output
BFS Traversal starting from node 0:
0 1 2 3 4 5 

---
## Input 2
Graph:
1: {2, 3}
2: {1, 4}
3: {1, 5}
4: {2}
5: {3}
Start Node: 1

---
## Output
BFS Traversal starting from node 1:
1 2 3 4 5 

## Why This Work
BFS works by systematically exploring the graph level by level. The use of a queue is crucial: it ensures that all nodes at the current depth are visited before moving to the next depth. This property guarantees that for unweighted graphs, BFS finds the shortest path (in terms of the number of edges) from the source to any reachable node.

Each node is visited at most once

Each edge is examined at most twice (once for each direction in undirected graphs)

Time complexity: O(V+E) where V = number of vertices, E = number of edges

---
## Summary
Algorithm Type: Graph traversal (level-order)

Data Structure: Queue

Key Property: Finds shortest path in unweighted graphs

Time Complexity: O(V+E)

Applications:

Shortest path finding

Network broadcasting

Web crawlers

Connected components detectio

---

*Posted by Naailah Mazaya*