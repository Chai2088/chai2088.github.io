---
title: "Virtual Mayhem"
category: [Game, Custom Engine]
layout: page
image: /assets/img/VirtualMayhemPoster.png
description: 9 artist and 4 programmers
---

__2D combat game set in a futuristic world__ similar to Street Fighter and Mortal Combat. This game was also built from scratch in collaboration with a 13 people (9 artists and 4 programmers) using a __custom engine__ developed by ourselves. [Try it here](https://www.digipen.es/es/galeria/juegos-de-estudiantes/virtual-mayhem)

{% 
    include embed/youtube.html id='P_VkUZp7_i4' 
%}
## Texture Atlas Editor Tool
To optimize texture loading in our game (which had hundreds of textures), I designed and built a custom editor tool that:

* Efficiently loads and manages texture atlases

* Allows visual selection of individual sprites via UV coordinate modification

* Features an intuitive interface displaying all atlas sprites in a grid

* Enables single-click sprite assignment to renderable objects

* Automatically stores selections for streamlined workflow

This tool significantly improved our loading pipeline, instead of loading thousands of textures we are loading half or more than half of it.


## Shaders
During the project I have playing around with shaders and developed a couple of shaders for the game. 

 * __Glitch__\
I developed a custom glitch shader to enhance the game's visual aesthetic and add a layer of stylized distortion. This effect simulates digital interference — with flickering lines, color channel shifts, and jittering.

* __Health Bar__\
Unlike traditional health bars found in most games, this shader represents a dynamic advantage bar rather than a static health meter. It visually conveys which player currently holds the upper hand in a match.

## Dynamic Story Log System
__Key Features:__

* Designed a sequential text-delivery system that reveals narrative in controlled, immersive chunks

* Integrated auto-triggered combat encounters between story segments to maintain gameplay rhythm

* Built a tool to reveal dialogue slowly that also enables to skip it.

__Impact:__

* Game is more immersive with a story for each character.

## AI Combat System
To allow a single player experience I designed and built an AI Combat system.

__Key Features:__

* Developed a responsive AI opponent that mimics human-like combat behavior (attacks, dodges, counters)

* Implemented 3 difficulty levels (Easy, Medium, Hard) with dynamic adjustments to aggression and reaction time

* Balanced AI to ensure fair but challenging solo gameplay, tested across 50+ play sessions

__Impact:__

* Allows for single player experience

* Adds extra challenge specially when facing _Hard_ difficulty level
