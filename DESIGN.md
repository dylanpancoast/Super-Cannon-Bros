Dylan Pancoast
CS50
08/07/2021

Super Cannon Bro

Beginnings:
When choosing a project I had a few ideas but I inevitably went with a video game of some kind.
At first, I had planned to learn Go and code Conway's Game of Life and some board games (think Checkers),
but I was worried that this would take too long and instead searched the "final projects" page of a previous CS50 class.
From here, I learned about Godot, watched a few videos about what Godot was and how it is used, and decided to make a game using the Godot engine.
Once I had downloaded Godot I began to watch tutorial videos on how to use it - the platform is relatively simple so I didn't need too many videos to get the hang of things.
After a while I settled on recreating Level 1-1 of Super Mario Bros (SMB, henceforth) and giving it a twist.
At first the twist was to add Link from Zelda: A Link to the Past to the game in lieu of Mario, but I found that using a top-down 2D character in a platform 2D world looked garbage,
so I settled for using Samus from Super Metroid.

Spritesheets/Animations/Background:
The aesthetics of the game were trivial for the goal of this project, so I will be brief here.
I downloaded spritesheets for Samus animations, SMB worlds, a random cannonball and explosion animation, and a few backgrounds.
I opened a webpage of the Level 1-1 map from SMB and created the environment that Samus would be playing in.
After creating the player/enemies, I also populated the game scene with their placements from the Level 1-1 map of SMB.
Lastly, I added an extra area to the map with the goal of recreating a more Metroid-y feel.
Once I realized where this project was going, I inserted a Rick and Morty-like portal to connect the extra area to the basic Level 1-1 map of SMB.
This didn't come until much later when I thought about randomly spawning enemies into the player's window.

Characters/Projectiles:
This step is what took the majority of my time.
Once I had a skeleton for Samus, Goombas, Koopas, and Koopa Shells, I started to code their actions and interactions.
First I had to code the physics of the game which all players would be subject to; I called this script Actors.gd
    Actors:
    In Actors, I had to define some basics: the speed a character would move at by default, the normal vector of the ground, gravity, vector velocity, etc.
Then I had to code Samus' movements/interactions; I called this script Player.gd.
I should note that by no means did I code all of this in one go.
Rather, I revisited Player.gd SEVERAL times over the last week adding new features and tweaking old ones to be compatible with new ones.
    Player (Player.gd script):
        -- Actions --
        I created functions for Samus' direction and velocity.
        This allowed me to control how Samus would move in the 2D plane once I mapped user input (henceforth, UI).
        I coded movement by connecting UI to the direction and velocity functions.
        I coded armcannon operation via UI.
        I coded animations of movement and armcannon operations.
        Notably, the armcannon operations had to be tied to the Cannonball.gd script so as to instance the physics of the Cannonball whenever it was created via UI.
        -- Death --
        I created a function to kill Samus whenever he was touched by an enemy.
        -- Teleporting --
        I linked signals from various graphical components of the modified Level 1-1 map of SMB to the Player.gd script to code the teleportation features.
        I also linked the crouch UI to the teleporters used in a typical SMB game (upright pipes)
        -- Enemy Spawning --
        This lengthy function was added to the Player.gd script so that enemies would randomly spawn only in the Player's window (otherwise, enemies could spawn in an irrelevant spot of the map)
        A safe zone was added around the center of the camera to prevent enemies from spawning onto (or within) Samus.
        A feature was added to prevent enemies which are instanced within a rigid object (like the floor or a wall) from staying instanced and taking up memory.
        Note that, unfortunately, I was unable to optimize this code to spawn enemies on the edges of the window.
Enemies:
    Goomba (Goomba.gd script):
        -- Actions --
        I inserted into the physics process function (included with the node used for all characters) a patrolling function such that Goombas would act similarly to how they act in SMB
        Since all Goombas are met as Samus walks right, all Goombas were started with leftward movement unless instanced by the Enemy Spawning function in the Player.gd script.
        If instanced into the scene via the Player.gd script, enemies are started with movement that is directed at the Player initially.
        Animated
        -- Death --
        I created a function to kill Goombas whenever they were touched by a Cannonball
        I created a funciton to stop Goomba movement whenever they move outside of the Player's window.
        This was done to optimize memory-usage (who cares where a Goomba is moving if it isn't in your frame of view?)
    Koopa (Koopa.gd script):
        -- Actions --
        Copied from Goomba
        Additional animation added (Koopas are clearly facing one way or another while walking left or right, so I had to account for that).
        -- Death --
        I created a funciton to instance a Koopa Shell (a retreated Koopa) when touched by a Cannonball
    Koopa Shell (Koopa_Shell.gd script):
        -- Action --
        I created a function to change movements speed based on which side fo the shell is hit by a Cannonball.
        If a shell were hit by a Cannonball while moving, it stops
        If a shell were hit be a Cannonball while stopped, it moves away from the blast.
        -- Death --
        I created a function to kill a Koopa Shell if it is stomped by Samus and which doesn't kill Samus in the process of him stomping it
    Bowser (Bowser.gd script):
        -- Action --
        Copied from Koopa
        Note that, unfortunately, I was unable to correctly code fireball projectiles from Bowser's mouth.
        -- Death --
        Copied from Goomba.
        Note that, unfortunately, I was unable to correctly code the concept of "Hit Points" for Bowser.
        My plan grew to include Bowsers who had hit points which would be reduced each time a cannonball struck, requiring some additional skill in the game.
Projectiles:
    Cannonball (Cannonball.gd script):
        -- Action --
        I coded velocity to cause Cannonball to move away from Samus when instanced into the game - so he is "shooting it" from his armcannon.
        -- Death --
        I created a funciton to "kill" the Cannonball when it hits an enemy or a solid surface (wall/floor)
        I created a function to "kill" a Cannonball when it leaves Samus' window.


Thanks for reading this and apologies if the late delivery of this has caused you any inconvenience. All my best for the rest of your summer!