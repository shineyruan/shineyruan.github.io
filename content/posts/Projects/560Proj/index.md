---
title: Minecraft Game Programming
date: 2020-12-09T18:30:11-04:00
tags: ["computer graphics", "Multi-lingual"]
categories: ["Projects"]
theme: Toha
hero: /images/posts/560Proj/560-screenshot.png
menu:
  sidebar:
    name: Mini Minecraft
    identifier: projects-cis560-project
    parent: projects
    weight: 10
---

In the fall of 2020, I finally had a chance to explore the field of computer graphics at University of Pennsylvania, and did a very interesting course project which builds a simplified Minecraft from scratch. This game is built on OpenGL 3.2 with Qt 5.15.0. Through building this game, I learned a lot about OpenGL rendering pipeline, game engine & texture mapping.

## Demo Video

{{<youtube l_YViGj6_Qc>}}

<!-- more -->

## Overview

Minecraft is originally well-known for its block-like structure. Everything is made of blocks, including mountains, trees, grasses, people, cows, zombies, etc. Although it might seem a little bit weird, this actually creates a really amazing, retro vibe as if you were playing those good old, low-resolution video games in the 70s and 80s. What's more, I personally think that using different types of blocks to create a huge variety of objects from scratch is one of the essence of Minecraft. It is exactly because of the set-up of blocks that players can easily make things on their own.

{{<figure src="minecraft.png" caption="Minecraft (Java edition) main menu.">}}

### Blocks

Minecraft is a game only built upon `Block`s. Each block is defined to be a 1x1 cube in 3D space. All `Block`s can only be arranged adjacent to each other without any translation or rotation. Now you may wonder: aren't there items such as leaves, torches, ladders, sticks in Minecraft that don't look like a cube? Well, yes, but these are considered to be *specially-shaped blocks*. There are still many *regular-shaped blocks* such as stones, dirt, ice that look exactly like a 3D cube. Those specially-shaped blocks are **only treated in their special shapes when rendering game graphics,** but when it comes to the deep low-level gaming logic, such as finding the state of the block (occupied/unoccupied) or the center position of the block, etc., these specially-shaped blocks are treated just like the regular ones!

Minecraft has lots of different types of blocks. Passionate Minecraft gamers also keeps adding new types of blocks to the original game to make it more playable. For my own Minecraft game, I have currently only created the following types of blocks:

Regular blocks (3D cubic shape):

- `CLOUD`
- `GRASS`
- `DIRT`
- `STONE`
- `SNOW` (actually corresponds to snow blocks in original Minecraft)
- `ICE`
- `REDSTONE_LAMP`
- ...

Specially-shaped blocks:

- `REDSTONE_TORCH`
- `LEVER`
- `REDSTONE_WIRE`
- ...

In additional to the regular biomes such as grasslands, mountains, forests in original Minecraft, we also created a new biome called *Heaven*. Heaven consists of `CLOUD` as the ground, and we used `QUARTZ`, `GOLD` and `QUARTZ_BRICK` to build a very large temple for God in Heaven. For details, please see our demo video!

### Chunks

Minecraft has a very huge map. It is impossible for the game to load all the maps into PC memory every time on start-up. Hence, it is critical for Minecraft to come up with a plan to only load the surrounding map at the player's location. In fact, Minecraft actually divides the entire map into `Chunks`, where each `Chunk` consists of 16x256x16 blocks (*x, y, z* direction, respectively; *y* is the direction pointing "upwards" *i.e.,* to the sky). Upon rendering, each `Chunk` would be the **smallest unit for rendering.** This mean that Minecraft doesn't render the surface of all blocks one by one. Instead, only rendering the surface (the boundary between an opaque block (`STONE`, `DIRT`, `SNOW`, etc.) and a transparent block (`EMPTY`, `WATER`, `ICE`, etc.)) of an entire `Chunk` would be much more efficient and make the game more responsive.

### Terrains, Terrain Generation Zones

When it comes to map loading, it is easy to discover that only loading one chunk (16x16 blocks in *x, z* direction) at a time reveals way too little information of the player's surroundings. A simple solution is that we can group `Chunk`s together and form `Terrain`s. As a result, each `Terrain` is defined to have 4x4 `Chunk`s in the *x*, *z* direction (64x64 blocks; 64x256x64 blocks for *x*, *y*, *z*). Furthermore, 5x5 `Terrain`s form a *terrain generation zone.* In this way, we can now always **keep loading the terrain generation zone centered at the chunk where player is standing,** which means we load 20x20 `Chunks` surrounding the player. Once the player moves off a `Chunk`, we keep loading new neighboring `Chunks` to the PC memory and remove those chunks that are no longer considered to be in the new terrain generation zone from the PC memory. By such means we maintain a constant amount of memory usage throughout the game while keeping the player well informed of the surrounding terrain.

*To be continued...*
