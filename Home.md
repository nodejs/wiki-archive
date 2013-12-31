Welcome to the Wolf wiki! This wiki is a technical documentation for developers and testers, that describes a structure of the engine, rationalization of different hints and gives an API for extension of the engine.

Wolf engine consists of two parts: web client and server.

## Web client

Web client allows to iteract with game servers via Web-browser. It visualises a world, received from the server and sends user input back to the server.

It is written on Dart, flexible object-oriented JavaScript analog. But it can be easily ported to JavaScript using Dart SDK.

**Related pages:**

* [[Architecture]]
* [[Network iteraction]]
    * [[Client-server iteraction protocol]]
* [[Visualisation]]
* [[Sounds and audio]]

**API:**

* [[Viewport API]]
* [[Renderable API]]

## Game server
Game server is a core of the game. It processes all game logic, physics, game events, actions of players and so on.

Server side is not implemented yet.