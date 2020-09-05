---
title: "Doppelganger"
date: 2020-09-02T09:11:33+07:00
toc: true
tags: [Unity, NSC2018, Custom Editor]
weight: 12
draft: false
---
![Doppelganger](/dg-cover.png)

game: https://csaratakij.itch.io/doppelganger \
walkthrough: https://www.youtube.com/watch?v=t01DpSmG0PQ \
respository: https://github.com/CSaratakij/DG-Script-Only

responsible: game system

other member: \
[Nutthan Lekprasan](mailto:nutthanlekprasan@outlook.co.th) (game designer) \
[Jetniphat Likhitwatthanasakun](mailto:jetniphatoat@gmail.com) (artist)

## Introduction
Before you read any further, I have to warn you. There are some __spoiler__ of the puzzle in this game. Please play the game first to avoid any spoiler.

This game is the entry for __The Twentieth National Software Contest: NSC 2018__.
We manage to get the __2nd runner up__ in ___"Program for entertainment"___ .

We have about 3 months to finish this project.

Since I mainly do a game system and some of the game design, I will cover these topics with in depth. However, I will leave the art aspect to the 
artist himself.

## Why this game call "Doppelganger"
If you already play the game, you will notice that there isn't a single doppelganger in this game.
We name this game __"Doppelganger"__ at first because we want the game to do something about a clone of our player character.

Turns out, none of the things we discuss in the early stage made it into the final product.
Most of the things in the game come from a first month we made doing a rappid prototype and play testing a lot.

But this is the entry for the contest, We cannot change our name mid-way due to the proposal we sent during the first round of the competition. So we kinda have to live with that.

## The origin of the "Focus" ability
![focus ability](/dg-focus.png)

Once we decide this is gonna be a 2d side scroller, I finish implementing the player controller shortly after that.
But there is no core gameplay loop yet. So I have to experiment something quick.

During that time, I saw a game that playing with a clone of player concept like our early idea.
I don't remember the exact name of this game, but the mechanic is very similar to __"Binary Land"__.

