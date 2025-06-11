
---
title: "N-Queens Problem"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Backtracking, Algorithm, C++]
---

# N-Queens Problem

The N-Queens Problem is a classic combinatorial problem that challenges us to place N non-attacking queens on an N x N chessboard. This means no two queens can share the same row, column, or diagonal. It's a prime example of a problem best solved using a backtracking algorithm.

## Problem Statement

Given an integer N, the task is to find all distinct configurations (or simply one configuration) where N queens can be placed on an N x N chessboard such that no two queens threaten each other.

A queen can attack horizontally, vertically, and diagonally. Therefore, for a configuration to be valid, every pair of queens must satisfy these conditions:

- They must not be in the same row.
- They must not be in the same column.
- They must not be on the same diagonal (both main diagonals and anti-diagonals).

## Backtracking Algorithm

The N-Queens problem is typically solved using a backtracking approach. This involves trying to place a queen in each column, one by one, and if a placement leads to a conflict, we "backtrack" and try a different position.

Here are the general steps:

1. Start from the first column (column 0).
2. For each column, iterate through all possible rows to place a queen.
3. Check if a position `(row, col)` is safe:
   - Verify that no other queen is in the same row.
   - Verify that no other queen is in the same column (this is implicitly handled by placing one queen per column).
   - Verify that no other queen is on the same upper-left to lower-right diagonal (`row - col` is constant).
   - Verify that no other queen is on the same lower-left to upper-right diagonal (`row + col` is constant).
4. If the position is safe:
   - Place a queen at `(row, col)`.
   - Recursively call the function for the next column `(col + 1)`.
   - If the recursive call for the next column returns true (meaning a solution was found), then propagate true back.
5. If the recursive call returns false (meaning no solution was found from that point), or if the current position `(row, col)` was not safe:
   - Backtrack: Remove the queen from `(row, col)`.
   - Try the next row in the current column.
6. **Base Case**: If all N queens have been successfully placed (`col == N`), then a valid solution has been found. Store or print this solution.

## C++ Code Example

```cpp
#include <iostream>
#include <vector>
#include <string>

// Function to print a single solution
void printBoard(const std::vector<std::string>& board) {
    for (const std::string& row : board) {
        std::cout << row << std::endl;
    }
    std::cout << std::endl;
}

// Function to check if it's safe to place a queen at board[row][col]
bool isSafe(const std::vector<std::string>& board, int row, int col, int N) {
    for (int i = 0; i < col; i++) {
        if (board[row][i] == 'Q') {
            return false;
        }
    }
    for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') {
            return false;
        }
    }
    for (int i = row, j = col; i < N && j >= 0; i++, j--) {
        if (board[i][j] == 'Q') {
            return false;
        }
    }
    return true;
}

// Recursive function to solve N-Queens problem
void solveNQueensUtil(std::vector<std::string>& board, int col, int N) {
    if (col >= N) {
        printBoard(board);
        return;
    }
    for (int i = 0; i < N; i++) {
        if (isSafe(board, i, col, N)) {
            board[i][col] = 'Q';
            solveNQueensUtil(board, col + 1, N);
            board[i][col] = '.'; 
        }
    }
}

// Main function to solve N-Queens problem
void solveNQueens(int N) {
    std::vector<std::string> board(N, std::string(N, '.'));
    solveNQueensUtil(board, 0, N);
}

int main() {
    int N = 4;
    std::cout << "Solutions for N = " << N << " Queens:\n";
    solveNQueens(N);

    N = 8;
    // std::cout << "Solutions for N = " << N << " Queens:\n";
    // solveNQueens(N);

    return 0;
}
```

### Example Output (for N=4)
```
.Q..
...Q
Q...
..Q.

..Q.
Q...
...Q
.Q..
```

## Why This Works

The backtracking algorithm works by systematically exploring all potential placements for queens. The `isSafe` function acts as a pruning mechanism, allowing the algorithm to abandon branches (i.e., configurations) that are guaranteed not to lead to a solution early on. Instead of trying every single combination of N queens on the board (which would be N^N possibilities), backtracking significantly reduces the search space by eliminating invalid paths as soon as a conflict is detected. This makes it a much more efficient approach than a brute-force method.

## Summary

The N-Queens Problem is a classic puzzle requiring the placement of non-attacking queens on a chessboard.

- It is optimally solved using a backtracking algorithm, which is a general algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions that fail to satisfy the constraints.
- The `isSafe` function is crucial for efficiently pruning the search space.
- Applications extend beyond puzzles to fields like constraint satisfaction problems and artificial intelligence.

Posted by **Naailah Mazaya**
