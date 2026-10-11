# Computational Geometry in Python

*Four Jupyter notebooks from a Computational Geometry course at the National and Kapodistrian University of Athens (2024-2025): convex hull algorithms with a performance comparison, Voronoi diagrams and Delaunay triangulations, linear programming, and kd-trees with orthogonal range search.*

*The notebooks are written in Greek, and the code and comments are in English. GitHub renders them with their outputs, so no setup is needed to read them.*

## Notebooks

| Notebook | Topic | What is implemented |
|---|---|---|
| [`01_convex_hull.ipynb`](01_convex_hull.ipynb) | Convex hulls (2D and 3D) | Graham Scan, Jarvis March, Divide & Conquer and QuickHull in 2D, plus 3D Gift Wrapping, all written from scratch |
| [`02_delaunay_voronoi.ipynb`](02_delaunay_voronoi.ipynb) | Voronoi and Delaunay | Plots and a complexity discussion, using SciPy's `Voronoi` and `Delaunay` as the assignment required |
| [`03_linear_programming.ipynb`](03_linear_programming.ipynb) | Linear programming | A 2-variable problem solved with SciPy's `linprog`, with the feasible region plotted and the optimum checked by hand |
| [`04_kd_tree_range_search.ipynb`](04_kd_tree_range_search.ipynb) | Geometric search | kd-tree construction with step-by-step visualization, and orthogonal range search, both written from scratch |

## Highlights

### 1. Convex hulls
- The four 2D algorithms produce the same hull on a 140-point test set, as the plots show.
- **Benchmark:** timings for 50 to 1,000,000 uniformly random points in a square. At 1,000,000 points:

| Algorithm | Time |
|---|---|
| QuickHull | 1.25 s |
| Graham Scan | 2.24 s |
| Gift Wrapping | 7.43 s |
| Divide & Conquer | 14.98 s |

- Gift Wrapping runs in O(n·h). Its time stays reasonable here because random points in a square give a small hull (small h), and it would degrade toward O(n²) for points on a circle. The conclusions therefore apply to this point distribution.
- **3D Gift Wrapping** on 120 random points in [−100, 100]³ finds a hull with 78 triangular faces, and the result matches `scipy.spatial.ConvexHull`.

### 2. Voronoi and Delaunay
The two structures are dual to each other: every Voronoi vertex is the circumcenter of a Delaunay triangle, and every Voronoi edge is perpendicular to the corresponding Delaunay edge. In the plane both have size O(n).

### 3. Linear programming
The problem is to maximize z = 3x₁ − 10x₂ subject to seven linear constraints and x₁, x₂ ≥ 0. The optimum lies at a vertex of the feasible region, the intersection of 6x₁ + 7x₂ = 18 and 3x₁ − 12x₂ = −8. Solving the system by hand gives x₁ = 160/93, x₂ = 34/31 and z = −180/31 ≈ −5.81, which agrees with `linprog`.

### 4. kd-tree and range search
- The tree is built by splitting at the median, alternating between the x and y axes.
- The range search skips every subtree that cannot intersect the query rectangle.
- On 120 random points and the rectangle [20, 70] × [30, 80], it finds 31 points, and the result matches a brute-force scan.

## Run it
Requires Python 3.9 or newer.

    pip install -r requirements.txt
    jupyter notebook

Open a notebook and use *Kernel → Restart & Run All*. The benchmark in notebook 01 runs for a short while because it goes up to 1,000,000 points.

## What I Learned
- **The best algorithm depends on the data.** Gift Wrapping looked competitive only because the hull was small. Reading complexity as O(n·h) rather than O(n²) explained the outcome.
- **Theory and measurement can disagree.** Divide & Conquer has the same O(n log n) complexity as Graham Scan, yet it was the slowest in practice, so constant factors and implementation details matter.
