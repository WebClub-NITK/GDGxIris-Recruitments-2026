## Task ID: BGMIClone

#### `Unity`, `Godot`

Mentors: [Ritvik Gampa](https://github.com/Ritvik-17) ([+91 8985003940](https://wa.me/918985003940))

Difficulty: `Hard`

## Overview

Your task is to build a clone for the game BGMI, it doesn’t have to be an exact replica, focus more on the concepts used to build the game and it’s fine you generalize functionality with other FPS video games.

This task is to introduce you to the fundamental concepts of game development like Camera movement, User Input, Player Movement, Physics Engine, Collision Detection, Ray Casting,  Game State, Inventory Management, Audio Management and Graphics & 3D Rendering.

Remember multiplayer is a bonus task and will be regarded as “expert” task if you add it as well. Although it will be impressive, do not worry about the fancier parts like latency and graphics give your best to build the mechanics well.

You can use any public assets available as well for the graphics part focus on this once you build the mechanics.

I recommend you instead use Unity or Godot

## Required Features

Sections **1, 2, 3, 4, 6, 7, and 8** are the Hard submission. Section **5** is optional. Section **9** makes the submission Expert.

You do not need to copy a commercial game's name, logo, or assets. Public assets are fine when you credit them. The mechanics below are the task.

You are more than welcomed to add more features to the game and customize however you want, but following features list is the recommended way to prioritize

### 1. Player / Character
- Third person character movement, Walking, Sprinting, Crouching, Jumping.
- Player Health, Fall Damage, Fall Physics, Healing, Player Death
- `Bonus: Prone, Armor`

### 2. Weapons & Shooting

Weapon pickup, Weapon switching, Primary/secondary weapons, Shooting, Reloading, Magazine/ammunition management, Bullet/Ray Cast mechanics, Damage calculation

`Bonus: Different weapon types (Assault Rifles, SMG’s, Shotguns & etc.), Recoil and Bullet Spread, Grenades / throwable weapons.`

### 3. Inventory & Loot

Inventory system, Item pickup, Item dropping, Healing items, Inventory UI, Ammunition, Weapons

`Bonus: Loot rarity, Backpack capacity, Armor`

### 4. Map / Environment

Mini Map, 3D terrain, Obstacles, Trees/vegetation

`Bonus: Buildings, Roads, Bridges`

### 5. Battle Royale / Match System (BONUS)

Player spawning, Match timer, Player elimination, Last-player-standing condition, Winner detection, Match end screen, Restart/new match

### 6. UI / HUD

Health bar, Kills, and Crosshair are required. Match timer is required only when section 5 is implemented.

### 7. AI / Bots

Enemy spawning, AI navigation, Detecting player, Chasing player, Shooting player

### 8. SFX & Sound

Player animations, Weapon animations, Muzzle flash, Shooting SFX, Weapon firing sounds, Reload sounds, Footsteps

### 9. Multiplayer **(Expert Task)**

Note: This task becomes and expert task when you implement this. Have a server which stores the game state and in real time update the game state, any number of players can join a match at any point of time (to make this simpler you can do it this way). Also follow Client-Server Architecture i.e. The server should be the authoritative source of truth. Clients send inputs/actions such as movement or shooting, and the server updates the game state and synchronizes relevant state back to the clients.

## Deployment

Ship a playable build. A web export, an itch.io page, or an APK is enough. The GitHub repo stays private. Localhost-only is not enough.

## Resources

Go through the official documentations of the game engine you will be using ([Unity Engine](https://docs.unity.com/en-us), [Godot Engine](https://docs.godotengine.org/en/stable/))
