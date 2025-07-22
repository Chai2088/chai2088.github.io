---
title: "CHIP-8 Emulator"
category: [Emulation, Low-Level Programming]
layout: page
image: /assets/img/Bloom.png
---
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

