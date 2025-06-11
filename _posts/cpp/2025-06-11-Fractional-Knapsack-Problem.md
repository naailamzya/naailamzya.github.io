---
title: "Fractional Knapsack Problem"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Greedy, Algorithm, C++]
---

# Fractional Knapsack Problem

The **Fractional Knapsack Problem** is a classic example of a greedy algorithm where items can be divided into smaller parts. Unlike the 0/1 Knapsack problem where we must take the whole item or leave it, here we can take any fraction of an item. This allows for optimal value collection within a limited knapsack capacity.

---

## Problem Statement

You are given:

- A knapsack with maximum capacity `W`.
- A list of `n` items, each with:
  - `value[i]`: the benefit you gain by putting it in the knapsack.
  - `weight[i]`: how much space it occupies.

**Goal**: Maximize the total value in the knapsack by possibly taking fractions of items.

---

## Greedy Strategy

To solve this using a greedy approach:

1. Calculate the **value-to-weight ratio** for each item.
2. Sort items in **descending order** based on this ratio.
3. Start adding items:
   - If the item fits completely, take it.
   - Otherwise, take the fraction that fits the remaining capacity.

This strategy ensures that you always pick the most valuable item per unit weight first.

---

## C++ Code Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Item {
    int value, weight;
};

// Comparator to sort items by value per unit weight
bool cmp(Item a, Item b) {
    double r1 = (double)a.value / a.weight;
    double r2 = (double)b.value / b.weight;
    return r1 > r2;
}

double fractionalKnapsack(int W, vector<Item> items) {
    sort(items.begin(), items.end(), cmp);

    double totalValue = 0.0;

    for (Item& item : items) {
        if (W == 0) break;

        if (item.weight <= W) {
            W -= item.weight;
            totalValue += item.value;
        } else {
            totalValue += item.value * ((double)W / item.weight);
            W = 0;
        }
    }

    return totalValue;
}

int main() {
    int W = 50;
    vector<Item> items;
    items.push_back(Item{60, 10});
    items.push_back(Item{100, 20});
    items.push_back(Item{120, 30});

    double maxValue = fractionalKnapsack(W, items);
    cout << "Maximum value in knapsack = " << maxValue << endl;
    return 0;
}
```

---

## Example Input

- Knapsack capacity = 50  
- Items:  
  - Value: 60, Weight: 10  
  - Value: 100, Weight: 20  
  - Value: 120, Weight: 30

### Output

```
Maximum value in knapsack = 240
```

---

## Why This Works

Fractional Knapsack allows item splitting, so always choosing the **highest value density** (value/weight) leads to the optimal result. That’s why the greedy strategy works perfectly here — no need for backtracking or dynamic programming.

---

## Summary

- Fractional knapsack enables **partial item selection**.
- Use **greedy approach** to get maximum value.
- Sort by **value per unit weight**.
- Efficient and easy to implement.
- Real-world applications include cargo loading, investment selection, etc.

---

*Posted by Naailah Mazaya 
