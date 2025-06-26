---
layout: page
icon: fas fa-code
order: 1
---

During my four years in college I have worked in several projects, including personal and school projects. Here I have posted some of coolest projects I have worked on.

## Crash N Burn
3D racing game featuring combat developed on a custom engine based on OpenGL. During this project I worked as the graphics programmer where I implemented a full featured 3D graphics pipeline. Among some rendering techniques I implemented in this project include: Cascaded Shadow Maps, Intance Rendering for Particles, PBR bloom.

### Cascaded Shadow Maps
> This is shadow rendering technique where the shadow map is split in multiple sections. Each section will render shadows at different distances from the camera. This ensures shadows closer to the camera have good quality and shadows far from the camera have less quality.
{% 
    include embed/youtube.html id='zNhy4wnzVCw' 
%}

```c++

```
### Particles
> To elevate the visual quality of the application, I built a custom particle system using instanced rendering—making it possible to display thousands of particles smoothly and efficiently.

### Bloom
> Bloom creates a glowing light effect around bright areas, making visuals feel more lifelike and cinematic. I included this feature in our game engine because it gives a more realistic feel to the light, also it blends very well to the aesthetic of the game.

## Virtual Mayhem


## Vulkan Renderer
A 3D graphics engine developed using Vulkan as a personal learning project. I built it from the ground up to explore and understand the Vulkan API. The engine includes common features found in modern graphics engines, such as lighting, shadows, and a particle system.

## Real Time Raytracer
A real-time ray tracer developed using Vulkan, leveraging its GPU ray tracing extensions. Built from scratch as part of a class project, this application delivers impressive visual results—especially given its real-time performance.

### History Tracking
> Instead of recalculating pixel values from scratch every time the camera moves—which is highly inefficient—we store previous frame pixels and use camera motion to track where each pixel maps in the new frame. This technique improves performance by reusing data across frames and it also conserves the quality of the image.

### Denoising
> Since noise is inevitable in real-time ray tracing, we use the A-Trous algorithm for denoising. This technique works by sampling neighboring pixels and computing an average to produce a smoother final image.

## CHIP-8 Emulator

## Chess AI

## Other Projects

> ### Bounding Volume Hierarchy
>> This is a space partioning technique used for improving the speed in queries. This can be used in many ways, such as performing collision queries, ray queries and checking which object to render which ones not. 

> ### Octrees
>>Octrees are another space partioning technique commonly used for raytracing and also collisions.