That's when I realized, I actually didn't think about this enough. So I start looking for the inspiration in [itch.io](https://itch.io).

After playing a bunch of weird game, One of the idea come up to my mind.

I remember playing some space shooter game that have a wrapping machanic.
If you go near the end of the screen, you will wrap yourself back to the opposite side of the screen you came from.

I think this is quite interesting, what if a 2d side scroller can do that? \
So, I do a quick prototype and make our play testing.

We kinda have a short discussion about how this can work with our doppelganger idea.

Then one of my team notice the similarity in the game that already publish.
That game is [Four Sided Fantasy](https://store.steampowered.com/app/337490/Four_Sided_Fantasy/) (__Damn__...It's hard to be original these day).

Actually, I don't mind if this game is similar to that game and I really like this mechanic.
But I have to keep exploring this deeper to see the other possibility, What cools thing we can do, if player have an ability to wrap themself?

One of the thing I remember experiment with was...

" What if the world wrapping box place outside of the player and it isn't the thing that player can control and it just sit there in the level? " \
{{< figure src="/dg-no-cake.jpg" caption="( Sorry, no cake for you )" >}}

There is other problem as well, the gating problem.

__Gating__ is how the game limit the player progression, in this case is how to prevent player to bypass our obstacle with ease.

The normal obstacle in the 2d side scroller is the height of the wall. But the world wrapping mechanic is really an over power ability.
It can bypass this type of obstacle easily and making it harder to design the game level around this.

This is especially true if the world wrapping box size itself fit to the entire screen.
So, I decide to just nerf this ability.

Instead of making it fit to the whole screen, Why not shrink it down to the smaller size and just place those world wrapping box at the center of the player.

I make a play testing again, and give this prototype to our game designer.

I let him come up with the combination of the level with this over power ability in mind. (a lot of his levels appear in the final product as well)

Once I tested, I quickly realized...

"You know what?, this __world wrapping mechanic__ can make the typical puzzle in __puzzle platformer__ much more __interesting__."

So I decide to combine puzzle platformer with the world wrapping mechanic (focus), and it turns out to be fun.

Now we have a core gameplay loop.

A short term goals of the player is to figure out how to keep progressing further by solving the well place obstacle with a focus ability.

At the end, our entire month was gone. But we have a strong starting point and an interesting mechanic worth exploring.

## Things that I implemented
This is the technical aspect of the project and it's really long.
If you aren't interested in this topic, you can skip to [How we design a game pacing](#how-we-design-a-game-pacing) .

### Game System
#### Player Controller (don't forget about one way collision, wall jump?, how it move during moving platform, push pull box, footstep)
![player controller](/dg-player-controller.png)

(jumping, physic based. Fighting with physics)

(check ground by overlap circle) (avoid alloc every fix time step by pre-alloc)

(wall jump, ray cast to check wall with the same direction of the player
facing, allow player to jump up about 63 degree)

(one way collision by unity itself)

(push pull box, overlap box (trigger area) to check player). Move by set its velocity)

(footstep by cast down to the ground, see if there is a ground tag and
play sound effect appropiate)

#### Camera System (Camera Trigger (avoid blind jump in platformer & cinematic cutscene trigger), game cinematic)
__Game Cinematic:__(?)
(explain about this section)

#### World Wrapping Mechanic (Focus)(don't forget about the sprite mask, box effect by focus)
(This one is the hardest to implement, took me sometime to figure out.)
#### GamePad support (XInput)
#### Game Progress

#### Door & Switch
![door and switch](/dg-door-and-switch.png)

#### Moving Platform
#### Collectable
#### Loading Screen
#### Game UI
(TODO) 
Unity Canvas do the jobs nicely

(TODO) 
I avoid Unity Event and its messaging system since it's slower than manually binding a native C# event.
( Even though it's less convenient to setup )

### Custom Editor
Unity have a way to extend an editor to suit our project need. \
We definitely take advantage of this.

These are the tools I built to help reduce our development time. \
We cannot hope to quickly iterate our levels and finish this project without these tools.

#### Save ID Generator
![save id generator](/dg-save-id-generator.png)

This tools help ensuring our checkpoint will have a unique ID.

Because we iterate a lot of levels in a short period of time, we keep adding, changing its location and removing the
checkpoint overtime.

Without this tools, we have to keep checking every checkpoint we place manually to make sure its ID will not conflict with each other. \
(Which is really the time consuming process)

With this tools, we can check and re-generate every checkpoint ID in one click. This help ensuring our save and load system to work properly.

One of the problem I came across is how to make Unity save the
change we made programmatically.

To make this tools work, I have to push our change to the Unity undo stack.

Once our change has pushed, It will set the Unity scene dirty and allow us
to save the scene. (I learn this the hard way...)

You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/SaveSetting.cs) .

#### Collision & Collider Plotter
![collision plotter](/dg-collision-plotter.png)

This tools help us place and delete our collision and collider quickly with ease.

We need this tools because of the problem about the way Unity resolve its collision.

Take a look at this example.

![sprite have its own collision](/dg-sprite-each-collision.png)

As you can see, each of the sprite has its own box collider.

The problem here is that, player will get stuck at some point when they try to walk on this type of the collider setup.

Despite how it look, a bunch of smaller box collider cannot provide a seemless ground surface.

> As far as the physics engine concern, 
> (TODO) (explain how physics engine resolution order )

Physics engine will resolve

So in the player perspective, the player might be able to walk in the right direction.

But as soon as they start walking in the left direction, their character might get stuck.

The ideas box collider setup would be a big box collider to make physics engine resolve collision correctly.

![one big box collider](/dg-sprite-one-big-collision.png)

The box collider size also adjust to the same sprite size when you add its component to the sprite itself.

So we have to seperate the collider from the sprite by making a new game object with the box collider.

Placing this ideas box collider with an existing Unity tools is really time consuming.

Not only we have to keep our box collider size right, we have to place this in the proper distance (make it snap to the grid).

The way I do this is using the fact that I can construct a rectangle by using only two points.
[image about two point the construct rectangle here]

[image about drawing here]
[image when in delete mode here]

As for the grid, I don't have to re-draw it since we use a Unity default scene view grid.

This tools also help us placing the necessary collider to represent the ground material to help playing the right sound of player footstep.

You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/Plotter/CollisionPlotter.cs) .

