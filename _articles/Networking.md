---
title: "Networking project"
category: [Networking, TCP]
layout: page
image: /assets/img/CrashNBurn.png
order: 8
---
Implemented a local multiplayer game that can host 2 players in a turn based game. This was built using __TCP protocol__ from the WinSock Library. The program touches every aspect of networking programing, from initializing the library and sockets to connecting the players to the server and finally it ensures a clean shutdown when closing the program.
## Initializing Library
```c++
WSADATA wsaData; // Structure to store information about the Windows Sockets implementation

// Initialize Winsock
int result = WSAStartup(MAKEWORD(2, 2), &wsaData);
if (result != 0) 
{
    std::cerr << "WSAStartup failed with error: " << result << std::endl;
    return 1;
}
```
## Attempting to connect to server
```c++
memset(&serverAddress.sin_zero, 0, 8);
serverAddress.sin_family = AF_INET;
serverAddress.sin_port = htons(connecting_port);

const char* ipAddress = "127.0.0.1";
int res = inet_pton(AF_INET, ipAddress, &serverAddress.sin_addr);
if (res != 1 &&
    LockstepConnectingState_HandleSocketError("that's not a valid address! "))
{
    return;
}

if ((connect(connecting_socket, (sockaddr*)&serverAddress, sizeof(serverAddress)) == SOCKET_ERROR) &&
    LockstepConnectingState_HandleSocketError("Connect failed: "))
{
    return;
}

// send a buffer containing the word "Lockstep" to the server
const char* msg = "Lockstep";
int msgSize = static_cast<int>(strlen(msg));
res = send(connecting_socket, msg, msgSize, 0);
if ((res == SOCKET_ERROR) &&
    LockstepConnectingState_HandleSocketError("Failed to send message "))
{
    return;
}
```
When a new player enters the game it attempts to connect to local host, if it fails to do so, because there is no host player yet, it becomes the host and waits for the next player to connect to start the game.

## Hosting 
```c++
const int bufferSize = 1024;
char buffer[bufferSize];
int hostSize = static_cast<int>(sizeof(hostAddress));
int res = recvfrom(hosting_socket, buffer, bufferSize, 0, (SOCKADDR*)&hostAddress, &hostSize);
    
// if any bytes are received (don't bother to parse):
if (res > 0)
{
    std::cout << "Received a message from a potential player, acknowledging..." << std::endl;

    // set the hosting socket to reference the address the message was received from
    // -- the same API we used in LockstepConnectingState - the one that lets us use send/recv with a UDP socket...
    if ((connect(hosting_socket, (sockaddr*)&hostAddress, sizeof(hostAddress)) == SOCKET_ERROR) &&
            LockstepHostingState_HandleSocketError("Failed to bind hosting socket"))
    {
        return;
    }

    // send an acknowledgement message, "LetUsBegin"
    const char* msg = "LetUsBegin";
    int msgSize = strlen(msg);
    int sendResult = send(hosting_socket, msg, msgSize, 0);

    if ((sendResult == SOCKET_ERROR) &&
        LockstepHostingState_HandleSocketError("Failed to send acknowledgement"))
    {
        return;
    }
    
    // -- move on to lockstep gameplay, using the hosting socket, in host mode
    std::cout << "Successfully hosting a game on port " << hosting_port << " with another user, moving on to gameplay..." << std::endl;
    PlayGame(new LockstepGame(hosting_socket, true));
    return;
}
```
It waits for a message from the second player. When received it sends an acknowledgement message and it starts the game.
## Demo

{% 
    include embed/youtube.html id='5fIG7RoLS7c' 
%}