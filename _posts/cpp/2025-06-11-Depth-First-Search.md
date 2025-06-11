---
title: "Depth-First Search Algorithm"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Graph, Algorithm, DFS]
---

# Depth-First Search (DFS) Algorithm

**Depth-First Search (DFS) Algorithm**  
Depth-First Search (DFS) is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root node (or an arbitrary node in a graph) and explores as far as possible along each branch before backtracking. DFS is a recursive algorithm (though it can also be implemented iteratively with a stack) and is often used to find paths or to explore all nodes and edges of a graph.

## Problem Statement
Given a graph (which can be represented as an adjacency list or matrix) and a starting node (source), the Goal is to:

1. Traverse all reachable nodes from the starting node.
2. Explore the graph's structure in a "deep" manner before exploring broadly.

DFS explores the graph by going as deep as possible along each branch before returning to backtrack and explore other branches.

## DFS Strategy
The DFS algorithm typically uses a stack data structure (implicitly through recursion, or explicitly for iterative implementation) to manage the order of node visits.

### Recursive Approach:
1. **Initialization**:
   - Create a visited set/array
   - Mark all nodes as unvisited

2. **Recursive Traversal**:
   ```pseudo
   dfs(current_node):
       mark current_node as visited
       process current_node (e.g., print)
       for each unvisited neighbor of current_node:
           dfs(neighbor)

3. **Termination**:
    1. Uses explicit stack
    2. Processes nodes in LIFO order
    3. Often processes neighbors in reverse order to match recursive behavior

---
## C++ Code Example
#include <iostream>
#include <vector>
#include <map>
#include <stack>

// Recursive DFS implementation
void dfs_recursive_util(const std::map<int, std::vector<int>>& adj, 
                       int currentNode, 
                       std::map<int, bool>& visited) {
    visited[currentNode] = true;
    std::cout << currentNode << " ";
    
    if (adj.count(currentNode)) {
        for (int neighbor : adj.at(currentNode)) {
            if (!visited[neighbor]) {
                dfs_recursive_util(adj, neighbor, visited);
            }
        }
    }
}

// Iterative DFS implementation
void dfs_iterative(const std::map<int, std::vector<int>>& adj, int startNode) {
    std::stack<int> s;
    std::map<int, bool> visited;

    for (auto const& [node, neighbors] : adj) {
        visited[node] = false;
    }

    s.push(startNode);
    visited[startNode] = true;

    while (!s.empty()) {
        int currentNode = s.top();
        s.pop();
        std::cout << currentNode << " ";

        if (adj.count(currentNode)) {
            // Process in reverse to match recursive order
            for (int i = adj.at(currentNode).size() - 1; i >= 0; --i) {
                int neighbor = adj.at(currentNode)[i];
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    s.push(neighbor);
                }
            }
        }
    }
}

---

## Example Input and Output

**Input 1 (Graph 1)**:
Graph:
0: {1, 2}
1: {3, 4}
2: {4}
3: {}
4: {5}
5: {}
Start Node: 0

**Output**
DFS Traversal (Recursive) starting from node 0:
0 1 3 4 5 2 

DFS Traversal (Iterative) starting from node 0:
0 2 4 5 1 3 


----
## 🔍 Why This Works

DFS bekerja dengan cara mendahulukan eksplorasi ke kedalaman dibandingkan lebar. Ketika mengunjungi simpul, DFS langsung menyusuri tetangga yang belum dikunjungi sedalam mungkin. Jika mencapai *jalan buntu*, algoritma akan melakukan *backtrack* ke simpul sebelumnya dan melanjutkan eksplorasi cabang lainnya.

### Poin penting:
- Menggunakan **set/array visited** untuk mencegah *infinite loop* (pada graf ber-siklus).
- Stack (eksplisit atau via rekursi) digunakan untuk mengelola urutan eksplorasi.
- Waktu eksekusi DFS adalah **O(V + E)**, di mana `V` adalah jumlah simpul (vertices) dan `E` jumlah sisi (edges), karena setiap simpul dan sisi akan dikunjungi maksimal satu kali.

---

## Summary

- **Depth-First Search (DFS)** adalah algoritma penelusuran graf yang menyusuri simpul secara mendalam sebelum *backtrack*.
- Dapat diimplementasikan secara **rekursif** (stack implisit) atau **iteratif** (stack eksplisit).
- **Aplikasi:** pencarian jalur, deteksi siklus, *topological sorting*, pencarian komponen terhubung, penyelesaian labirin.
- **Kompleksitas waktu:** `O(V + E)`.
- Sangat bermanfaat dalam berbagai masalah komputasi graf dan algoritma.

---

**Posted by Naailah Mazaya**
