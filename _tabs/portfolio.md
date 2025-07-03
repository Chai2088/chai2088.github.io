---
layout: page
icon: fas fa-code
order: 1
---

During my four years in college I have worked in several projects, including personal and school projects. Here I have posted some of coolest projects I have worked on.

## Crash N Burn
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
## Virtual Mayhem
__2D combat game set in a futuristic world__ similar to Street Fighter and Mortal Combat. This game was also built from scratch in collaboration with a 13 people (9 artists and 4 programmers) using a __custom engine__ developed by ourselves. [Try it here](https://www.digipen.es/es/galeria/juegos-de-estudiantes/virtual-mayhem)

{% 
    include embed/youtube.html id='P_VkUZp7_i4' 
%}
### Shaders
> During the project I have playing around with shaders and developed a couple of shaders for the game. 
>
> * __Glitch__\
I developed a custom glitch shader to enhance the game's visual aesthetic and add a layer of stylized distortion. This effect simulates digital interference — with flickering lines, color channel shifts, and jittering.
>
>* __Health Bar__\
Unlike traditional health bars found in most games, this shader represents a dynamic advantage bar rather than a static health meter. It visually conveys which player currently holds the upper hand in a match.

### Story Mode System
> I designed and implemented a story log system that delivers narrative text in a controlled, sequential flow. The system allows story events to unfold gradually, enhancing immersion and pacing. Between story segments, it seamlessly triggers combat encounters, creating a dynamic interplay between storytelling and gameplay progression.

### AI Opponent
> Since the game is designed as a two-player combat experience, I developed an __AI opponent__ to ensure a compelling solo mode as well. The AI can engage the player in responsive, fast-paced combat, simulating a real opponent. To make the game accessible to a wider audience, I implemented three __difficulty levels__, allowing players of varying skill levels to enjoy the game at their own pace.

## Vulkan Renderer
A 3D graphics engine developed using Vulkan as a personal learning project. I built it from the ground up to explore and understand the Vulkan API. The engine includes common features found in modern graphics engines, such as lighting, shadows, and a particle system.

## Real Time Raytracer
A real-time ray tracer developed using Vulkan, leveraging its GPU ray tracing extensions. Built from scratch as part of a class project, this application delivers impressive visual results—especially given its real-time performance.

{% 
    include embed/youtube.html id='ZG7WJ7GvBRA' 
%}

### History Tracking
> Instead of recalculating pixel values from scratch every time the camera moves—which is highly inefficient—we store previous frame pixels and use camera motion to track where each pixel maps in the new frame. This technique improves performance by reusing data across frames and it also conserves the quality of the image.

### Denoising
> Since noise is inevitable in real-time ray tracing, we use the A-Trous algorithm for denoising. This technique works by sampling neighboring pixels and computing an average to produce a smoother final image.

## CHIP-8 Emulator
For our final project in an optimization class, I developed a CHIP-8 emulator. This project gave me hands-on experience with low-level systems and taught me how to emulate a simple virtual machine from scratch.

As part of the emulator, I built a basic disassembler that translates raw bytes into human-readable CHIP-8 instructions. The disassembler works by reading each opcode and matching it to its corresponding instruction using simple pattern checks.

Additionally, I implemented a display system that interprets drawing instructions and renders pixels to the screen—allowing programs written for the CHIP-8 to run and display as intended.

### Disassembler
```c++
switch (op)
{
case 0x0:
    instruction = "LD";
    return true;
case 0x1:
    instruction = "OR";
    return true;
case 0x2:
    instruction = "AND";
    return true;
case 0x3:
    instruction = "XOR";
    return true;
case 0x4:
    instruction = "ADD";
    return true;
case 0x5:
    instruction = "SUB";
    return true;
case 0x6:
    instruction = "SHR";
    return true;
case 0x7:
    instruction = "SUBN";
    return true;
case 0xe:
    instruction = "SHL";
    return true;
default:
    break;
}
return false;
```
### Challenges

>One of the main challenges I faced while developing the CHIP-8 emulator was managing the timing. Since CHIP-8 was originally designed for very old hardware, running it on modern systems resulted in extremely high framerates. Without proper frame limiting, the emulator would run at 2x (or even faster) speed, making the games unplayable.
>
>To solve this, I implemented a frame rate cap. As shown in the code below, I process opcodes in a loop for up to 1.667 milliseconds, matching the 600Hz refresh rate of the original CHIP-8. After that, I render the frame to the screen. This approach ensures that instruction processing runs independently of rendering, preserving both timing accuracy and performance.

```c++
auto start = std::chrono::steady_clock::now();
const float cycleTime = 1.667f; // in ms = 600 Hz
while (mRunning)
{
    CheckInputs();

    auto end = std::chrono::steady_clock::now();
    std::chrono::duration<float, std::chrono::milliseconds::period> elapsed_time{ end - start };
    
    //keep processing opcode until reaching the 60 fps framerate
    if (elapsed_time.count() > cycleTime)
    {
        //process opcodes
        mCore.Update(elapsed_time.count());
        start = end;
        mDrawTimer += elapsed_time.count();
    }
    //Draws to the screen happen every 16.67 ms (60 fps)
    if (mDrawTimer > maxFrameTime || mCore.mScheduledDraws > 0)
    {
        mRenderer.Clear();
        Chip8VideoToScreen();
        mRenderer.Present();
        mCore.mScheduledDraws = 0;
    }
}
```


## AI Chess
This class project was developed by a team of three with the goal of researching and comparing different algorithms that can play chess. As part of our exploration, we built a demo where users can play against various AI opponents we developed.

Our focus was on finding a balance between the speed of decision-making and the quality of the moves chosen by the AI. We implemented several algorithms and compared them based on both their performance (number of wins) and their computational efficiency (decision time). This hands-on approach allowed us to better understand the trade-offs between fast decision-making and strategic depth in AI gameplay.

### Minimax Algorithm
> It evaluates possible moves by exploring a game tree, where players alternate turns as either the maximizing or minimizing player. The algorithm simulates all possible moves until it reaches terminal nodes (end states), then works its way back up the tree to choose the optimal move.

```c#
if (depth == 0)
{
    return EvaluateBoard(board);
}

if (isMaximizingPlayer)
{
    int maxEval = int.MinValue;
    var allPieces = board.Pieces.Values.Where(p => p.Piece.IsWhite == IsWhite).ToList();

    foreach (var piece in allPieces)
    {
        var possibleMoves = piece.GetPossibleMoves(board).ToList();
        foreach (var move in possibleMoves)
        {
            var originalPosition = piece.Position;
            var targetPiece = board.Pieces.ContainsKey(move) ? board.Pieces[move] : null;

            // Make the move
            piece.Move(move);
            board.Pieces[move] = piece;
            board.Pieces.Remove(originalPosition);

            int eval = Minimax(board, depth - 1, false);

            // Undo the move
            piece.Move(originalPosition);
            board.Pieces[originalPosition] = piece;
            if (targetPiece != null)
            {
                board.Pieces[move] = targetPiece;
            }
            else
            {
                board.Pieces.Remove(move);
            }

            maxEval = Math.Max(maxEval, eval);
        }
    }

    return maxEval;
}
else
{
    int minEval = int.MaxValue;
    var allPieces = board.Pieces.Values.Where(p => p.Piece.IsWhite != IsWhite).ToList();

    foreach (var piece in allPieces)
    {
        var possibleMoves = piece.GetPossibleMoves(board).ToList();
        foreach (var move in possibleMoves)
        {
            var originalPosition = piece.Position;
            var targetPiece = board.Pieces.ContainsKey(move) ? board.Pieces[move] : null;

            // Make the move
            piece.Move(move);
            board.Pieces[move] = piece;
            board.Pieces.Remove(originalPosition);

            int eval = Minimax(board, depth - 1, true);

            // Undo the move
            piece.Move(originalPosition);
            board.Pieces[originalPosition] = piece;
            if (targetPiece != null)
            {
                board.Pieces[move] = targetPiece;
            }
            else
            {
                board.Pieces.Remove(move);
            }

            minEval = Math.Min(minEval, eval);
        }
    }

    return minEval;
}
```
### Alpha-Beta Pruning
> This is a optimization based of the minimax algorithm. It speeds up the decision-making process by eliminating branches in the game tree that don’t need to be explored because they cannot influence the final decision. 
> * Alpha refers to the best score the maximizing player can get.
> * Beta refers to the best score the minimizing player can get.
>
> If the move leads to worse output than the previous move it skips the current branch.

```c#
if (isMaximizingPlayer)
{
    if (boardValue > bestValue)
    {
        bestValue = boardValue;
        bestMove = (piece, move);
    }
    alpha = Math.Max(alpha, boardValue);
}
else
{
    if (boardValue < bestValue)
    {
        bestValue = boardValue;
        bestMove = (piece, move);
    }
    beta = Math.Min(beta, boardValue);
}

if (beta <= alpha)
{
    // Undo the move
    piece.Move(originalPosition);
    board.Pieces[originalPosition] = piece;
    if (targetPiece != null)
    {
        board.Pieces[move] = targetPiece;
    }
    else
    {
        board.Pieces.Remove(move);
    }
    return bestMove;
}
```
## Other Projects

### Bounding Volume Hierarchy
> This is a space partioning technique used for improving the speed in queries. This can be used in many ways, such as performing collision queries, ray queries and checking which object to render which ones not. 

### Octrees
>Octrees are another space partioning technique commonly used for raytracing and also collisions.

