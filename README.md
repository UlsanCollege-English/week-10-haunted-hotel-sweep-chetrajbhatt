[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/I7NCKCh8)
# Week 10 Coding #8: Haunted Hotel Sweep

## Summary

This assignment models a haunted hotel as a **graph**, where each room or area is a node and hallways between them are edges stored in an adjacency list (`dict[str, list[str]]`). BFS (Breadth-First Search) explores the hotel level by level using a queue, while DFS (Depth-First Search) dives deep down each corridor using a stack. A `visited` set is essential in both traversals to avoid revisiting rooms and getting stuck in cycles — like endlessly walking the same haunted hallway.

---

## Approach

- **`get_neighbors`** — used `graph.get(area, [])` to safely return neighbors without raising a `KeyError` if the area is missing.
- **`has_path`** — ran a BFS from `start`, tracking visited nodes; returned `True` the moment `target` was dequeued, and `False` if the queue emptied without finding it.
- **`bfs_order`** — initialized a `deque` with `start`, then repeatedly dequeued the front node, appended it to the result, and enqueued any unvisited neighbors to explore layer by layer.
- **`dfs_order`** — used a list as a stack; popped the top node, marked it visited, then pushed neighbors in **reverse order** so the first neighbor is processed first, preserving original neighbor order.
- **`count_reachable_areas`** — reused `bfs_order` and returned its length, since BFS already collects every unique reachable node.
- **Preventing repeated visits** — added each node to `visited` before (BFS) or upon (DFS) processing, so no room is ever swept twice.

---

## Complexity

### `get_neighbors`

- Time: O(1)
- Space: O(1)
- Why: Just a dictionary lookup — constant time regardless of graph size.

### `has_path`

- Time: O(V + E)
- Space: O(V)
- Why: In the worst case BFS visits every vertex (V) and edge (E). The visited set and queue each hold at most V nodes.

### `bfs_order`

- Time: O(V + E)
- Space: O(V)
- Why: Every vertex is enqueued once and every edge is checked once. The queue and visited set grow up to V nodes.

### `dfs_order`

- Time: O(V + E)
- Space: O(V)
- Why: Every vertex is pushed/popped once and every edge is checked once. The stack and visited set hold at most V nodes.

### Stretch: `count_reachable_areas`

- Time: O(V + E)
- Space: O(V)
- Why: Delegates entirely to `bfs_order`, inheriting the same time and space complexity.

---

## Edge-Case Checklist

- [x] empty graph — `get_neighbors` returns `[]`; all traversal functions return `False`, `[]`, or `0`
- [x] missing start area — all traversal functions check `if start not in graph` and return early
- [x] missing target area — `has_path` checks `if target not in graph` and returns `False`
- [x] `start == target` — BFS in `has_path` dequeues `start` and immediately returns `True`
- [x] graph with a cycle — the `visited` set prevents infinite loops in all traversals
- [x] disconnected graph — BFS/DFS only visit nodes reachable from `start`; unreachable nodes are never added
- [x] area with no neighbors — `graph.get(current, [])` returns `[]`, so the inner loop does nothing

> Trickiest case: cycles. Without `visited`, the traversal would loop forever between connected rooms. Adding nodes to `visited` before enqueueing (BFS) or upon popping (DFS) was the key fix.

---

## Tests Added

- Test `has_path` returns `True` when `start == target` and the node exists
- Test `bfs_order` and `dfs_order` return `[]` when start is missing from the graph
- Test `count_reachable_areas` returns `0` on an empty graph

---

## Known Limitations

```text
No known limitations.
```

---

## Assistance & Sources

AI used? Y

It helped with:

- explaining the difference between BFS queue and DFS stack behavior
- debugging the reversed neighbor order in `dfs_order`
- syntax reminders for `collections.deque` and `dict.get()`
- reviewing edge cases like cycles and disconnected graphs

Other sources used:

- Python docs: [`collections.deque`](https://docs.python.org/3/library/collections.html#collections.deque)
- Python docs: [`dict.get()`](https://docs.python.org/3/library/stdtypes.html#dict.get)
