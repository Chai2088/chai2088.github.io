---
title: "Crash N Burn"
category: [Game, Custom Engine]
layout: page
image: /assets/img/Bloom.png
---

__3D racing game featuring combat__ developed on a __custom engine__ based on OpenGL. This was a team project composed of 8 programmers. During this project I worked as the graphics programmer where I implemented a full featured 3D graphics pipeline. Among some rendering techniques I implemented in this project include: Cascaded Shadow Maps, Intance Rendering for Particles, PBR bloom.[Try it here](https://www.digipen.es/es/galeria/juegos-de-estudiantes/crashnburn)
{% 
    include embed/youtube.html id='zNP3G0KptD4'
%}

### Cascaded Shadow Maps
> While developing this game we came upon this challenge I had to work on which was the shadow quality, our game is a racing game meaning the map is huge, so we would also need to have an equally big shadow map, but having a single huge shadow map wasnt the best solution for our problems, as it require a huge chunk of memory, and additionatilly not all the places required a good shadow quality. 
>
>Thus I chose this approach called cascaded shadow maps, which is a shadow rendering technique where the shadow map is split into multiple sections. Each section will render shadows at different distances from the camera. This ensures shadows closer to the camera have good quality and shadows far from the camera have less quality.

{% 
    include embed/youtube.html id='JlPHo7LQlpQ'
%}

### Particles
> To elevate the visual quality of the application, I built a custom particle system using instanced rendering—making it possible to display thousands of particles smoothly and efficiently. 
>
>In 3D engines, particles can be both 3d geometry or 2d planes, I decided to use the 2d planes with the billboarding approach so that all the 2d planes are always facing the camera, so the user gets the illusion that the particles are 3d but in fact are 2d. 
>
>I thought this approach was more efficient to using 3d geometry, as it would reduce the workload to the GPU and it would get the result were looking for.

### Bloom
> Bloom creates a glowing light effect around bright areas, making visuals feel more lifelike and cinematic. I included this feature in our game engine because it gives a more realistic feel to the light, also it blends very well to the aesthetic of the game.

![CV](/assets/img/Bloom.png)
