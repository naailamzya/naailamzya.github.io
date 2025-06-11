---
title: "Dijkstra's Algorithm for Shortest Path"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Graph, Algorithm, Shortest Path]
---

# Dijkstra's Algorithm for Shortest Path

Dijkstra's Algorithm is a single-source shortest path algorithm for a graph with non-negative edge weights. It finds the shortest paths from a single source vertex to all other vertices in a graph. It's a greedy algorithm that systematically expands the path from the source, always choosing the unvisited vertex with the smallest known distance.

---

## Problem Statement

You are given:

- A weighted graph G = (V, E), where V is the set of vertices and E is the set of edges.
- Each edge (u, v) has a non-negative weight w(u, v).
- A designated source vertex s.

**Goal**: Find the shortest path (i.e., the path with the minimum total weight) from the source vertex s to every other reachable vertex in the graph.

---

## Dijkstra's Algorithm Strategy

Dijkstra's algorithm maintains a set of visited vertices and an array `dist` where `dist[v]` stores the shortest distance found so far from the source to vertex `v`. It uses a **priority queue** to efficiently select the unvisited vertex with the minimum distance.

### General Steps:

1. **Initialization**:
   - Create a dist array/map and initialize all distances to infinity, except for the source vertex, which is 0.
   - Create a min-priority queue and insert the source vertex with its distance (0).
   - Create a visited set/array to track finalized shortest paths.

2. **Processing Vertices**:
   - While the priority queue is not empty:
     - Extract the vertex `u` with the minimum distance.
     - If `u` has been visited, skip.
     - For each neighbor `v` of `u`, if a shorter path is found, update `dist[v]` and add it to the priority queue.

3. **Termination**:
   - When the queue is empty, `dist[v]` contains the shortest path from the source to `v` for all reachable nodes.

---

## C++ Code Example

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <limits> // For std::numeric_limits

typedef std::pair<int, int> pii; // {distance, vertex}

void dijkstra(int numVertices, const std::map<int, std::vector<std::pair<int, int>>>& adj, int startNode) {
    if (adj.find(startNode) == adj.end() && numVertices > 0 && startNode < numVertices) {
        // Assume valid
    } else if (startNode >= numVertices || startNode < 0) {
        std::cout << "Error: Start node " << startNode << " is out of bounds for " << numVertices << " vertices." << std::endl;
        return;
    }

    std::map<int, int> dist;
    for (int i = 0; i < numVertices; ++i) dist[i] = std::numeric_limits<int>::max();
    dist[startNode] = 0;

    std::priority_queue<pii, std::vector<pii>, std::greater<pii>> pq;
    pq.push({0, startNode});

    std::map<int, bool> visited;
    for (int i = 0; i < numVertices; ++i) visited[i] = false;

    std::cout << "Dijkstra's Algorithm starting from node " << startNode << ":\n";

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();
        if (visited[u]) continue;
        visited[u] = true;

        if (adj.count(u)) {
            for (auto& edge : adj.at(u)) {
                int v = edge.first, weight = edge.second;
                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.push({dist[v], v});
                }
            }
        }
    }

    for (int i = 0; i < numVertices; ++i) {
        if (dist[i] == std::numeric_limits<int>::max()) {
            std::cout << "Distance from " << startNode << " to " << i << ": INFINITY" << std::endl;
        } else {
            std::cout << "Distance from " << startNode << " to " << i << ": " << dist[i] << std::endl;
        }
    }
}

int main() {
    int numVertices1 = 5;
    std::map<int, std::vector<std::pair<int, int>>> adj1 = {
        {0, {{1, 10}, {4, 3}}},
        {1, {{2, 2}}},
        {2, {{3, 9}}},
        {3, {{4, 4}}},
        {4, {{1, 1}, {2, 8}, {3, 2}}}
    };
    std::cout << "--- Graph 1 ---" << std::endl;
    dijkstra(numVertices1, adj1, 0);

    int numVertices2 = 4;
    std::map<int, std::vector<std::pair<int, int>>> adj2 = {
        {0, {{1, 5}, {2, 2}}},
        {1, {{3, 3}}},
        {2, {{1, 1}}}
    };
    std::cout << "\n--- Graph 2 ---" << std::endl;
    dijkstra(numVertices2, adj2, 0);

    std::cout << "\n--- Graph 2 (starting from node 3 - isolated) ---" << std::endl;
    dijkstra(numVertices2, adj2, 3);
    return 0;
}

---

## Contoh Input dan Output

### Input 1 (Graph 1)

**Graph (Adjacency List)**:
```
cpp
{
    0: {{1, 10}, {4, 3}},
    1: {{2, 2}},
    2: {{3, 9}},
    3: {{4, 4}},
    4: {{1, 1}, {2, 8}, {3, 2}}
}
Dijkstra's Algorithm starting from node 0:
Distance from 0 to 0: 0
Distance from 0 to 1: 4   (0 → 4 → 1)
Distance from 0 to 2: 6   (0 → 4 → 1 → 2)
Distance from 0 to 3: 6   (0 → 4 → 3)
Distance from 0 to 4: 3   (0 → 4)

---

## Why This Works

Dijkstra's algorithm relies on the **greedy strategy** and the property that all edge weights are **non-negative**. It ensures that once a vertex is visited (finalized), the shortest path to it has been found. Here's why:

- The algorithm always selects the unvisited vertex with the **smallest known distance** from the source.
- If a shorter path to this vertex existed through another vertex, it would have been selected **earlier** due to the greedy nature.
- The algorithm systematically expands outward from the source in "waves" of increasing distance, ensuring the **optimality** of each finalized path.
- The use of a priority queue ensures efficient selection of the next vertex to process, and all neighbors are "relaxed" (updated) if a better path is found through the current vertex.

Thus, the correctness of Dijkstra's algorithm is based on its greedy approach and the assumption that **edge weights are non-negative**, allowing safe early finalization of shortest paths.

---

## Summary

- **Dijkstra's Algorithm** finds the shortest path from a source node to all other nodes in a graph with **non-negative edge weights**.
- It uses a **greedy approach** and a **priority queue** to explore vertices in order of increasing distance from the source.
- The core idea is to iteratively relax the distances of neighboring vertices and finalize nodes when their shortest path is determined.
- **Limitations**:
  - Cannot handle **negative edge weights**.
  - If negative edges are present, consider using **Bellman-Ford** or **SPFA** algorithms instead.
- **Time Complexity**:
  - Using a binary heap: **O(E log V)**, where *E* is the number of edges and *V* is the number of vertices.

---

### Applications

- GPS and Navigation Systems
- Network Routing Protocols (e.g., OSPF)
- Optimal pathfinding in games and maps
- Resource allocation and scheduling problems


**Posted by Naailah Mazaya**