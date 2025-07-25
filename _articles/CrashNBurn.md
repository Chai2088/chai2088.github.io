---
title: "Crash N Burn"
category: [Game, Custom Engine]
layout: page
image: /assets/img/CrashNBurn.png
order: 1
---

__3D racing game featuring combat__ developed on a __custom engine__ based on OpenGL. This was a team project composed of 8 programmers. During this project I worked as the graphics programmer where I implemented a full featured 3D graphics pipeline. Among some rendering techniques I implemented in this project include: Cascaded Shadow Maps, Intance Rendering for Particles, PBR bloom. [Try it here](https://www.digipen.es/es/galeria/juegos-de-estudiantes/crashnburn)
{% 
    include embed/youtube.html id='zNP3G0KptD4'
%}

## Cascaded Shadow Maps

__Challenge:__

* Traditional shadow maps would require a big amount of memory

* Resolution limitations might reduce the shadow quality

__Solution:__

* Implemented Cascaded Shadow Maps with 3 partitions

* Reduces the memory use from the GPU

* Ensures shadows closer to the camera have very good quality

{% 
    include embed/youtube.html id='JlPHo7LQlpQ'
%}

## Particle System
I designed and built a particle system in order to add extra visual effects into the game

__Key Features:__

__🎯 Billboard Optimization__

* Implemented camera-facing quads

* Renders much faster than 3D particles with similar visuals

__⚡ Instanced Rendering__

* Batches render calls into a single render call

* Allows faster render time due to GPU friendly memory layout

* Enables higher particle counts compared to forward rendering

__🛠️ Editor Tool__

* Created an editor for more accesible particle creation and modification

* Allows for saving and deleting of particles

* Has an attribute control panel to modify velocity, particle life...

__Impact:__

* Enchances visual quality of the game

* Very fast rendering time and allows very big quantities of particles

## Bloom
Bloom creates a glowing light effect around bright areas, making visuals feel more lifelike and cinematic. I included this feature in our game engine because it gives a more realistic feel to the light, also it blends very well to the aesthetic of the game.

![Bloom](/assets/img/Bloom.png)

## Speed FX Shaders
__🏁 Radial Motion Blur__

* Dynamic screen-space blur intensity tied to ship velocity

* Adjustable falloff from edges to maintain center clarity

__🚀 Velocity Rays__

* Scale opacity/width with acceleration

* Rotates depending of the velocity

__📽️ Camera Shake__

* Triggers a camera shake when stepping into a boost

* Creates a sensation of sudden acceleration 

## Player HUD

* Allows for 2 - 4 player displays

* Added real time lap time lap information, such as, current lap time and best lap

* Shows the player scoreboard at the end of the game

* Dynamically adjust to screen size


![HUD Reference](/assets/img/HUD.png)