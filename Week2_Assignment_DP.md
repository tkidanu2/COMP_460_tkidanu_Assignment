# Why a Plain Greedy Algorithm Fails for SSSP

## Problem Understanding
The Single-Source Shortest Path (SSSP) problem aims to find the shortest path from a source vertex to all other vertices in a weighted graph. A naive approach might suggest using a plain greedy algorithm, but this demonstrates why such an approach is insufficient.

## The Plain Greedy Strategy
A plain greedy strategy for finding a path from source to target would be:
- At every vertex, choose the outgoing edge with the smallest weight
- Focus on making the **locally optimal** choice at each step
- Hope this leads to a **globally optimal** solution

As I'll demonstrate, this approach fails to guarantee the shortest path.

## The Graph Example
Consider the paths from source vertex `s` to target vertex `t`:

**Available Paths:**
- **Path 1:** `s → a → t`
- **Path 2:** `s → b → c → d → e → t`

## The Greedy Approach Analysis

### Step-by-Step Execution
1. **Start at source `s`:** Two choices available
   - Travel to vertex `a` with cost **1**
   - Travel to vertex `b` with cost **2**

2. **Greedy Decision:** Choose locally optimal edge
   - Since edge `(s, a)` has weight 1 < edge `(s, b)` weight 2
   - Algorithm chooses path to `a`

3. **From vertex `a`:** Only one option
   - Travel to target `t` with cost **100**

4. **Result:** Path `s → a → t` with total weight `1 + 100 = 101`

### The Optimal Path Analysis
If we had chosen the initially suboptimal edge:

1. **Alternative choice:** Travel `s → b` (cost 2)
2. **Complete path:** `s → b → c → d → e → t`
3. **Total weight:** `2 + 2 + 2 + 2 + 2 = 10`

## Conclusion: The Fatal Flaw

### Comparison Results
- **Greedy Algorithm Result:** `s → a → t` with total weight **101**
- **Optimal (Shortest) Path:** `s → b → c → d → e → t` with total weight **10**

### Why the Greedy Approach Failed
The plain greedy algorithm failed because:
- Its initial locally optimal choice (`s → a`) locked it into a globally suboptimal path
- It lacked "foresight" to understand that a slightly more expensive initial step would lead to a much cheaper overall solution
- The algorithm had no mechanism to reconsider or backtrack from poor early decisions

## Key Insights
This example demonstrates that the SSSP problem does **not** exhibit the greedy choice property - locally optimal decisions do not guarantee globally optimal solutions.

### Why Dijkstra's Algorithm Works
While Dijkstra's algorithm is also considered "greedy," it succeeds because:
- It maintains a **global perspective** by tracking total distance from source
- It considers the **cumulative cost** rather than just immediate edge weights
- It systematically explores vertices in order of their distance from the source
- It can "correct" earlier decisions as more information becomes available