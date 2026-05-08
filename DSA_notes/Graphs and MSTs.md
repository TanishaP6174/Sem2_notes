# Graph Theory & MST — Complete Notes

---

## 1. What is a Graph?

A **graph** $G = (V, E)$ consists of a set of vertices $V$ and edges $E \subseteq V \times V$.

- **Undirected**: $(u,v) \equiv (v,u)$
- **Directed (Digraph)**: $(u,v) \neq (v,u)$; edge goes from $u$ to $v$
- $n = |V|$, $m = |E|$
- A **simple graph** has no self-loops and no multi-edges
- **Degree** of $v$: number of edges incident to $v$. For directed graphs: **in-degree** and **out-degree**
- $\sum_{v} \deg(v) = 2m$ (handshaking lemma)

---

## 2. Graph Storage

### 2.1 Adjacency Matrix — $O(n^2)$ space

$A[i][j] = 1$ if edge $(i,j) \in E$, else $0$. For undirected graphs, $A$ is symmetric.

**Puzzle insight**: If $A_{TR} = 0$ and $A_{BL} = 0$ (top-right and bottom-left blocks are zero when vertices are split into two halves), there are no edges between the two halves $\Rightarrow$ **G is disconnected**. If $A_{TR} = 0$ and $A_{BR} = 0$, the right-half vertices have no edges to any vertex outside themselves, and may be isolated. Check what structural property zeroing blocks implies about connectivity.

- Edge query: $O(1)$
- List all neighbors: $O(n)$

### 2.2 Incidence Matrix — $O(mn)$ space

$B[i][j] = 1$ if vertex $i$ is incident to edge $j$. Rarely used in algorithms.

### 2.3 Adjacency List — $O(m + n)$ space

Each vertex stores a list of its neighbors. Standard representation for sparse graphs.

- Edge query: $O(\deg(v))$
- List all neighbors: $O(\deg(v))$

---

## 3. Breadth-First Search (BFS)

**Goal**: Explore all vertices reachable from source $s$, level by level.

**Algorithm**:

```
BFS(G, s):
  color[s] = GRAY, d[s] = 0, π[s] = NIL
  Q = {s}
  while Q not empty:
    u = dequeue(Q)
    for each v in Adj[u]:
      if color[v] == WHITE:
        color[v] = GRAY, d[v] = d[u]+1, π[v] = u
        enqueue(Q, v)
    color[u] = BLACK
```

**Complexity**: $O(n + m)$

**Key properties**:

- $d[v]$ = shortest path distance (in terms of edges) from $s$ to $v$
- BFS tree: edges ${(\pi[v], v)}$ form a tree rooted at $s$
- For unweighted graphs, BFS gives single-source shortest paths

**Correctness (formal)**: We prove $d[v] = \delta(s,v)$ for all reachable $v$ by induction on $\delta(s,v)$.

Base: $d[s] = 0 = \delta(s,s)$. Inductive step: suppose all vertices at distance $< k$ have correct $d$. Let $v$ be at distance $k$, with predecessor $u$ on a shortest path ($\delta(s,u) = k-1$). By inductive hypothesis, $d[u] = k-1$ when $u$ is dequeued. At that point, if $v$ is still WHITE, we set $d[v] = d[u]+1 = k$. If $v$ was already colored, it was reached via some path of length $\leq k$, so $d[v] \leq k = \delta(s,v)$. Since BFS never underestimates, $d[v] = \delta(s,v)$. $\square$

**Queue invariant**: The queue at any moment contains vertices at distance $k$ followed by vertices at distance $k+1$ for some $k$. This means BFS processes vertices in non-decreasing order of distance, which is why it finds shortest paths.

---

## 4. Depth-First Search (DFS)

**Algorithm**:

```
DFS(G):
  for each u in V: color[u] = WHITE, π[u] = NIL
  time = 0
  for each u in V:
    if color[u] == WHITE: DFS-VISIT(u)

DFS-VISIT(u):
  color[u] = GRAY, d[u] = ++time
  for each v in Adj[u]:
    if color[v] == WHITE:
      π[v] = u
      DFS-VISIT(v)
  color[u] = BLACK, f[u] = ++time
```

**Complexity**: $O(n + m)$

**Discovery time $d[u]$, finish time $f[u]$**.

**Edge classification** (for directed graphs):

- **Tree edge**: $v$ is WHITE when $(u,v)$ is explored — $v$ is a descendant of $u$
- **Back edge**: $v$ is GRAY — $v$ is an ancestor of $u$; **indicates a cycle**
- **Forward edge**: $v$ is BLACK and $d[u] < d[v]$ — $v$ is a descendant already fully finished
- **Cross edge**: $v$ is BLACK and $d[u] > d[v]$ — $v$ is in a completely separate branch, finished before $u$ was even discovered

**Why forward/cross edges have these finish-time properties**: By the parenthesis theorem, if $v$ is a descendant of $u$, then $[d[v], f[v]] \subset [d[u], f[u]]$, so $d[u] < d[v]$ and $f[v] < f[u]$. For a cross edge, $v$'s interval is entirely before $u$'s, so $f[v] < d[u]$, meaning $d[u] > d[v]$ and $f[v] < f[u]$. You can always determine edge type from discovery/finish times alone.

For **undirected** graphs, only tree edges and back edges exist. Why? When exploring $(u,v)$ in an undirected graph, if $v$ is BLACK then $(v,u)$ was already explored as a tree edge when $v$ was processed, so from $u$'s perspective it's a back edge (to a predecessor), never a forward or cross edge.

**Parenthesis theorem**: For any two vertices $u, v$, exactly one holds:

- $[d[u], f[u]]$ and $[d[v], f[v]]$ are disjoint — neither is an ancestor of the other
- One interval is entirely within the other — one is an ancestor of the other

**Cycle detection summary**:

- Undirected graph: back edge found during DFS $\Rightarrow$ cycle exists
- Directed graph: GRAY vertex encountered during DFS $\Rightarrow$ cycle exists (back edge to an ancestor still on the stack)

---

## 5. Directed Acyclic Graphs (DAGs)

A **DAG** is a directed graph with **no directed cycles**.

**Theorem**: A directed graph is a DAG $\iff$ DFS produces no back edges.

### 5.1 Topological Sort

**Definition**: A linear ordering of all vertices such that for every edge $(u,v)$, $u$ appears before $v$.

**Algorithm (DFS-based)**:

```
TopSort(G):
  run DFS, recording finish times
  output vertices in decreasing order of finish time
```

**Correctness**: When edge $(u,v)$ is in $G$ (a DAG), $f[v] < f[u]$ always. So sorting by decreasing finish time respects all edges.

**Proof**: Consider any edge $(u,v)$. When we call DFS-VISIT$(u)$, $v$ is either WHITE or BLACK (never GRAY, since no back edges in DAG). If WHITE, $v$ finishes before $u$ so $f[v] < f[u]$. If BLACK, $v$ already finished so $f[v] < f[u]$. $\square$

