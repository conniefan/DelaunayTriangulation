# Delaunay Triangulation

Harvey Zhang and Connie Fan

In this project, we explored the parallelization of Delaunay Triangulation algorithms using CUDA on the GPU. Specifically, we analyzed the performance of four GPU implementations based on Voronoi diagrams. For sufficiently large input sizes, our Jump Flood GPU implementation achieves a significantly higher speedup over a single-threaded CPU Randomized Incremental implementation[1]. For an input set of 10 million points, our best implementation has a 70x speedup.

[Final paper](Parallel_Delaunay_Triangulation_on_CUDA.pdf)

[Checkpoint](checkpoint.md)

[Proposal](proposal.md)

## Background
Delaunay Triangulation (DT) is an algorithm that computes a triangulation of a set of points or
vertices such that no vertex is within the circumcircle of any triangle in the resulting triangulation.
Incremental DT attempts to insert a new vertex into an existing set of triangulation by determining the
triangles that violate the Delaunay condition. The DT algorithm is generally computation-heavy and
several components of the algorithm may see significant speedups from parallelization. For example,
the incremental algorithm can be parallelized by allowing for parallel/concurrent insertions into the
existing set of triangles. However, implementing such parallelization schemes may not be trivial [2].

Delaunay Triangulation, in general, can be applied to areas such as 3D reconstruction, meshing, and
even path planning [3]. Because of its compute-heavy nature, parallel implementations of the DT
algorithm have been the subject of much academic research. There have been works on memoryefficient
parallelizations of DT [2] as well as several approaches in implementing GPU-accelerated
versions of DT [4] [5].

##  The Challenge

There are several challenges inherent in the algorithm that make parallelization difficult:
1. **Sequential Nature of Incremental Approach:** The incremental approach of the algorithm
allows vertices to be added to the existing triangulation one at a time. This sequential nature
may present contention or accuracy trade-offs when attempting to insert multiple vertices to
the existing graph at once.
2. **Memory Complexity:** The DT algorithm is a relatively complex algorithm with many
memory allocations and de-allocations, which presents the challenge for efficient use of
memory and improving memory locality.
3. **Iteration Dependencies:** As presented in [6], Incremental DT is fundamentally a randomized
incremental algorithm with iterative dependencies in which the next step may depend
on the previous step. Since the vertices are inserted in an uniform random order, there
will be a certain probability of creating a dependency with existing vertices at any given
insertion/iteration. The random nature of insertion creates problems with both cache locality
and contentions in parallelization.

## Schedule

We will aim to implement several parallel versions of the Delaunay Triangulation algorithm using
different approaches and platforms. In the case that the project progresses slowly, one of either the
GPU-accelerated implementation or distribution MPI implementation will be implemented.


| Week        | Dates           | Tasks  | Assignee |
| :-------------: |:-------------:| -----| -----|
| 1    | 11/4 - 11/10 | Understand the algorithms. Try to fix the starter code so that it compiles. Write at least two different sequential implementations. (Done!) | Both |
| 2    | 11/11 - 11/17      |   Debug sequential implementations and explore GPU-accelerated implementation. (Done!)| Both |
| 3 | 11/18 - 11/24     |   Complete GPU implementation without KNN <br/>Implement GPU Memory Optimizations | Harvey <br/>Connie |
| 4 | 11/25 - 12/1     |  Debug GPU implementation and profile <br/>Research distributed partition schemes <br/>Implement work partitioning scheme | Harvey <br/>Both <br/>Connie |
| 5 | 12/2 - 12/8    |   Implement efficient merging of local triangulations <br/>Research/Brainstorm approaches to reducing thread contention and communication <br/>Explore optimizing KNN implementation and memory optimizations | Harvey <br/>Both <br/>Connie |
| 6 | 12/9 - 12/15     |  Profile Communication and Cache Behavior of MPI implementation <br/>Profile Execution Time and Run Time Distribution <br/>Complete Presentation Poster | Harvey <br/>Connie <br/>Both |


## References

1. Julian Shun, Guy E Blelloch, Jeremy T Fineman, Phillip B Gibbons, Aapo Kyrola, Harsha Vardhan Simhadri, and Kanat Tangwongsan. Brief announcement: the problem based benchmark suite. In *Proceedings of the twenty-fourth annual ACM symposium on Parallelism in algorithms and architectures*, pages 68–70. ACM, 2012.
2. Daniel K Blandford, Guy E Blelloch, and Clemens Kadow. Engineering a compact parallel delaunay algorithm in 3d. In *Proceedings of the twenty-second annual symposium on Computational geometry*, pages 292–300. ACM, 2006.
3. Pranav Kant Gaur and S. K. Bose. On recent advances in 2d constrained delaunay triangulation algorithms, 2017.
4.  Meng Qi, Thanh-Tung Cao, and Tiow-Seng Tan. Computing 2d constrained delaunay triangulation using the gpu. *IEEE transactions on visualization and computer graphics*, 19(5):736–748, 2013.
5.  Guodong Rong, Tiow-Seng Tan, Thanh-Tung Cao, et al. Computing two-dimensional delaunay triangulation using graphics hardware. In *Proceedings of the 2008 symposium on Interactive 3D graphics and games*, pages 89–97. ACM, 2008.
6.  Guy E Blelloch, Yan Gu, Julian Shun, and Yihan Sun. Parallelism in randomized incremental algorithms. In *Proceedings of the 28th ACM Symposium on Parallelism in Algorithms and Architectures*, pages 467–478. ACM, 2016.
