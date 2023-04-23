---
Tags: 
Created: 2023-04-22 20:37:22
---
(Links:: [[Computer Networks]])

The game is heavily inspired by the online multiplayer game www.diep.io. It works as follows: each player controls a tank from a top-down perspective on a two dimensional plane of fixed size. The players can navigate the game's battlefield and competes in a battle royale type of game against other players tanks.
At the start of the game, players are given a basic tank which they can upgrade as they earn experience points by destroying enemy tanks and obstacles.
The upgrades include the following abilities:
- Bullet damage
- Player Health
- Health regeneration
- Movement speed
- Bullet speed

Characters can move in all 4 directions using the arrow keys, aim with the mouse pointer and shoot by clicking the mouse. A global scoreboard is kept of player scores (points).
The last player (or maybe even team) wins the game.

The game will use dead-reckoning and UDP. 
Each client handles the command ordering, drop detection and resending. 
Frames are calculated in advance in the case that a frame hast to be retransmitted. Each frame header contains a sequence number and execution time to identify them. When acknowledgements are received too late, the same frame and sequence number will be retransmitted (the sender having assumed that the frame hadn't been transmitted).
The path of each bullet is easily determined by it's coordinates and velocity. These values will be passed on through the network to other clients where the same dead-reckoning algorithm will determine it's path. Player tanks will also use dead-reckoning to "predict" their path and the direction that they are pointing in, updating the player position and direction once the changes are too noticable. Lagging movements of the player tanks can be mininmized by slowly accelerating and decelerating the tank.



---
References: