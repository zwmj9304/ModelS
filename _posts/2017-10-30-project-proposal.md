---
layout: post
title: Proposal - Project Model S
---
![TrailerImg]({{site.rawurl}}/_images/trailer.png "Re-modelling of a 3D scanned building")
<div class="message">
  <b>Summary:</b> We are parallelizing Variational Shape Approximation, a mesh simplifying algorithm to 
  efficiently process large input meshes obtained from 3D scanners.
</div>

### Background
Using 3D scanning to measure and reproduce drawings/models of existing objects has always been an important topic.

In the world of design, there has been an increasing trend to utilize 3D scanning to facilitate modeling. One of the
most classical scenario was back in the 1990s, when Frank Gehry had Rick Smith digitize his physical model of 
Walt Disney Concert Hall, which is later used for computational modelling. This application of 3D scanning is very
intriguing and promising for designers. However, designers are often frustrated by the fact that reconstructed meshes
typically consist of **too many** faces, which is not friendly for remodelling.

Therefore, an efficient algorithm is needed for simplifying large input meshes while preserving most of the geometric
features. After thorough and careful research, we decided to use **Variational Shape Approximation** (a.k.a. VSA) as the 
simplification algorithm. 

VSA has the following advantages in terms of simplifying a mesh:
1.	It outputs polygon mesh, which is user-friendly to human editors.
2.	It adheres to the geometric structure of the original object.
3.	It enables interactive remeshing. User can point to parts of the original mesh to add a face to the simplified 
result.

Current problems with VSA are that:
1.	There is no working open source implementation of a whole set of VSA algorithm (presented in [Cohen-Steiner et al.’s
paper](https://hal.inria.fr/inria-00070632/file/RR-5371.pdf), including meshing).
2.	As pointed out, VSA is slower than other greedy mesh decimation algorithms because it involves multiple iterations,
which opens opportunities to parallelize this algorithm using various techniques we learn in 15148.

Details of this research can be found in the 
[More Background]({{site.baseurl}}more_background.html) section.

### Challenges
The major part of this algorithm being parallelized is the Lloyd iteration part, which is quite similar to the [K-Means
clustering algorithm](https://en.wikipedia.org/wiki/K-means_clustering) that is widely used in machine learning. 
Although parallelization of K-Means has been an wellstudied topic, the algorithm we are using has some major differences
from K-Means that makes our project quite challenging.

1. While K-Means has this good data parallel property where each distance calculation can be performed in parallel, VSA
relies on this region growing procedure that uses a global priority queue. How we program CUDA over this priority queue
can be very challenging.
2. In Scotty3D, mesh is stored with a pointer based halfedge data structure, which is inefficient in CUDA. Implementing
an efficient halfedge-like data structure for CUDA is also a challenge.
3. We are dealing with huge input sizes (presumably billions of faces in one single mesh). Memory efficiency is also a
challenge that could directly limit our input size.
4. (optional) We will also consider migrating of our code onto different modelling software platforms. Integrating CUDA on those
platforms will be a problem.
### Resources
* [VSA Source Code (partial) used in MEPP](https://github.com/MEPP-team/MEPP/blob/master/src/components/Segmentation/VSA/src/VSA_Component.cpp)
* [An experimental (broken) implementation of VSA](https://github.com/cnr-isti-vclab/meshlab/tree/master/src/plugins_experimental/filter_vsa)
* [A CUDA implementation of K-Means](https://github.com/src-d/kmcuda)
* [Autodesk Recap Software](https://www.autodesk.com/products/recap/overview)
### Goals and Deliverables
The goal is to achieve good speed up of VSA without downgrading its output quality, and to enable processing of very 
large data sets.

It is desired that given large inputs, our CUDA VSA can beat CPU based greedy mesh decimation algorithms. Because the
amount of computation is different between these two algorithms, VSA may not be able to compete with greedy method if
both of them are implemented in CUDA.

We are using C++ with Scotty3D as our major platform for developing. We will further implement CUDA based parallel VSA
and integrate it into Scotty3D. The CUDA VSA program will also be further extracted out as a stand alone program to be
integrated on other modelling platforms.

### Schedule
##### **Nov 6 2017** - Environment set up
- Clean up Scotty3D
- Implement part of the VSA algorithm
##### **Nov 13 2017** - Working sequential algorithm on Scotty3D
- Implement the whole set of algorithm in Scotty3D
- Developed halfedge-like mesh data structure for CUDA
##### **Nov 20 2017** - Preliminary CUDA Integration
- Build up the CPU-GPU processing pipeline for input meshes
- Support CUDA function call in Scotty3D
- Test one or two parallel approches
##### **Nov 27 2017** - Support for Interactive Simplification
- Add features on top of VSA to support interactively adding and removing faces
- Continue testing parallel approaches
##### **Dec 4 2017** - Parallel VSA finished
- Finished parallel VSA implementation
- Test on large inputs
- Gather performance data
##### **Dec 11 2017** - Prepare for presentation
- Build up the deliverable
- Poster and documentation
