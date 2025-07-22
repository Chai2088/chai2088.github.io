---
title: "Vulkan Renderer"
category: [Vulkan, Graphics Engine, Ray Tracing]
layout: page
image: /assets/img/Bloom.png
---
A real-time ray tracer developed using Vulkan, leveraging its GPU ray tracing extensions. Built from scratch as part of a class project, this application delivers impressive visual results—especially given its real-time performance.

{% 
    include embed/youtube.html id='ZG7WJ7GvBRA' 
%}

### History Tracking
> Instead of recalculating pixel values from scratch every time the camera moves—which is highly inefficient—we store previous frame pixels and use camera motion to track where each pixel maps in the new frame. This technique improves performance by reusing data across frames and it also conserves the quality of the image.

### Denoising
> Since noise is inevitable in real-time ray tracing, we use the A-Trous algorithm for denoising. This technique works by sampling neighboring pixels and computing an average to produce a smoother final image.
