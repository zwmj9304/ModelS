---
layout: post
title: Proposal - Project Model S
---
![TrailerImg]({{ site.url }}/_images/trailer.png "Spoiler Alert!")
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
typically consist of *too many* faces, which is not friendly for remodelling.

Therefore, an efficient algorithm is needed for simplifying large input meshes while preserving most of the geometric
features. After thorough and careful research, we decided to use *Variational Shape Approximation* (a.k.a. VSA) as the 
simplification algorithm. (Details of this research can be found in the "More Background" section.)

### Challenges

### Resources

### Goals and Deliverables
The goal is to achieve good speed up of VSA, enabling processing of very large data sets.

We are using C++ with Scotty3D as our major platform for developing. We will further implement CUDA based parallel VSA
and integrate it into Scotty3D. The CUDA VSA program will also be further extracted out as a stand alone program to be
integrated on other modelling platforms.
### Schedule
