---
layout: post
title: Checkpoint - Project Model S
---

<div class="message">
  <b>Summary:</b> We are following our schedule pretty well. 
  Our next steps will be testing different parallel approaches.
</div>

### Checkpoint Results
##### Sequential Implementation of VSA
We have implemented the two major parts the Variational Shape Approximation as described
in the reference research paper. Given an input mesh, VSA will first try to find an optimal
partition using Lloyd Algorithm with L<sub>2,1</sub> error metric. After Lloyd Algorithm
converges, VSA will initiate a second meshing step, that builds a new simplified mesh
from the result of first step.

![ResultImg]({{site.rawurl}}/_images/checkpoint_result.jpg "Variational Shape Approximation")

##### CUDA Integration in Scotty3D
We have integrated CUDA in Scotty3D. Compilation is tested on GHC machines and Ubuntu 16.04 
(with CMake 3.8.4 and CUDA 8.0 installed). CUDA function calls are tested on a Ubuntu machine
with Nvidia GTX 1060 graphics card.
Building on Windows with Visual Studio 2017 and CUDA 9.0 is being tested.

##### Adjancency List Representation of Mesh Faces
We have written a few functions to convert the original pointer chasing scheme of Scotty3D into 
a more parallel friendly data structure. The new data structure is an array of structs which
store the particular face's neighbors' indices, normal vector, centroid, and whether the face is a 
boundary face or not. The idea is that when we want to go to a specific face, we don't have to go 
through all the pointers until we find the one we want, but instead can specifically access the 
index in the array.

##### Baseline Performance Measurements
The following result is obtained on a MacBook Air machine with 1.7 GHz Intel Core i5 processor,
with -O3 compiler optimization.
```
[VSA] Input Face Count = 28576, Proxy Count = 200, Iterations = 20
[VSA] Initialization Time (0.0012 sec)
[VSA] Flooding Time       (0.8839 sec)
[VSA] Proxy fitting Time  (0.2336 sec)
[VSA] Meshing Time        (0.0656 sec)
```
As seen from the above results, flooding and proxy fitting takes up most of CPU time. We will
focus on parallelizing those two routines.

### Problems
Due to an X11 forwarding issue, we were not able to run Scotty3D on GHC machines. Although we still
wish to run Scotty3D through X11 forwarding on GHC machines, we are working on an alternative approach 
to directly run the CUDA VSA algorithm bypassing X11 forwarding.

### Schedule Checklist
##### Environment set up
- (FINISHED) Clean up Scotty3D
- (FINISHED) Implement part of the VSA algorithm

##### Working sequential algorithm on Scotty3D
- (FINISHED) Implement the whole set of algorithm in Scotty3D
- (FINISHED) Develop halfedge-like mesh data structure for CUDA

##### Preliminary CUDA Integration
- (FINISHED) Build up the CPU-GPU processing pipeline for input meshes
- (FINISHED) Support CUDA function call in Scotty3D
- (DOING RIGHT NOW) Test one or two parallel approches

### Revised Schedule
##### Nov 27 2017 - Support for Interactive Simplification
- Test one or two parallel approches
- Test on large inputs
- Compare different approaches and continue optimizing

##### Dec 4 2017 - Parallel VSA finished
- Finish parallel VSA implementation
- Add features on top of VSA to support interactively adding and removing faces

##### Dec 11 2017 - Prepare for presentation
- Gather performance data
- Poster and documentation
