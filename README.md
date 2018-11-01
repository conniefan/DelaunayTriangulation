# Delaunay Triangulation

Harvey Zhang and Connie Fan

In this project, we plan to implement parallelized versions of the Incremental Delaunay Triangulation algorithm. Specifically, we will begin by optimizing a shared address space implementation of DT provided in the Problem Based Benchmark Suite (PBBS) package [1]. We will then attempt to explore one of two approaches 1) a distributed implementation of the incremental DT algorithm using MPI and/or 2) a joint CPU-GPU parallelization implementation using OpenMP and CUDA with the goal of making the algorithm run as fast as possible. 

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

## Resources

We will use the Incremental Delaunay Triangulation implementation in the Problem Based Benchmark
Suite (PBBS) package [1] provided by Professor Guy Blelloch as our starting point. Specifically,
the package contains both a sequential implementation and a benchmark parallel implementation
using Cilk. In the case that we cannot successfully compile the starter code in the package, we will
attempt to write our own sequential and simple OpenMP implementations from scratch, with similar
code structures as that from PBBS. We intend to use this sequential implementation and the basic
Cilk/OpenMP implementation as benchmarks for the evaluation of our implementation.

Aside from the starter code, we will also consult several existing parallel implementations of the DT
algorithm to further understand the problem complexity. Specifically, we will likely utilize ideas
from the papers listed in previous sections, such as [2] and [4].

## Goals and Deliverables

The goals and outcomes of the project are as follows:

PLAN TO ACHIEVE:

1. A working sequential version of Delaunay Triangulation along with a basic parallel version
using Cilk/OpenMP
2. A distributed DT computation using MPI
3. A joint CPU-GPU approach utilizing both GPU-acceleration with CUDA and CPU parallelization
with OpenMP.

HOPE TO ACHIEVE:

* Apply DT to 3D mesh reconstruction and parallelize DT specific to this application setting.

If the project work goes slowly, only one of either goal 2 or 3 will be focused on for the final results.
We expect our parallel implementation to present speedups over the sequential and naive parallel
implementations.

We plan to use mainly speedup graphs as our demo/visualization during our final presentation. If time
permits, we plan to show some mesh-reconstruction results generated from our parallel triangulation
implementation to demonstrate the correctness of the implementation.

## Platform Choice

The GHC machines and Latedays cluster are the target platform for our parallel implementation. The
GHC machines supports both CUDA and OpenMP while the Latedays cluster supports OpenMPI
implementations with relatively large workloads. These platforms are readily available and allow us
to test the implementation with varying problem sizes and hardware cores. The implementation will
be completed in C/C++.

## Schedule

We will aim to implement several parallel versions of the Delaunay Triangulation algorithm using
different approaches and platforms. In the case that the project progresses slowly, one of either the
GPU-accelerated implementation or distribution MPI implementation will be implemented.

| Week        | Dates           | Deliverable  |
| :-------------: |:-------------:| -----|
| 1    | 11/4 - 11/10 | Understand the algorithms. Try to fix the starter code so that it compiles. Write at least two different sequential implementations. |
| 2    | 11/11 - 11/17      |   Debug sequential implementations and explore GPU-accelerated implementation.|
| 3 | 11/18 - 11/24     |    Debug and finish GPU-accelerated implementation. |
| 4 | 11/25 - 12/1     | Implement distributed MPI parallelization implementation. |
| 5 | 12/2 - 12/8    |   Debug MPI parallelization and explore hardware-specific improvements (optimize for GHC machines). |
| 6 | 12/9 - 12/15     |  Perform tests for other variables, such as points in more than 2 dimensions and write final report. |

 
## References

1. Julian Shun, Guy E Blelloch, Jeremy T Fineman, Phillip B Gibbons, Aapo Kyrola, Harsha Vardhan Simhadri, and Kanat Tangwongsan. Brief announcement: the problem based benchmark suite. In *Proceedings of the twenty-fourth annual ACM symposium on Parallelism in algorithms and architectures*, pages 68–70. ACM, 2012.
2. Daniel K Blandford, Guy E Blelloch, and Clemens Kadow. Engineering a compact parallel delaunay algorithm in 3d. In *Proceedings of the twenty-second annual symposium on Computational geometry*, pages 292–300. ACM, 2006.
3. Pranav Kant Gaur and S. K. Bose. On recent advances in 2d constrained delaunay triangulation algorithms, 2017.
4.  Meng Qi, Thanh-Tung Cao, and Tiow-Seng Tan. Computing 2d constrained delaunay triangulation using the gpu. *IEEE transactions on visualization and computer graphics*, 19(5):736–748, 2013.
5.  Guodong Rong, Tiow-Seng Tan, Thanh-Tung Cao, et al. Computing two-dimensional delaunay triangulation using graphics hardware. In *Proceedings of the 2008 symposium on Interactive 3D graphics and games*, pages 89–97. ACM, 2008.
6.  Guy E Blelloch, Yan Gu, Julian Shun, and Yihan Sun. Parallelism in randomized incremental algorithms. In *Proceedings of the 28th ACM Symposium on Parallelism in Algorithms and Architectures*, pages 467–478. ACM, 2016.
