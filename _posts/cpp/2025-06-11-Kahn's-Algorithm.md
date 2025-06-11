---
title: "Kahn's Algorithm for Topological Sort"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Graph, Algorithm, Topological Sort]
---

# Kahn's Algorithm for Topological Sort

Kahn's Algorithm is an algorithm for performing topological sorting on a directed acyclic graph (DAG). A topological sort (also known as a topological ordering) of a DAG is a linear ordering of its vertices such that for every directed edge from vertex `u` to vertex `v`, `u` comes before `v` in the ordering. Kahn's Algorithm uses the concept of **in-degree** (the number of incoming edges to a vertex) to determine the order of vertices.

---

## Problem Statement

You are given:

- A directed acyclic graph (DAG) `G = (V, E)`, where `V` is the set of vertices and `E` is the set of directed edges.

**Goal**: Produce a linear ordering of all vertices such that for every directed edge `(u, v)`, vertex `u` appears before vertex `v` in the ordering.  
If multiple such orderings exist, any valid one is acceptable. If the graph contains a cycle, a topological sort is not possible.

---

## 🧮 Kahn's Algorithm Strategy

Kahn's Algorithm iteratively identifies and removes vertices that have no incoming edges (an in-degree of `0`). These vertices can be placed first in the topological order. When a vertex is "removed", its outgoing edges are also conceptually removed, which may reduce the in-degree of its neighbors to `0`, making them eligible for the next step.

### Steps:

1. **Calculate In-Degrees**  
   For every vertex in the graph, compute its in-degree using an array or map.

2. **Initialize Queue**  
   Add all vertices with an in-degree of `0` to a queue. These are the starting points.

3. **Process Vertices**
   - Create an empty list `topological_order`.
   - While the queue is not empty:
     - Dequeue a vertex `u`.
     - Add `u` to `topological_order`.
     - For each neighbor `v` of `u` (i.e., for each edge `(u, v)`):
       - Decrement the in-degree of `v` by 1.
       - If in-degree becomes `0`, enqueue `v`.

4. **Detect Cycle (Optional but Recommended)**
   - Keep a count of how many vertices are added to `topological_order`.
   - If at the end, this count is less than the total number of vertices, the graph contains a cycle.

---

## C++ Code Example

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <algorithm> // For std::find

// Function to perform topological sort using Kahn's Algorithm
std::vector<int> kahnTopologicalSort(int numVertices, const std::map<int, std::vector<int>>& adj) {
    std::map<int, int> inDegree;
    for (int i = 0; i < numVertices; ++i) {
        inDegree[i] = 0;
    }

    for (auto const& [u, neighbors] : adj) {
        for (int v : neighbors) {
            inDegree[v]++;
        }
    }

    std::queue<int> q;
    for (int i = 0; i < numVertices; ++i) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }

    std::vector<int> topologicalOrder;
    int visitedCount = 0;

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        topologicalOrder.push_back(u);
        visitedCount++;

        if (adj.count(u)) {
            for (int v : adj.at(u)) {
                inDegree[v]--;
                if (inDegree[v] == 0) {
                    q.push(v);
                }
            }
        }
    }

    if (visitedCount != numVertices) {
        std::cout << "Graph contains a cycle. Topological sort not possible for all vertices." << std::endl;
        return {};
    }

    return topologicalOrder;
}

int main() {
    // Example 1
    int numVertices1 = 6;
    std::map<int, std::vector<int>> adj1 = {
        {0, {}},
        {1, {}},
        {2, {3}},
        {3, {1}},
        {4, {0, 1}},
        {5, {0, 2}}
    };

    std::cout << "--- Graph 1 (Acyclic) ---" << std::endl;
    std::vector<int> result1 = kahnTopologicalSort(numVertices1, adj1);
    if (!result1.empty()) {
        std::cout << "Topological Sort Order: ";
        for (int vertex : result1) {
            std::cout << vertex << " ";
        }
        std::cout << std::endl;
    }

    // Example 2
    int numVertices2 = 3;
    std::map<int, std::vector<int>> adj2 = {
        {0, {1}},
        {1, {2}},
        {2, {0}}
    };

    std::cout << "\n--- Graph 2 (Cyclic) ---" << std::endl;
    std::vector<int> result2 = kahnTopologicalSort(numVertices2, adj2);

    // Example 3
    int numVertices3 = 7;
    std::map<int, std::vector<int>> adj3 = {
        {0, {1, 2}},
        {1, {3}},
        {2, {3, 4}},
        {3, {5}},
        {4, {6}},
        {5, {6}},
        {6, {}}
    };

    std::cout << "\n--- Graph 3 (Acyclic) ---" << std::endl;
    std::vector<int> result3 = kahnTopologicalSort(numVertices3, adj3);
    if (!result3.empty()) {
        std::cout << "Topological Sort Order: ";
        for (int vertex : result3) {
            std::cout << vertex << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}

---
## Example Input and Output

## Input 1 (Graph 1):
Graph:
0: {}
1: {}
2: {3}
3: {1}
4: {0, 1}
5: {0, 2}
Number of Vertices: 6

## Output
Topological Sort Order: 4 5 0 2 3 1 
(Other valid orders exist, e.g., 5 4 0 2 3 1)

----
## Why This Works

Kahn's algorithm **correctly produces a topological sort** because:

- It **systematically removes vertices** with no dependencies (in-degree = 0).
- By processing "independent" vertices first, it **simulates safe edge removal**.
- If at some point there are no more nodes with in-degree 0 but some nodes are still unvisited, it means there is a **cycle**.

The **queue** ensures a breadth-first processing of dependencies.

### ⏱Time Complexity
Where `V` is the number of vertices and `E` is the number of edges.

---

## Summary

- **Kahn's Algorithm** is used for **Topological Sorting** of a **Directed Acyclic Graph (DAG)**.
- It is based on calculating **in-degrees** and using a **queue** to process vertices in the correct order.

### Steps:

1. Calculate in-degrees of all vertices.
2. Add all vertices with in-degree `0` to the queue.
3. While the queue is not empty:
   - Remove a vertex from the queue and add it to the result list.
   - Decrease the in-degree of all its neighbors by 1.
   - If any neighbor's in-degree becomes `0`, add it to the queue.
4. After processing, if the size of the result list is **less than the number of vertices**, then the graph contains a **cycle**.

### Cycle Detection:

- If not all nodes are included in the topological sort result → **the graph has a cycle**.

**Posted by Naailah Mazaya**