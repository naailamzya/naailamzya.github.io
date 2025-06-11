---
title: "Huffman Coding"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Greedy, Algorithm, C++]
---


# Huffman Coding

**Huffman Coding** is a compression algorithm that reduces the size of data by encoding characters based on their frequencies. It is a **greedy algorithm** and builds an optimal prefix tree for lossless data compression.

---

## Basic Concept

- Characters that occur more frequently are assigned shorter codes.
- Less frequent characters are given longer codes.
- The result is a binary tree (Huffman Tree) that maps characters to binary strings.

---

## Algorithm Steps

1. Count the frequency of each character in the input.
2. Create a tree node for each character and insert it into a min heap based on frequency.
3. While there is more than one node in the heap:
   - Remove two nodes with the lowest frequencies.
   - Create a new node with these two as children; the frequency is their sum.
   - Insert the new node back into the heap.
4. Traverse the resulting tree to assign binary codes:
   - Left branch → `0`, right branch → `1`.

---

## Example

Given the characters and frequencies:

| Character | Frequency |
|-----------|-----------|
| a         | 5         |
| b         | 9         |
| c         | 12        |
| d         | 13        |
| e         | 16        |
| f         | 45        |

Resulting Huffman Codes:

| Character | Huffman Code |
|-----------|---------------|
| a         | 1100          |
| b         | 1101          |
| c         | 100           |
| d         | 101           |
| e         | 111           |
| f         | 0             |

---

## C++ Example Program

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
using namespace std;

// Tree node
struct Node {
    char ch;
    int freq;
    Node *left, *right;
    
    Node(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};

// Custom comparison for priority queue
struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;
    }
};

// Recursive function to print codes
void printCodes(Node* root, string code) {
    if (!root) return;

    if (root->ch != '#') {
        cout << root->ch << ": " << code << endl;
    }

    printCodes(root->left, code + "0");
    printCodes(root->right, code + "1");
}

int main() {
    char chars[] = {'a', 'b', 'c', 'd', 'e', 'f'};
    int freq[] = {5, 9, 12, 13, 16, 45};
    int n = sizeof(chars) / sizeof(chars[0]);

    priority_queue<Node*, vector<Node*>, Compare> pq;

    for (int i = 0; i < n; i++) {
        pq.push(new Node(chars[i], freq[i]));
    }

    while (pq.size() > 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();
        Node* combined = new Node('#', left->freq + right->freq);
        combined->left = left;
        combined->right = right;
        pq.push(combined);
    }

    Node* root = pq.top();
    printCodes(root, "");

    return 0;
}
```

---
## Conclusion
Huffman Coding is highly efficient for text compression and is widely used in formats such as .zip, .mp3, and .jpeg. By using a binary tree and a greedy approach, it minimizes the total length of encoded data.

---

*Posted by Naailah Mazaya 
