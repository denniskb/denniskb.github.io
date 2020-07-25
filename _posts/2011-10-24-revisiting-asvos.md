---
layout: blog/post
title: Revisiting ASVOs
---

In this article I would like to revisit “Animated Sparse Voxel Octrees” (ASVOs) and voxels in general. Since the release of my bachelor thesis in March 2011, many things happened in the voxel graphics world and I gained a new view on my work, which I’d like to share with you.

#### Contents
- Developments in voxels over the last year
- Short summary of my work
- Conclusion

#### Developments in voxels over the last year
When I started working on ASVOs in 2010, voxels were gaining momentum: There were plans to use voxels in mainstream games (i.e., Crysis 2, [Miner Wars 2081](https://www.minerwars.com/)) and many projects working on voxel engines became widely and publicly known (i.e., [Atomontage Engine](https://www.atomontage.com/) (AE), [Unlimited Detail Technology](http://www.euclideon.com/) (UDT)). Over the course of one year some of the plans came true and the projects made incredible progress.

Although neither Crysis 2 (CryEngine 3) nor Miner Wars 2081 use voxels as rendering primitives, they utilize them internally to a big extent - enough to say that voxels have made it into mainstream games again. Crysis 2 uses SVOs to [enhance the quality of its terrain](https://highperformancegraphics.org/previous/www_2010/media/Keynote/HPG2010_Keynote_Kaplanyan.pdf), while Miner Wars’ completely destructible game world is represented by voxels, which are converted to triangles prior to rendering using a marching cubes like algorithm.

UDT, which has developed a very fast software voxel renderer, has made tremendous improvements in visual quality (compare their [2010 tech demo](https://www.youtube.com/watch?v=Q-ATtrImCx4) with the [2011 tech demo](https://www.youtube.com/watch?v=00gAbgBu8R4)). AE, on the other hand, focuses on creating a complete physics simulator (including destructible environments, liquids, chemistry, …) with voxels as the basis of the abstract model while staying realtime. Its feature list is continuously growing.

Voxel research offered quite some surprises, too. Techniques I initially considered for my work but decided they weren’t feasible (like [realtime voxelization](https://blog.icare3d.org/2011/06/interactive-indirect-illumination-and.html)), became very possible over the course of just one year.

#### Short summary of my work
I invented an animation technique for SVOs, which makes things like skeletal animation possible with voxels. The reason you didn’t hear anything about a revolution of the realtime 3D graphics world yet - or maybe you did *looks at UDT* ;) – is because my technique also has some fallacies. For details please view the [project description on my resume](/#asvo).

Considering the aforementioned developments and the feedback I received, from today’s point of view, I focused way too much on the rasterization aspect in my work instead of emphasizing the fact that the animation technique is renderer-independent. What I should have done is to describe how it can be integrated into ray-tracers and rasterizers and to provide my own rasterizer solely as an implementation example.

#### Conclusion
Not only voxels have evolved. [MegaMeshes](https://www.youtube.com/watch?v=M04SMNkTx9E) is possibly one of the most stunning advancements in the polygon world. They take away one of the main reasons to use voxels: LOD. Of course, progressive meshes existed earlier, but I’ve never seen them utilized so extensively leading to such high polygon counts.

With polygons sharing nearly all of the benefits of voxels (except for destructibility/ease of manipulation) and having some exclusive ones (i.e., animation), there is really only one reason left to use voxels: Unification (and therefore simplification). A SVO not only describes the shape of a voxel model, but can also store visual information like color, density, transparency, etc. Not only is it used to render the model, but it serves as an acceleration structure for many algorithms like culling, intersection and collision tests, etc. Its traversal automatically yields LOD and feedback for streaming. All of these use cases can be implemented elegantly as slight variations of depth first search.

By using voxels, one could unify many parts of a game engine and make writing, maintaining and extending one, simpler. That also means that, in order to switch from polygons to voxels or at least incorporate voxels in an existing engine, one would have to rewrite many parts of it. It’s questionable whether the benefits would/will exceed the effort.