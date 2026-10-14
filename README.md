<!-- solve-problem:45fc3989-809f-4e5b-a27b-2cb5542cb748 -->
# Linear-space logarithmic-query 3D point location

A [Tangle Solve problem](https://solve.tangle.tools/problems/45fc3989-809f-4e5b-a27b-2cb5542cb748).

## What is the problem?

Given a subdivision of 3-dimensional space into polyhedral cells — specifically, a planar straight-line graph lifted: a set of O(n) triangular (or constant-complexity polygonal) faces forming the boundary between solid cells, with total complexity n — preprocess it into a data structure with **O(n) space** (linear in the input complexity) that answers point-location queries ("which cell contains query point q?") in **O(log n)** time per query. This is the direct 3D analogue of the classical planar problem, where Kirkpatrick's hierarchy (and later methods) achieve O(n) space and O(log n) query exactly.

Formal setting: input is a polyhedral subdivision of ℝ³ (or a bounding box) with n faces/vertices total; the subdivision is "general" — cells of bounded complexity, no convexity or monotonicity assumptions; queries are single points; worst-case query time must be O(log n); total storage O(n). Preprocessing time is typically allowed to be O(n log n) or polynomial. Ambiguity to note: whether exact arithmetic is required (yes, in the classical formulation) and whether "subdivision" includes non-manifold or non-contractible cell topologies — most work assumes a cell complex with bounded-degree faces.

## Why does it matters?

A solution would close the most basic data-structure gap between 2D and 3D computational geometry. Point location is the primitive underlying spatial databases, volumetric mesh queries in scientific visualization and simulation, ray shooting, and motion planning in 3D; currently, practitioners in volumetric FEM meshes fall back on heuristics (walking algorithms) with no worst-case guarantees. It would also likely yield linear-space O(log n) structures for related 3D problems (ray shooting amid polyhedra, nearest-neighbor in subdivisions) via standard reductions. Its persistence as TOPP Problem 13 signals that standard techniques are genuinely stuck.

## What's already known?

- **Planar case: solved.** Kirkpatrick (1983): O(n) space, O(log n) query via hierarchical triangulation simplification. Many alternatives exist (separating chains, persistent search trees, randomized incremental — see Snoeyink's survey chapter).
- **3D case:** the trivial solution is O(n) space with O(n) query (linear scan). Known tradeoffs (verify exact bounds before relying on them): query O(log² n) or similar with near-linear space via multi-level/locus approaches; and linear-space structures with sublinear-but-superlogarithmic query time. It is folklore that no known technique achieves O(n) space and O(log n) query simultaneously for general 3D subdivisions.
- **Why standard methods fail:** Kirkpatrick-style simplification requires an independent-set removal argument; in 3D, degenerate vertices/edges cannot be removed while keeping cell complexity bounded (removing vertices in 3D subdivisions can blow up cell complexity). Persistent/locus methods blow up space because the "vertical decomposition" of a 3D subdivision has Θ(n²) complexity. Cutting trees give O(n polylog) space with O(log n)-ish queries only for restricted (e.g., single non-intersecting surface) cases. Special cases that are solved: point location above a single terrain in O(log n) with near-linear space, and convex subdivisions.
- See TOPP Problem 13 (The Open Problems Project, Demaine, Mitchell, O'Rourke) for the canonical statement and status.

## What would count as solving it?

1. **Upper bound (the prize):** A randomized or deterministic construction, for any polyhedral subdivision of ℝ³ of complexity n, using O(n) expected/worst-case space and answering queries in O(log n) time — with a proof.
2. **Lower bound:** A proof that within, say, the pointer-machine or comparison model, Ω(n log^c n) space is required for O(log n) queries (this would resolve the problem negatively).
3. **Partial results:** improved tradeoffs (e.g., O(n) space with O(log n · log* n) query), or solutions for broad subclasses (e.g., bounded-degree, fat, or axis-aligned subdivisions) would be significant progress.

A referee accepts: full proof of correctness and complexity, handling degenerate inputs (or an explicit general-position assumption justified via symbolic perturbation).

## How to start

1. Read Snoeyink's 1997 survey chapter "Point Location" (in *Handbook of Discrete and Computational Geometry*), especially its discussion of the 3D open problem — it explains precisely why planar techniques fail to lift. Then read TOPP Problem 13.
2. Understand the obstruction concretely: try to lift Kirkpatrick's hierarchy to 3D and identify where the independent-set/constant-blowup argument breaks (vertex removal in 3D increases incident-cell complexity). Any solution must handle "cells whose complexity cannot be locally simplified."
3. Consider whether the vertical-decomposition blowup can be avoided: study whether a subquadratic "shallow" decomposition exists, or whether hierarchical schemes over cells (rather than vertices) can maintain bounded complexity. Alternatively, attack restricted subclasses (convex cells, terrains) and then generalize.
4. Consider lower-bound approaches: existing point-location lower bounds (Chazelle, Afshani) via pointer-machine or encoding arguments; check whether an Ω(n log n)-type space bound for 3D is provable.

## Key references

- J. Snoeyink, "Point Location," in *Handbook of Discrete and Computational Geometry* (Goodman & O'Rourke, eds.), CRC Press, 1997. Survey of point-location techniques that states the 3D linear-space/log-query open problem and explains the failure of planar methods in 3D. (Verify edition/pages; a revised version appears in the 3rd edition, 2017, by Snoeyink with Devillers.)
- E. Demaine, J. Mitchell, J. O'Rourke, "The Open Problems Project," Problem 13: "3D Point Location." Canonical problem statement and status discussion. https://topp.openproblem.net/
- D. Kirkpatrick, "Optimal search in planar subdivisions," *SIAM J. Comput.* 12(1):28–35, 1983. The O(n)-space, O(log n)-query planar solution via hierarchical triangulation — the method whose 3D lifting is the core difficulty.
- (Background, verify relevance) B. Chazelle, lower bounds for point location / orthogonal range searching, showing space-query tradeoff proof techniques. (Specific citation to be confirmed by the solver.)

## Warnings

- The census record is thin; the "key works" may be incompletely cited. Verify all claimed known 3D bounds in the literature before claiming novelty — search for work post-dating Snoeyink's survey (e.g., papers on point location in 3D subdivisions, in FEM mesh data structures, and on persistent data structures).
- Do not conflate the general problem with solved special cases: point location in arrangements of a single surface, convex subdivisions, or "beneath/above a terrain" all have near-optimal solutions and do not resolve the general case.
- The naive "lift Kirkpatrick" attempt fails for a structural reason (vertex removal increases cell complexity in 3D); any proposal must explicitly address this, or a referee will reject it immediately.
- Watch space accounting: structures that use O(n) words but store Θ(n²) precomputed comparisons or rely on vertical decompositions do not qualify.
- Walking algorithms (used in practice on Delaunay meshes) have good practical performance but no worst-case O(log n) guarantee — they are not a solution.

---
*`ALGO-000151` · unverified*

## Acceptance guidance

1. **Upper bound (the prize):** A randomized or deterministic construction, for any polyhedral subdivision of ℝ³ of complexity n, using O(n) expected/worst-case space and answering queries in O(log n) time — with a proof.
2. **Lower bound:** A proof that within, say, the pointer-machine or comparison model, Ω(n log^c n) space is required for O(log n) queries (this would resolve the problem negatively).
3. **Partial results:** improved tradeoffs (e.g., O(n) space with O(log n · log* n) query), or solutions for broad subclasses (e.g., bounded-degree, fat, or axis-aligned subdivisions) would be significant progress.

A referee accepts: full proof of correctness and complexity, handling degenerate inputs (or an explicit general-position assumption justified via symbolic perturbation).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/45fc3989-809f-4e5b-a27b-2cb5542cb748?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000151.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000151)
