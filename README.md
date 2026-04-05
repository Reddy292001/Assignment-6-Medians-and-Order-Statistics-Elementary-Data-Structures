## Overview

This repository contains the work for Assignment 6. The assignment is divided into two main parts.  
The first part focuses on selection algorithms for finding the k-th smallest element in an array.  
The second part focuses on implementing basic data structures from scratch and discussing their performance and practical use.

## Repository Contents

- `Assignment6_Colab.ipynb` – Colab notebook
- `selection_benchmark_results.csv` – benchmark output for the selection algorithms
- `report.md` / `report.docx` – written report for the assignment
- `README.md` – instructions and summary of the work

## Part 1: Selection Algorithms

### Implemented Algorithms

This assignment includes two methods for finding the k-th smallest element in an array:

1. **Deterministic Selection (Median of Medians)**  
   This method guarantees worst-case linear time.  
   The implementation divides the array into groups of five, finds the median of each group, recursively finds the median of those medians, and uses that value as the pivot.

2. **Randomized Selection (Quickselect-style)**  
   This method chooses a pivot randomly and partitions the array around it.  
   It has expected linear running time in practice, although its worst case is still quadratic.

### Correctness and Edge Cases

The implementation includes checks for:
- invalid input arrays
- out-of-range values of `k`
- arrays with duplicate elements
- small input cases such as single-element arrays

Three-way partitioning is used so duplicate values are handled correctly.

## Performance Analysis

### Time Complexity

- **Deterministic Selection:**  
  Worst-case time complexity is **O(n)** because the Median of Medians strategy guarantees a reasonably balanced pivot.

- **Randomized Selection:**  
  Expected time complexity is **O(n)** because each partition step is linear and the pivot is chosen randomly.  
  Worst-case time complexity is **O(n²)** if poor pivots are selected repeatedly.

### Space Complexity

Both implementations create additional lists during partitioning (`less`, `equal`, and `greater`), so they are not fully in-place.

- Deterministic selection has more overhead because it also creates groups of five and a list of medians.
- Randomized selection has less overhead, which helps explain why it is usually faster in practice.

## Empirical Benchmark Results

The benchmark tests compare both selection algorithms on the following input types:

- random
- sorted
- reverse sorted
- many duplicates

The tested input sizes are:

- 1,000
- 5,000
- 10,000
- 20,000

In all benchmark cases, the code selects the middle position of the array.

### Main Observations

- Running time increases as input size increases for both algorithms.
- The randomized algorithm is faster in most cases.
- The deterministic algorithm is more stable across input distributions, but it has higher constant overhead.

Across the benchmark results, the randomized implementation is faster in almost all cases, while the deterministic implementation provides the stronger worst-case guarantee.

### Mean Timing by Size

- **Size 1000:** deterministic 0.370 ms, randomized 0.234 ms
- **Size 5000:** deterministic 1.916 ms, randomized 0.777 ms
- **Size 10000:** deterministic 6.986 ms, randomized 1.594 ms
- **Size 20000:** deterministic 14.568 ms, randomized 3.111 ms

### Mean Timing by Distribution

- **many_duplicates:** deterministic 6.620 ms, randomized 1.739 ms
- **random:** deterministic 4.907 ms, randomized 1.442 ms
- **reverse_sorted:** deterministic 6.320 ms, randomized 1.402 ms
- **sorted:** deterministic 5.993 ms, randomized 1.132 ms

## Part 2: Elementary Data Structures

### Implemented Structures

The following data structures were implemented from scratch in Python:

- Dynamic array (`ArrayList`)
- Matrix
- Array-based stack (`ArrayStack`)
- Array-based circular queue (`ArrayQueue`)
- Singly linked list (`SinglyLinkedList`)
- Optional rooted tree (`RootedTree`)

### Complexity of Operations

#### ArrayList
- `append` – amortized **O(1)**
- `insert` – **O(n)**
- `delete` – **O(n)**
- `get` / `set` – **O(1)**

#### Matrix
- `get` / `set` – **O(1)**
- `insert_row` – **O(c)** where `c` is the number of columns
- `delete_row` – up to **O(r)** where `r` is the number of rows

#### ArrayStack
- `push` – amortized **O(1)**
- `pop` – amortized **O(1)**
- `peek` – **O(1)**

#### ArrayQueue
- `enqueue` – amortized **O(1)**
- `dequeue` – amortized **O(1)**
- `front` – **O(1)**

#### SinglyLinkedList
- `insert_front` – **O(1)**
- `insert_end` – **O(n)**
- `delete_value` – **O(n)**
- `traverse` – **O(n)**

#### RootedTree
- `dfs` – **O(n)**

## Trade-Offs and Practical Applications

This assignment highlights the main differences between arrays and linked lists.

- Arrays provide fast indexing and good cache performance.
- Linked lists are useful when frequent insertion at the front is needed.
- Array-based stacks are simple and efficient for push/pop operations.
- Circular-array queues are practical because they avoid shifting elements during dequeue.
- Trees are useful for representing hierarchical relationships.

### Real-World Examples

- **Stacks:** undo systems, browser history, function calls
- **Queues:** scheduling, buffering, task processing
- **Dynamic arrays:** general-purpose storage where indexing matters
- **Linked lists:** applications with frequent front insertion
- **Rooted trees:** file systems, menus, organizational structures


### Colab Notebook

1. Open Google Colab.
2. Upload the notebook file `Assignment6_Colab.ipynb`.
3. Run the cells from top to bottom in order.
4. Make sure the cells that define the functions and classes are executed before running any demo or benchmark cells.
5. To generate the benchmark CSV file, run the cell that calls:

```python
run_selection_benchmarks()
