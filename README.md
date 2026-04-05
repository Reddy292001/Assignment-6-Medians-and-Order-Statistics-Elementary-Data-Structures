# Assignment 6 Report

## 1. Part 1: Selection algorithms

### 1.1 Deterministic selection

The deterministic method is implemented as `deterministic_select` with a recursive helper. The code breaks the input into groups of five, sorts each small group to obtain its median, recursively selects the median of the medians, and then applies a three-way partition around that pivot. The implementation uses 1-based indexing for the public API and converts to 0-based indexing internally. Duplicate values are handled explicitly through the `less`, `equal`, and `greater` partitions.

### 1.2 Randomized selection

The randomized method is implemented as `randomized_select`, which uses a randomized pivot selection strategy similar to Quickselect. The notebook comments state that the method has expected linear running time in practice but does not guarantee the same worst-case bound as the deterministic version.

### 1.3 Correctness and edge-case handling

The notebook includes a shared input-validation helper to reject invalid arrays and out-of-range values of `k`. It also uses three-way partitioning so arrays with duplicate values are handled cleanly. In addition, the notebook contains several small correctness checks, including single-element inputs and duplicate-heavy inputs.

## 2. Part 1 performance analysis

### 2.1 Time complexity

Based on the implementation structure, the deterministic algorithm is designed to achieve worst-case $O(n)$ time by choosing a pivot through the Median of Medians method. The recursive structure and grouping-by-five strategy are consistent with that guarantee. The randomized algorithm is designed for expected $O(n)$ time because each partition step is linear and the pivot is chosen randomly, but its worst case remains $O(n^2)$ if unlucky pivots are selected repeatedly.

### 2.2 Space complexity and overhead

Both selection implementations make copies of the input sequence before processing. They also allocate fresh `less`, `equal`, and `greater` lists during partitioning. This means the implementations are not in-place. The deterministic version has more overhead because it must create groups of five, store medians, and recurse on the median list before partitioning the original data. The randomized version has less pivot-selection overhead, which helps explain why it is usually faster in the benchmark results.

## 3. Part 1 empirical analysis

The benchmark CSV records results for input sizes 1,000, 5,000, 10,000, and 20,000 under four distributions: random, sorted, reverse-sorted, and many duplicates. In all cases, the code selects the median position (`k = n/2`).

Across the 16 benchmark cases, the randomized method is faster in 15 cases, while the deterministic method is faster in 1 case. On average, the deterministic implementation takes about 3.33 times as long as the randomized implementation in this dataset.

The benchmark results show three main trends:

1. Running time grows as the input size grows for both algorithms.

2. The randomized implementation is usually faster in practice, which matches the lower overhead of randomized pivot selection.

3. The deterministic method remains stable across different distributions, but its constant-factor overhead is visible, especially on larger inputs.

One notable exception appears at size 1,000 with many duplicates, where the deterministic method is slightly faster than the randomized method. Even so, the broader pattern in the CSV favors the randomized implementation for runtime performance.

### 3.1 Mean timing by size (milliseconds)

- Size 1000: deterministic 0.370 ms, randomized 0.234 ms, ratio 1.65x.

- Size 5000: deterministic 1.916 ms, randomized 0.777 ms, ratio 2.43x.

- Size 10000: deterministic 6.986 ms, randomized 1.594 ms, ratio 4.38x.

- Size 20000: deterministic 14.568 ms, randomized 3.111 ms, ratio 4.87x.

### 3.2 Mean timing by distribution (milliseconds)

- many_duplicates: deterministic 6.620 ms, randomized 1.739 ms, ratio 2.56x.

- random: deterministic 4.907 ms, randomized 1.442 ms, ratio 2.98x.

- reverse_sorted: deterministic 6.320 ms, randomized 1.402 ms, ratio 4.07x.

- sorted: deterministic 5.993 ms, randomized 1.132 ms, ratio 3.71x.

## 4. Part 2: Elementary data structures

### 4.1 Implemented structures

The notebook includes the following data structures required by the assignment: a dynamic array (`ArrayList`), a matrix stored as a list of lists, an array-based stack (`ArrayStack`), an array-based circular queue (`ArrayQueue`), and a singly linked list (`SinglyLinkedList`). It also includes an optional rooted tree (`RootedTree`) represented with node objects and child lists.

### 4.2 Complexity of the implemented operations

- **ArrayList:** `append` is amortized $O(1)$ because resizing happens only occasionally; `insert` and `delete` are $O(n)$ due to shifting elements; `get` and `set` are $O(1)$.

- **Matrix:** `get` and `set` are $O(1)$; `insert_row` is $O(c)$ because the inserted row is copied; `delete_row` is $O(r)$ in the worst case because the outer list may shift remaining row references.

- **ArrayStack:** `push` and `pop` are amortized $O(1)$; `peek` is $O(1)$.

- **ArrayQueue:** the queue uses circular indexing to avoid shifting on dequeue, so `enqueue` and `dequeue` are amortized $O(1)$, while `front` is $O(1)$. Resizing still costs $O(n)$ when it occurs.

- **SinglyLinkedList:** `insert_front` is $O(1)$; `insert_end` is $O(n)$ because there is no tail pointer; `delete_value` is $O(n)$ in the worst case; `traverse` is $O(n)$.

- **RootedTree:** depth-first traversal visits each node once, so `dfs` is $O(n)$.

### 4.3 Trade-offs and practical applications

The notebook reflects the main trade-offs the assignment asks to discuss. Arrays provide fast indexing and good cache locality, which is why the `ArrayList`, stack, and circular queue are efficient for direct access and amortized constant-time end operations. Linked lists avoid contiguous resizing and allow constant-time insertion at the front, but they use extra pointer storage and do not support constant-time random access. The circular-array queue is a strong practical choice because it avoids the cost of shifting elements after every dequeue.

In real-world terms, stacks are natural for undo systems and function-call management, queues are useful for scheduling and buffering, dynamic arrays are common for general-purpose storage when indexing matters, linked lists are useful when front insertion is frequent, and rooted trees are a natural fit for hierarchies such as folders, menus, or parsed document structures.

## 5. Conclusion

Based on the notebook and the benchmark CSV, the submission addresses the main assignment requirements well. Part 1 includes both required order-statistics algorithms and empirical timing results across multiple input sizes and distributions. Part 2 implements the core required data structures and includes the optional rooted tree as an extension.

The main result from the benchmark data is that the randomized selection implementation is usually faster in practice for the tested inputs, while the deterministic method offers the stronger worst-case guarantee with higher constant overhead.