#### Sprite Plotter
This is the tools \
(todo) The placing sprite logic is similar to the Collision & Collider Plotter.

You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/Plotter/SpritePlotter.cs) .

#### Scene Selector
This is the tools \
you can view the implementation [here](https://github.com/CSaratakij/SceneSelector) .

## How we design a game pacing
(the overall design of phase and the chart of difficulty & the period of the phase)
(don't forget to warn about spoiler alert in our game pacing design, how you teach player on spot of how focus works and how box puzzle works)
(how you teach player to grab & push box)

Once we finished interating a lot of levels and decide which level to keep in the game, 
we connect all the levels based on their difficulty and start play testing from the beginning of the game.

Not only to estimate how much time player need to complete this game, but also wanted to see how each level feels when connect to each other.

We re-arrange the order of each level if it necessary and take a note when player starting to feels repetitive.

Then we introduce the new ability or some shift in the game mechanic when player feels repetitive the most.

{{< figure src="/dg-pacing.jpg" caption="( This image is a rough approximate of how this game pacing works )" >}}

I strongly disagree about front-load tutorial, so the first two phase of the
game was crafted with the extra care to make sure that I introduce player to the game properly.

### Beginning Phase
(familar with the input & how the player controller works)
(1. open space to the right guide player to moving to the right)
(2. height wall to force player to press a longer jump)
(3. Cat remind player to go to the right, make this a long term goal)

### Tutorial Phase
(introduce to the main mechanic, the focus and make a box to open the
door. keep occasionally remind player about how focus works and what the
limit it is, how spacing of the player impact how focus works)
(1. Teach about box and remind about long jump)
(2. Teach about focus)
(3. Teach about the limit of the focus ability)

### Increase difficulty to the highest
(throw more stuff to the player, introduce new ability along the way, make
player do some timing required and throw the most difficulty puzzle to the
player in which player need to master this mechanic to solve it properly)
(1. Make player play with a timing)
(2. Introduce the new ability (Move mode))
(3. Box effect by focus)
(4. Introduce the new ability (Edit mode))
(5. Present with the highest difficulty)

### Slowly Decrease difficulty to the end
(slowly decrease difficulty after the hardest difficulty puzzle, and
sudden drop in difficulty as the player reach to the final level)
(1. Box move along the moving platform)
(2. mundane puzzle as the final level)

(TODO)
So, at the end....It turns out to be....
Can be finish in 10 minutes, if you know how to solve its puzzle. \
not too long and too short.

## Getting the most out of final round
In the final round, we have only 10 days left to complete this project. \
So we decide to make the most out of it.

One issue that keep ourself to not work as fast as we would like to be is how we keep forgetting how much work left for us to do.

And there isn't a way to tell each person progress, unless we ask each other.

That's why we spent a whole day to just listing all of our work and adapt a kanban board.

Our kanban board rules are simple. \
We have __Backlog__, __Todo__, __In-Progress__ and __Done__.

__Backlog:__ is for the task that we need to complete, but it isn't the main focus right now.

__Todo:__ is for the task that we currently focus. You can put 3 tasks at the maximum into this section.

__In-Progress:__ is for the task that currently working on. You can put only 1 task at the time into this section.
And you have to remove the task from this section if you stop or finish
the task.

__Done:__ is for the complete task.

With this board, we can finish most of the necessary task. \
You can view the board [here](https://trello.com/b/hVPRrx2p/dg-frame) .

## Conclusion
(TODO) Shout out to my team, This project won't be possible without them.

