# PageRank from Scratch — Eigendecomposition & Power Iteration

**Tech Stack:** NumPy · NetworkX · Matplotlib  

## Overview

The web is a directed graph. **PageRank** answers a fundamental question: *"if you surfed the web randomly forever, how often would you land on each page?"*

This project implements PageRank from first principles—building the theory, handling real-world edge cases, and solving using two independent methods: **power iteration** and **eigendecomposition**. We verify both methods produce identical results and explain *why* they mathematically must.

The stationary distribution of a random walk on the web graph is the **dominant eigenvector** of a column-stochastic transition matrix. The damping factor ensures convergence through the Perron-Frobenius theorem.

---

## Implementation Summary

### 1. **Graph Construction**
- Build directed graph from adjacency lists
- Convert to column-stochastic transition matrix where each column sums to 1
- Handle **dangling nodes** (pages with no outgoing links) — the silent bug that breaks naive implementations
- Returns normalized adjacency matrix ready for Markov chain analysis

### 2. **The Transition Matrix**
- **Column-stochastic property:** `M[i][j]` = probability of transitioning to page i from page j
- **Dangling node handling:** Pages with zero out-degree create zero-sum columns. Solution: distribute probability 1/n uniformly across all nodes
- **Google Matrix (with damping factor d ≈ 0.85):**
  ```
  G = d·M̃ + (1-d)·(1/n)·𝟙𝟙ᵀ
  ```
  - With probability d: follow a real link
  - With probability (1-d): teleport randomly to any page
  - Ensures every entry is strictly positive → guarantees convergence

### 3. **Dual-Method Solver**

#### Power Iteration
- Iteratively apply the matrix: `r ← G·r`
- Repeated multiplication converges to the dominant eigenvector
- Fast in practice, especially for sparse matrices
- Tracks convergence curve over iterations

#### Eigendecomposition  
- Compute eigenvalues and eigenvectors of G
- Extract eigenvector corresponding to eigenvalue λ = 1
- Theoretically guaranteed: G always has eigenvalue 1 with unique positive eigenvector
- Provides exact solution in single computation

### 4. **Verification & Analysis**
- Compare results from both methods — verify numerical equivalence
- Plot convergence behavior of power iteration over iterations
- Rank nodes by PageRank score
- Statistical analysis of rank distribution

### 5. **Visualization**
- Render directed graph with NetworkX and Matplotlib
- **Node size** proportional to PageRank score
- **Color gradient** to highlight high-ranking pages
- **Edge arrows** show link direction
- **Convergence plot** showing rank stabilization over iterations

---

## Key Challenges Solved

1. **Dangling Nodes** — Pages with no outgoing links cause columns to sum to zero, breaking the stochastic matrix. Solution: redistribute their "rank budget" uniformly across all nodes.

2. **Damping Factor Justification** — It's not a hack. The damping factor guarantees the transition matrix is irreducible (every node can reach every other node) and aperiodic, invoking the Perron-Frobenius theorem which guarantees:
   - Unique eigenvalue λ₁ = 1
   - Unique positive eigenvector (normalized)
   - Convergence from any starting distribution

3. **Method Equivalence** — Prove and verify that power iteration and eigendecomposition yield the same PageRank scores despite using different algorithms.

---

## Mathematical Foundation

### Markov Chain Perspective
The directed graph defines a random walk. The **transition matrix** M encodes probabilities:
- From any page j, jump to each of its neighbors with equal probability
- Dangling pages teleport with uniform probability

### Stationary Distribution
The PageRank vector **r** is the **stationary distribution** — the unique probability distribution that satisfies:
$$\mathbf{r} = G \cdot \mathbf{r}$$

This is equivalent to finding the eigenvector of G with eigenvalue λ = 1.

### Perron-Frobenius Theorem
For a column-stochastic matrix G with strictly positive entries (achieved via damping), the theorem guarantees:
- Single dominant eigenvalue λ₁ = 1
- Unique positive eigenvector (PageRank)
- Power iteration converges from any initial distribution

---

## Algorithm Complexity

| Method | Time | Space | Notes |
|--------|------|-------|-------|
| Power iteration | O(kn + m) | O(n²) | k iterations, n nodes, m edges |
| Eigendecomposition | O(n³) | O(n²) | Full matrix eigenvalue decomposition |

For large, sparse graphs: **power iteration is preferred**.

