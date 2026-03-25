# Algorithm Design and Analysis (Spring 2026)

## P1
(15 points) Prove the following generalization of the master theorem. Given constants $a \geq 1 , b > 1 , d \geq 0$ , and $w \geq 0$ , if $T ( n ) = 1$ for $n < b$ and $T ( n ) = a T ( n / b ) + n ^ { d } \log ^ { w } n$ , we have

$$
T(n) = \left\{
\begin{array}{l l}
O(n^{d} \log^{w} n), & \text{if } a < b^{d}, \\
O(n^{\log_{b} a}), & \text{if } a > b^{d}, \\
O(n^{d} \log^{w+1} n), & \text{if } a = b^{d}.
\end{array}
\right.
$$

Prove by drawing a table: 
| level | num of problems | problem scale | level complexity |
| ----- | --------------- | ------------- | ---------------- |
| $0$ | $1$ | $n$ | $n^d\log^w{n}$ |
| $1$ | $a$ | $\frac{n}{b}$ | $a \cdot \left ( \frac{n}{b} \right )^d\log^w{\left( \frac{n}{b} \right)}$ |
| $2$ | $a^2$ | $\frac{n}{b^2}$ | $a^2 \cdot \left ( \frac{n}{b^2} \right )^d\log^w{\left( \frac{n}{b^2} \right)}$ |
| $k$ | $a^k$ | $\frac{n}{b^k}$ | $a^k \cdot \left ( \frac{n}{b^k} \right )^d\log^w{\left( \frac{n}{b^k} \right)}$ |
| $\dots$ |  |  |  |
| $\log_b{n}$ | $a^{\log_b{n}}$ | $1$ | $a^{\log_b{n}}$ |

The total running time is
$$n^d\log^w{n} + a \cdot \left ( \frac{n}{b} \right )^d\log^w{\left( \frac{n}{b} \right)} + \cdots + a^k \cdot \left ( \frac{n}{b^k} \right )^d\log^w{\left( \frac{n}{b^k} \right)} + \cdots + a^{\log_b{n}}$$

Simplification

$$n^d \cdot \left( \log^w{n}+\frac{a}{b^d}\log^w\left(\frac{n}{b}\right) + \cdots \left( \frac{a}{b^d}\right)^k\log^w\left(\frac{n}{b^k}\right) + \cdots + \left (\frac{a}{b^d}\right)^{\log_b{n}} \right)$$

Convert into sum expression 

$$
T(n) = n^d \cdot \left( \sum_{k=0}^{\log_{b} n} \left( \frac{a}{b^d} \right)^k \left( \log n - k \log b \right)^w \right) + n^{\log_{b} a}
$$

> Note: When $k = \log_{b} n$, $\left( \log \frac{n}{b^k} \right)^w = 0$, the leaf cost vanishes. But $T(1)$ is actually $1$. So we must add the last term $a^{\log_{b} n} = a^{\frac{\log_{a} n}{\log_{a} b}} = n^{\log_{b} a}$ separately.

- Case 1: $a < b^d$
  $$
  T(n) < n^d \log^w n \cdot \sum_{k=0}^{\log_{b} n} \left( \frac{a}{b^d} \right)^k + n^{\log_{b} a}
  $$

  Given $\frac{a}{b^d} < 1$, 

  $$
  \sum_{k=0}^{\log_{b} n} \left( \frac{a}{b^d} \right)^k 
  < \sum_{k=0}^{\infty} \left( \frac{a}{b^d} \right)^k = O(1)
  $$

  So

  $$
  T(n) = O(n^d \log^w n + n^{\log_{b} a})
  $$

  We also have $\log_{b} a < d$. So $n^{\log_{b} a} < n^d$. Thus

  $$
  T(n) = O(n^d \log^w n)
  $$
- Case 2: $a > b^d$:
  In this case $\tfrac{a}{b^d} > 1$. The last term dominates the total complexity. We set $j = \log_b {n}- k$

    $$\begin{align*} T(n) & = n^d \cdot \left( \sum_{k=0} ^{\log_b{n}} \left( \frac{a}{b^d}\right)^k \left( \log{n} - k \log{b}\right)^w\right)+n^{\log_b{a}} \\
    & = n^d \cdot \left( \sum_{j = 0} ^ {\log_b n} \left( \frac{a}{b^d}\right)^{\log_b{n}-j} \left( j\log{b} \right)^w \right) + n^{\log_b{a}}\\ 
    & = n^{\log_b{a}} \cdot \left( \sum_{j = 0} ^ {\log_b n} \left( \frac{a}{b^d}\right)^{-j} \left( j\log{b} \right)^w \right) + n^{\log_b{a}}\\
    & = n^{\log_b{a}} \cdot \left( \sum_{j = 0} ^ {\log_b n} \left( \frac{a}{b^d}\right)^{-j} \left( j\log{b} \right)^w + 1\right)
     \end{align*}$$ 
    Let's consider the summation term. For the sake of simplification, we denote $a/b^d$ as $r$, and ignore the constant coefficient $\log^w{b}$.
    $$\sum_{j = 0} ^ {\log_b n} \left( \frac{a}{b^d}\right)^{-j} \left( j\log{b} \right)^w \propto \sum_{j = 0} ^ {\log_b n} r^{-j} j^w < \sum_{j = 0} ^ {\infty} r^{-j} j^w$$
    Since $r <1$, the summation term consists of an exponential decay and a polynomial increase. We know the sum converges. i.e. $\exist C \in \mathbb{R}$, s.t. $\sum_{j = 0} ^ {\infty} r^{-j} j^w < C$. So the huge scaring summation factor is actually $O(1)$. We can now conclude that
    $$T(n) = O(n^{\log_b{a}})$$
- Case 3: $a = b^d$:
    $$\begin{align*}T(n) & = n^d \cdot  \sum_{k=0} ^{\log_b{n}} \left( \log{n} - k \log{b}\right)^w+n^d \\
    & < n^d \cdot \log_b {n} \cdot \log ^w {n} + n^d \\
    & = \frac{1}{\log b} n^d \log^{w+1} n + n^d \\
    & = O(n^d \log^{w+1} {n})
    \end{align*}$$ 


## P2
(25 points) Consider the randomized quicksort algorithm, where each pivot is chosen uniformly at random. Analyze its expected running time.

a. (10 points) Prove that the expected running time is $O ( n ^ { c } )$ for some constant $c < 2$ , using a method similar to the one presented in lecture. (You may choose any constant $c < 2$ .) In particular, define a suitable “lucky” event, and analyze the lucky and unlucky cases separately.

Proof: We call a pivot $p$ *lucky* if $p \in [\tfrac{n}{3} , \tfrac{2n}{3}]$. If $p$ is not lucky, then it is *unlucky*.

The recursive form of the compexity of quicksort is

$$T(n) = T(L) + T(R) + O(n)$$

Where $L + R = n-1$.

- Case 1: The pivot is lucky
  $$\max{(L, R)} \leq \frac{2n}{3}$$
  In this case, 
  $$T(n) \leq 2T \left( \frac{2n}{3} \right) + O(n)$$

- Case 2: The pivot is unlucky

  We consider the worst case

  $$T(n) \leq T(n-1) + O(n)$$

We assume the pivot $p$ is uniformly distributed. So $P(p \text{ is lucky}) = \frac{1}{3}$. Let $\tau (n)$ be the total cost we take until the first lucky pivot occurs. Since we are lucky with the probability $\tfrac{1}{3}$, the depth of our recursion before we get a lucky pivot follows a geometric distribution with expectation 3. So $E(\tau(n)) = O(n)$

$$T(n) = \tau(n) + 2T(\tfrac{2n}{3})$$

We take expectation from both side

$$E(T(n)) = 2E(T(\tfrac{2n}{3})) + O(n)$$

Refer to the master theorem, $E(T(n)) = O\left(n^{\log_{\tfrac{3}{2}}{2}}\right) = O(n^{1.71})$. We conclude 

$$E(T(n)) = O(n^c) \text{,}\quad c = 1.71 < 2$$

b. (15 points) Prove that the expected running time is $O ( n \log n )$ . Hint: Let $x _ { i }$ be the $i$ -th smallest element and $x _ { j }$ the $j$ -th smallest element, where $j > i$ . Show that $x _ { i }$ and $x _ { j }$ are compared in quicksort if and only if either $x _ { i }$ or $x _ { j }$ is the first pivot chosen among the elements $x _ { i } , x _ { i + 1 } , \dotsc , x _ { j }$ . What is the probability of this event?

Proof:
Let $x_i$ and $x_j$ be the $i$-th and $j$-th smallest element relatively $(i<j)$. Define indicator variable:
$$X_{ij} = \begin{cases}
    1 \quad \text{if } x_i \text{ and } x_j \text{ are compared} \\
    0 \quad \text{otherwise}
\end{cases}$$
Then the total comparisons:
$$X = \sum_{i<j} X_{ij}$$

The expectation:
$$E(X) = \sum_{i<j} E(X_ij) = \sum_{i<j} P(x_i \text{ and } x_j \text{ are compared})$$
We claim that $x_j$ and $x_j$ are compared in quicksort iff either $x_i$ or $x_j$ is the first pivot chosen among $x_i, x_{i+1}, \cdots , x_j$. In fact, if some $x_k \, (i<k<j)$ is chosen first as a pivot, then $x_i$ and $x_j$ are divided into different sub questions, and are never compared. On the other hand, if $x_i$ or $x_j$ is chosen as a pivot, then $x_i$ and $x_j$ must be compared.

Since the pivot is chosen uniformly:
$$P(x_i \text{ and } x_j \text{ are compared} ) = \frac{2}{j-i+1}$$

Thus
$$E(X) = \sum_{i<j} \frac{2}{j-i+1}$$

Let $k = j-i$, then
$$E(X) = \sum_{k=1}^{n-1}(n-k) \cdot \frac{2}{k+1} \leq \sum_{k=1}^n \frac{2}{k+1}\cdot n = O(n\log{n})$$

Therefore:
$$E(T(n)) = O(n\log{n})$$

## P3

(30 points) Given an $n \times m$ 2-dimensional integer array $A | 0 , \ldots , n - 1 ; 0 , \ldots , m - 1 |$ where $A [ i , j ]$ denotes the cell at the $i$ -th row and the $j$ -th column, a local minimum is a cell $A [ i , j ]$ such that $A [ i , j ]$ is smaller than each of its four adjacent cells $A [ i - 1 , j ] , A [ i +$ $1 , j ] , A [ i , j - 1 ] , A [ i , j + 1 ]$ . Notice that $A [ i , j ]$ only has three adjacent cells if it is on the boundary, and it only has two adjacent cells if it is at the corner. Assume all the cells have distinct values. Your objective is to find one local minimum (i.e., you do not have to find all of them).

 a. (10 points) Suppose $m = 1$ so $A$ is a 1-dimensional array. Design a divide-andconquer-based algorithm for the problem above. Write a recurrence relation of the algorithm, and analyze its running time.

  **Algorithm**

  First check the position `mid = (left+right)/2` and its neighbors.
  - case 1: `mid` is a local minimum
    if $A[\text{mid}] < A[\text{mid}-1] \text{ 且 } A[\text{mid}] < A[\text{mid}+1]$ then return mid
  - case 2: The left neighbor is smaller
    if $A[\text{mid}-1] < A[\text{mid}]$, there exists a local min in the left half of the array. So recursively search left half of the array.
  - case 3: The right neighbor is smaller
    Symmetrical to case 2, recursively search the right hand of the array. 
    
  **Pseudo code**:
  ```python
  def LocalMin(A, left, right):
    """
    Args:
        A: the array to find a local minimum
        left: the left bound of searching interval
        right: the right bound of searching interval 
    Returns:
        a local minimum in A[left: right]
    """
    if (left == right):
        return left

    mid = (left + right) // 2

    # handle boundary situation
    left_val = A[mid-1] if mid > left else INF
    right_val = A[mid+1] if mid < right else INF
    
    if (A[mid] < left_val and A[mid] < right_val):
        return mid
    elif (left_val < A[mid]):
        return LocalMin(A, left, mid-1)
    else:
        return LocalMin(A, mid+1, right)
  ```
  **Validity analysis**

  We need to show that if $A[\text{mid}-1] < A[\text{mid}]$, then there exists a local minimum $A[k]$ s.t. $\text{left} \leq k \leq \text{mid}-1$. 

  If the subsequence is strictly increasing, we claim $A[\text{left}]$ is a local minimum. In fact, if $\text{left} = 0$, in other word $A[\text{left}]$ is in the on the boundary, by definition it is a local minimum. If $\text{left} \neq 0$, by recursive hypothesis, $A[\text{left}] < A[\text{left}-1]$. The increase of the sequence implies that $A[\text{left}] < A[\text{left}+1]$. So $A[\text{left}]$ is a local minimum.

  If the subsequence is not strictly increasing, then there exists an index $k$ such that $A[k] < A[k-1]$ and $A[k] < A[k+1]$. In particular, when the subsequence is strictly decreasing, $k = \text{mid}-1$.

  Hence, a local minimum must exist in the left half.

  **Complexity analysis**

  $$T(n) = T(\tfrac{n}{2}) + O(1)$$

  By master theorem
  $$T(n) = O(\log{n})$$
  
---

  b. (10 points) Suppose $m = n$ . Design a divide-and-conquer-based algorithm for the problem above. Write a recurrence relation of the algorithm, and analyze its running time. 

  **Algorithm**
  
  We implement a divide-and-conquer approach by performing a binary search on the rows.

  1. Pick the middle row `mid_row = (upper + lower) // 2`.
  2. Find the global minimum of this row. Let the minimum be at $A[\text{mid\_row}][i]$, where $0\leq i \leq n-1$.
  3. Compare $A[\text{mid\_row}][i]$ with its vertical neighbors $A[\text{mid\_row}-1][i]$ and $A[\text{mid\_row}+1][i]$ (if exist)
      - case 1: $A[\text{mid\_row}][i]$ is smaller than their neighbors, then $A[\text{mid\_row}][i]$ is already a local minimum. Return it.
      - case 2: $A[\text{mid\_row}-1][i] < A[\text{mid\_row}][i]$, there exist a local minimum in the upper half of the matrix. Recursively search the upper half sub-matrix.
      - case 3: $A[\text{mid\_row}+1][i] < A[\text{mid\_row}][i]$, symmetric to case 2, recursively search the lower half sub-matrix.
    
**Pseudo code**
  ```python
  def LocalMin2D(A, upper, lower):
    """
    Args:
        A: The n * n 2D array
        upper, lower: The row boundaries for the current search.
    Returns:
        The coordinates (i, j) of a local minimum
    """
    if (upper == lower):
        # obtain the column num of the minimum in this row
        min_column = argmin(k, A[upper][k]) # O(n) complexity
        return (upper, min_column)
    
    mid_row = (upper + lower) // 2

    min_column = argmin(k, A[mid_row][k]) # O(n) complexity
    current_val = A[mid_row][min_column]
    # handle boundary situations
    upper_val = A[mid_row-1][min_column] if mid_row > upper else INF
    lower_val = A[mid_row+1][min_column] if mid_row < lower else INF

    if (current_val < upper_val and current_val < lower_val):
        return (mid_row, min_column)
    elif (upper_val < current_val):
        return LocalMin2D(A, upper, mid_row-1)
    else:
        return LocalMin2D(A, mid_row+1, lower)
  ```

  **Validity analysis**

  If the recursion returns **before** reach the base case, where $\text{upper} = \text{lower}$, it is trival that the return value is a local minimum.

  If the recursion reaches the base case, we need to check whether the return value is smaller than its vertical neighbors.

  In fact, when we reach the base case, by recursion hypothesis, the value of some column in the row is smaller than the minimum of its two neighbor rows (if exists). That's why we come to this specific row. Further more, the value of the returned coordinate is even smaller than (or equal to) that of the "some column" described before. So the value of the returned coordinate is smaller than its vertical neighbors.

  The above is my explaination, which is not necessarily formal and accurate. Here I paste a graph of Chat-GPT's insight:

  > At each step, we select the minimum element in the middle row. Therefore, this element is smaller than its horizontal neighbors.
  >
  > If it is also smaller than its vertical neighbors, then it is a local minimum.
  >
  > Otherwise, suppose the upper neighbor is smaller. Then we claim that there exists a local minimum in the upper half.
  >
  > Starting from this smaller neighbor, we can follow a path of strictly decreasing values. Since all values are distinct and the matrix is finite, this path must terminate at a cell that is smaller than all of its neighbors, i.e., a local minimum.
  >
  > Hence, the recursion is correct.

  **Complexity analysis**

  The complexity of an $n\times m$ grid is $T(n, m)$ then
  $$T(n, m) = T(n/2, m) + O(m)$$
  The $O(n)$ complexity comes from the `argmin` function. 
  There are in total $\log {n}$ levels of recursion before we reach the base case. Each level contributes an $O(m)$ complexity. Thus
  
  $$T(n, m) = O(m\log{n})$$

  In this question, $m = n$, so 
  $$T(n) = O(n\log{n})$$

---

  c. (10 points) Generalize your algorithm such that it works for general $m$ and $n$ . The running time of your algorithm should smoothly interpolate between the running times for the first two parts.

  We can naturally generalize the algorithm in (b). But consider the different rate at which $m$ and $n$ influence the total running time, we implement a trick here:

  **Algorithm**

  If $m>n$, run $\operatorname{LocalMin2D}(A^T)$. 

  Otherwise run $\operatorname{LocalMin2D}(A)$.

  **Complexity**

  $$T(n, m) = O(\min{(m, n)}\log{(\max{(m, n)})})$$

## P4

(30 points) We are given a $n$ -vertex tournament graph ${ \vec { G } } = ( V , A )$ , which is a directed version of a complete undirected graph $G = ( V , E )$ . For each undirected edge $e =$ $( u , v ) \in E$ , we have a corresponding directed arc ( $u  v$ or $v  u$ ) in $A$ . We aim to design an efficient algorithm to find a directed Hamiltonian path in $\vec { G }$ , which is a path including all vertices in $V$ , such as $u _ { 1 } \to u _ { 2 } \to u _ { 3 } \dots \to u _ { n }$ .

- a. (15 points) Assume we have a path $P$ with length $k$ , and another vertex $u$ not included in $P$ . Prove that we can always find a feasible location in $P$ to insert $u$ . (E.g., when $P = v _ { 1 }  v _ { 2 }  v _ { 3 }$ , we can insert $u$ into $P$ and change $P$ to $v _ { 1 }  u  v _ { 2 }  v _ { 3 }$ if $v _ { 1 }  u$ and $u \to v _ { 2 }$ are both in $A$ .) Design an $O ( k )$ algorithm to find a feasible location to insert $u$ into $P$ , analyze the running time, and prove the correctness.   
- b. (15 points) Use divide and conquer to find a $O ( n \log n )$ algorithm to find a directed Hamiltonian path, analyze the running time, and prove the correctness.

--- 

 How long does it take you to finish the assignment (including thinking and discussion)? Give a score (1,2,3,4,5) to the difficulty. Do you have any collaborators? Please write down their names here.

 | problem | difficulty | comment |
 | ------- | ---------- | ------- |
 | 1 | 16384 | 初见端倪 |
 | 2 | 32768 | AI 汗流浃背 |
 | 3 | 65536 | 666演都不演了 |