**Alternative (Kahn's algorithm)**: Repeatedly remove vertices with in-degree 0. $O(n+m)$.

---

## 6. Strongly Connected Components (SCC)

**Definition**: A **strongly connected component** of a directed graph is a maximal set $S \subseteq V$ such that for every $u, v \in S$, there is a path from $u$ to $v$ and from $v$ to $u$.

### 6.1 Kosaraju's Algorithm

1. Run DFS on $G$, record finish times
2. Compute $G^T$ (reverse all edges)
3. Run DFS on $G^T$ in decreasing order of finish times from step 1
4. Each DFS tree in step 3 is one SCC

**Complexity**: $O(n + m)$

**Key lemma**: If $C$ and $C'$ are two SCCs and there is an edge from $C$ to $C'$, then $\max_{v \in C} f[v] > \max_{v \in C'} f[v]$ in the original DFS.

**Proof of key lemma**: Let $u$ be the first vertex discovered in $C \cup C'$ during the original DFS. Case 1: $u \in C$. DFS from $u$ can reach all of $C$ (by SCC definition) and all of $C'$ (via the inter-SCC edge), so all vertices in $C'$ finish before $u$ does, giving $f[u] > f[v]$ for all $v \in C'$. Case 2: $u \in C'$. DFS from $u$ can reach all of $C'$ but cannot reach $C$ (there's no edge from $C'$ back to $C$, otherwise $C$ and $C'$ would be the same SCC). So all of $C'$ finishes before any vertex in $C$ is discovered, meaning $\max f[C] > \max f[C']$. $\square$

**Why step 3 works**: In $G^T$, the inter-SCC edges are reversed, so edges go from $C'$ to $C$. When we process in decreasing finish time order, we start DFS from the vertex with the highest finish time, which is in the "source" SCC $C$. On $G^T$, we cannot escape $C$ (there's no edge from $C$ to $C'$ in $G^T$), so the DFS tree is exactly $C$. Then we pick the next unvisited vertex (highest finish time in remaining SCCs) and repeat. $\square$

**Component graph**: Contract each SCC to a single node. The result is always a DAG.

---

## 7. Single-Source Shortest Path (SSSP)

Given graph $G = (V,E,w)$ and source $s$, find shortest paths from $s$ to all $v \in V$.

**Shortest path weight**: $\delta(s,v) = \min_{\text{paths } p: s\to v} w(p)$, $\infty$ if no path.

**Relaxation**: For edge $(u,v)$:

```
Relax(u, v, w):
  if d[v] > d[u] + w(u,v):
    d[v] = d[u] + w(u,v)
    π[v] = u
```

All SSSP algorithms are instances of repeated relaxation.

### 7.1 Triangle Inequality

**Theorem**: For any edge $(u,v) \in E$: $\delta(s,v) \leq \delta(s,u) + w(u,v)$.

**Proof**: Any path to $u$ extended by $(u,v)$ is a valid path to $v$, so the shortest path to $v$ is at most this length. $\square$

This is the structural invariant that relaxation exploits.

### 7.2 Bellman-Ford — $O(nm)$

Handles **negative edge weights**. Detects **negative cycles**.

```
Bellman-Ford(G, s):
  d[s] = 0, d[v] = ∞ for v ≠ s
  repeat (n-1) times:
    for each edge (u,v):
      Relax(u, v, w)
  for each edge (u,v):
    if d[v] > d[u] + w(u,v): return "negative cycle"
```

**Correctness (inductive proof)**: Let $d^{(k)}[v]$ denote $d[v]$ after $k$ iterations of the outer loop.

**Claim**: After $k$ iterations, $d^{(k)}[v] \leq$ weight of the shortest path from $s$ to $v$ using at most $k$ edges.

**Proof by induction on $k$**: Base ($k=0$): $d[s]=0$, $d[v]=\infty$ for $v \neq s$. Correct since a 0-edge path only exists from $s$ to $s$. Inductive step: assume claim holds for $k-1$. In iteration $k$, for each edge $(u,v)$ we relax: $d[v] = \min(d[v],\ d[u] + w(u,v))$. By inductive hypothesis, $d^{(k-1)}[u]$ is at most the shortest path to $u$ using $\leq k-1$ edges. So after relaxing, $d^{(k)}[v] \leq d^{(k-1)}[u] + w(u,v) \leq$ shortest path to $v$ using $\leq k$ edges. $\square$

Since any shortest path (with no negative cycle) has $\leq n-1$ edges, after $n-1$ iterations all distances are correct.

**Negative cycle detection**: If after $n-1$ iterations some $d[v] > d[u] + w(u,v)$, then $d[v]$ can still be reduced, meaning there is a path with $\geq n$ edges that is shorter — only possible if the path goes through a negative cycle.

### 7.3 Dijkstra's Algorithm — $O((n+m)\log n)$ with binary heap

Works only for **non-negative edge weights**.

```
Dijkstra(G, s):
  d[s] = 0, d[v] = ∞ for v ≠ s
  Q = min-priority queue of all vertices keyed by d
  while Q not empty:
    u = Extract-Min(Q)
    for each v in Adj[u]:
      Relax(u, v, w)  // decrease-key in Q if updated
```

**Correctness**: When $u$ is extracted, $d[u] = \delta(s,u)$.

**Proof (by induction)**: At extraction of $u$, assume all previously extracted vertices $x$ have $d[x] = \delta(s,x)$. Any other path to $u$ goes through some vertex $y$ still in $Q$, so $d[y] \geq d[u]$ (since $d[u]$ was minimum), and the path through $y$ has weight $\geq d[y] \geq d[u]$. (Requires non-negative weights to ensure path can't get shorter by going through $Q$.) $\square$

**Complexity**: $O((n+m)\log n)$ with binary heap; $O(n^2)$ with array.

### 7.4 SSSP on DAGs — $O(n + m)$

Relax edges in **topological order**. Since the graph is a DAG, when we process vertex $u$ (in topological order), all vertices that could be predecessors of $u$ on a shortest path have already been processed and their $d$ values finalized. So when we relax all edges out of $u$, we're guaranteed to have the correct $d[u]$. No vertex ever needs to be revisited. Works with negative edges (just no negative cycles, which DAGs can't have anyway).

---

## 8. All-Pairs Shortest Path (APSP)

Find $\delta(u,v)$ for all pairs $u,v$.

### 8.1 Floyd-Warshall — $O(n^3)$

$dp[i][j][k]$ = shortest path from $i$ to $j$ using only vertices ${1,\ldots,k}$ as intermediates.

$$dp[i][j][k] = \min(dp[i][j][k-1],\ dp[i][k][k-1] + dp[k][j][k-1])$$

**Why this subproblem is correct**: Consider the true shortest path $p$ from $i$ to $j$ through intermediates $\subseteq {1,\ldots,k}$. Either $k$ is not on $p$ (then $dp[i][j][k] = dp[i][j][k-1]$), or $k$ is on $p$ exactly once (shortest paths don't repeat vertices without negative cycles). In the latter case, $p$ splits into $i \to k$ and $k \to j$, each using intermediates $\subseteq {1,\ldots,k-1}$. The recurrence captures both cases. $\square$

**Handles negative weights** (no negative cycles). Detects negative cycles by checking if $dp[i][i] < 0$.

**Why $dp[i][i] < 0$ means negative cycle**: $dp[i][i]$ is the shortest "path" from $i$ back to $i$, i.e., the shortest cycle through $i$. If this is negative, there's a negative cycle. In a graph with no negative cycles, $dp[i][i] = 0$ (the trivial path).

### 8.2 Johnson's Algorithm — $O(nm + n^2 \log n)$

Allows running Dijkstra for all sources even with negative edges.

**Reweighting**: Define $h: V \to \mathbb{R}$ (from Bellman-Ford on augmented graph). New weight: $$\hat{w}(u,v) = w(u,v) + h(u) - h(v)$$

**Key properties**:

1. $\hat{w}(u,v) \geq 0$ for all edges — proved below
2. Shortest paths are preserved: for any path $p = v_0, v_1, \ldots, v_k$ from $u$ to $v$:

$$\hat{w}(p) = \sum_{i=0}^{k-1} \hat{w}(v_i, v_{i+1}) = \sum_{i=0}^{k-1}[w(v_i,v_{i+1}) + h(v_i) - h(v_{i+1})]$$

This telescopes to $w(p) + h(u) - h(v)$. Since $h(u)$ and $h(v)$ are constants for fixed endpoints, minimizing $\hat{w}(p)$ is equivalent to minimizing $w(p)$. So the shortest path under $\hat{w}$ is the same path as under $w$. $\square$

**Algorithm**:

1. Add virtual source $s'$, edges $(s', v, 0)$ for all $v$
2. Run Bellman-Ford from $s'$, get $h[v] = \delta(s', v)$
3. Reweight: $\hat{w}(u,v) = w(u,v) + h[u] - h[v]$
4. Run Dijkstra from each vertex on reweighted graph
5. Recover true distances: $\delta(u,v) = \hat{\delta}(u,v) - h[u] + h[v]$

**Why $\hat{w}(u,v) \geq 0$?** By Bellman-Ford, $h[v] \leq h[u] + w(u,v) \Rightarrow w(u,v) + h[u] - h[v] \geq 0$. $\square$

---

## 9. Minimum Spanning Tree (MST)

Given **undirected, connected, weighted** graph $G = (V,E,w)$, find a spanning tree $T \subseteq E$ minimizing $\sum_{e \in T} w(e)$.

**Spanning tree**: Connected subgraph with $n$ vertices and $n-1$ edges (no cycles).

### 9.1 Generic MST Algorithm (Cut Property)

**Cut**: A partition of $V$ into $(S, V \setminus S)$.

**Cut property**: Let $(S, V \setminus S)$ be any cut. If edge $e$ is the **minimum weight edge crossing** the cut, then $e$ belongs to some MST.

**Proof**: Let $T$ be an MST not containing $e = (u,v)$. Since $T$ spans the graph, there is a path from $u$ to $v$ in $T$. This path must cross the cut $(S, V \setminus S)$ via some edge $f$. Since $w(e) \leq w(f)$, replace $f$ with $e$ to get $T' = T - {f} + {e}$. $T'$ is still a spanning tree (removing $f$ disconnects the path; adding $e$ reconnects it). $w(T') \leq w(T)$, so $T'$ is also an MST containing $e$. $\square$

### 9.2 Kruskal's Algorithm — $O(m \log m)$

**Idea**: Greedily add the cheapest edge that doesn't create a cycle.

```
Kruskal(G):
  sort edges by weight
  T = {}
  for each edge (u,v) in sorted order:
    if Find(u) ≠ Find(v):   // different components
      T = T ∪ {(u,v)}
      Union(u, v)
  return T
```

**Correctness**: Each added edge is the minimum edge crossing the cut between its component and the rest. By the cut property, it belongs to an MST.

**Complexity**: $O(m \log m)$ for sorting. Union-Find operations are nearly $O(1)$ amortized.

### 9.3 Union-Find (Disjoint Set Union)

Supports: **Find(x)** (which component is $x$ in?) and **Union(x,y)** (merge components).

**Optimizations**:

- **Union by rank**: Always attach the smaller tree under the larger
- **Path compression**: During Find, make every node on the path point directly to root

```
Find(x):
  if parent[x] ≠ x: parent[x] = Find(parent[x])
  return parent[x]

Union(x, y):
  rx, ry = Find(x), Find(y)
  if rank[rx] < rank[ry]: swap(rx, ry)
  parent[ry] = rx
  if rank[rx] == rank[ry]: rank[rx]++
```

With both optimizations: amortized $O(\alpha(n))$ per operation, where $\alpha$ is the inverse Ackermann function (effectively constant).

### 9.4 Prim's Algorithm — $O((n+m)\log n)$

**Idea**: Grow MST one vertex at a time, always adding the cheapest edge from the current tree to a new vertex.

```
Prim(G, r):
  key[r] = 0, key[v] = ∞ for v ≠ r, π[v] = NIL
  Q = min-priority queue of all vertices keyed by key
  while Q not empty:
    u = Extract-Min(Q)
    for each v in Adj[u]:
      if v in Q and w(u,v) < key[v]:
        π[v] = u, key[v] = w(u,v)  // decrease-key
```

At the end, ${(π[v], v)}$ is the MST.

**Correctness**: Each extraction adds the minimum-weight edge crossing the cut $(V \setminus Q,\ Q)$. By the cut property, this edge is in some MST.

---

## 10. Trees — Properties and Proofs

**Definition (used in exam puzzles)**: A **tree** is a graph where each pair of vertices is connected by exactly one path.

**Equivalent characterizations** (for graph $G$ on $n$ vertices):

|Statement|Alone sufficient?|
|---|---|
|(i) $G$ is connected|No|
|(ii) $G$ has no cycle|No|
|(iii) $G$ has $n-1$ edges|No|

Any **two** of the above together imply $G$ is a tree.

**Proof that (i)+(ii) $\Rightarrow$ tree**: Connected means a path exists between every pair. If two distinct paths existed between $u$ and $v$, their union would contain a cycle, contradicting (ii). So exactly one path exists between every pair. $\square$

**Proof that (i)+(iii) $\Rightarrow$ tree**: A connected graph on $n$ vertices has $\geq n-1$ edges. If $G$ had a cycle, we could remove an edge from the cycle and remain connected, giving a connected graph with $n-2$ edges — but any connected graph needs $\geq n-1$ edges, contradiction. So $G$ has no cycle, and by (i)+(ii) it's a tree. $\square$

**Proof that (ii)+(iii) $\Rightarrow$ tree**: A forest (acyclic graph) on $n$ vertices with $k$ components has exactly $n-k$ edges. If $n-k = n-1$ then $k=1$, so $G$ is connected. By (i)+(ii) it's a tree. $\square$

**Counterexamples** (for puzzles):

- Only (i): A connected graph with a cycle (e.g., triangle)
- Only (ii): A forest with multiple components (e.g., two disjoint edges on 4 vertices)
- Only (iii): A disconnected graph with $n-1$ edges (e.g., one cycle plus isolated vertex)

---

## 11. Graph Isomorphism

Two graphs $G_1, G_2$ are **isomorphic** if there exists a bijection $f: V_1 \to V_2$ such that $(u,v) \in E_1 \iff (f(u), f(v)) \in E_2$.

**To prove isomorphic**: Explicitly exhibit the bijection $f$ and verify all edges match.

**To prove non-isomorphic**: Find an invariant one has that the other doesn't:

- Different number of vertices or edges
- Different degree sequences (sort and compare)
- One is connected, the other isn't
- Different number of cycles, triangles, etc.

---

## 12. Adjacency Matrix Partition Puzzles

Partition $A$ (the $n \times n$ adjacency matrix) into four $\frac{n}{2} \times \frac{n}{2}$ blocks: $A_{TL}, A_{TR}, A_{BL}, A_{BR}$. Let $L$ = first half of vertices, $R$ = second half.

- $A_{TL}$: edges within $L$
- $A_{BR}$: edges within $R$
- $A_{TR}$: edges from $L$ to $R$
- $A_{BL}$: edges from $R$ to $L$ (for undirected: $A_{BL} = A_{TR}^T$)

**$A_{TR} = 0$ and $A_{BL} = 0$**: No edges between $L$ and $R$ $\Rightarrow$ **$G$ is disconnected** (unless $|L|$ or $|R|$ is 0, which it isn't).

**$A_{TR} = 0$ and $A_{BR} = 0$**: $R$ has no edges touching it $\Rightarrow$ vertices in $R$ are isolated $\Rightarrow$ **$G$ is disconnected** (and definitely not complete).

**Each block is a permutation matrix** (exactly one 1 per row, one 1 per column): Every vertex has degree exactly 2 (one edge within its half, one edge crossing). The graph is 2-regular. A 2-regular graph consists of disjoint cycles $\Rightarrow$ **$G$ definitely contains a cycle**.

---

## 13. Key Complexity Summary

|Algorithm|Time|Notes|
|---|---|---|
|BFS|$O(n+m)$|SSSP for unweighted|
|DFS|$O(n+m)$|Topo sort, SCC|
|Topological Sort|$O(n+m)$|DAGs only|
|Kosaraju SCC|$O(n+m)$|Two DFS passes|
|Bellman-Ford|$O(nm)$|Negative weights ok|
|Dijkstra (heap)|$O((n+m)\log n)$|Non-negative weights only|
|Floyd-Warshall|$O(n^3)$|APSP, negative ok|
|Johnson's|$O(nm + n^2\log n)$|APSP, sparse graphs|
|Kruskal's MST|$O(m\log m)$|Sort + Union-Find|
|Prim's MST|$O((n+m)\log n)$|Like Dijkstra|

---

## 14. Exam-Style Intuitions

**Why no back edges $\Leftrightarrow$ DAG?** A back edge $(u, \text{ancestor})$ directly creates a cycle. Conversely, any cycle must have at least one back edge in DFS (since DFS can't exit a cycle without one).

**Why topological order = reverse finish time?** In a DAG, edge $(u,v)$ means $u$ must come first. DFS finishes $v$ before $u$ (since $v$ is deeper), so reversing finish times puts $u$ before $v$.

**Why Dijkstra fails with negative edges?** Once we extract $u$ and finalize $d[u]$, a later negative edge could provide a shorter path to $u$ through a vertex still in $Q$. We'd miss it since $u$ is already processed.

**MST uniqueness**: If all edge weights are distinct, the MST is unique.

**Proof**: Suppose two distinct MSTs $T_1$ and $T_2$ exist. Let $e$ be the minimum weight edge in $T_1 \setminus T_2$ (in $T_1$ but not $T_2$). Adding $e$ to $T_2$ creates a cycle $C$. Since $T_1$ has no cycle, some edge $f \in C$ is not in $T_1$, so $f \in T_2 \setminus T_1$. By our choice of $e$, $w(e) < w(f)$ (since all weights distinct and $e$ was the min edge in the symmetric difference). Then $T_2' = T_2 + e - f$ is a spanning tree with smaller weight than $T_2$, contradicting $T_2$ being an MST. $\square$

**Kruskal vs Prim**: Kruskal works globally (sort all edges); better for sparse graphs. Prim works locally (expand from one tree); better with adjacency matrix ($O(n^2)$).