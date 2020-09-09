---
title: "Doppelganger"
date: 2020-09-02T09:11:33+07:00
toc: true
tags: [Unity, NSC2018, Custom Editor]
weight: 12
draft: false
---
The puzzle platformer with a twist. Player can use a “Focus” power, the ability to bend a dimension to overcome the obstacle.

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
We have about __3 months__ to finish this project.

We manage to get the __2nd runner up__ in ___"Program for entertainment"___ .

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
This is the technical aspect of the project and it's quite long.
If you aren't interested in this topic, you can skip to [How we design a game pacing](#how-we-design-a-game-pacing) .

### Game System
#### Player Controller
![player controller](/dg-player-controller.png)

This is the first thing I finished implementing.

Without knowing what the game is gonna look like at first, I took an
easiest way possible to approach this by making it based on the physics engine.

In which you can call this a __"Physics based platformer"__.

As the result, I have to fight the physics engine because in reallity the character wasn't behave
in the way the normal physics would behave as the physics engine estimate.

The better ways to do the platformer in general is to not rely on the
physics engine and roll your own based on the specific need of your current project.

All the physics based platformer suffer from the floatlyness in which is
the feeling of not in control of the character.

Making the tight platformer controls by based around the physics engine is
just a brute force process of tweaking a multiple parameters of the physics engine to get the result closest to what you want.

There is still a cost to pay, because the root cause of the floatlyness is
not resolve.

All you can do is to mitigate its effect. \
Take a look at [Celeste](https://store.steampowered.com/app/504230/Celeste).

One of the game that have a tight and satify player controller in the
platformer genere.

They need to nail this stuff, because in Celeste there are a ton of
platform session that require a percise jump from the player.

This is not possible if you based it around a physics engine.

Lucky for me, this game is not really having a lot of the platform session
and it didn't require a percise jump.

This allow me to focus on the other aspect of the project without having
to re-implement the player controller.

Although, I love a tight control feeling of the platformer in general.
(Like in __Celeste__)

If I have a chance to remake this game, This will be the first thing I want to re-implement.

Now, let's talk about the implementation detail of player controller.

__Walking__

Nothing fancy here.

Just get the move direction from the input and move its character according to its move speed.

Since I use the physics engine, I need to add a physics material and low
the friction value to 0 to prevent its collider from getting stuck when character is in the air while bumping to the wall.

The footstep sound effect will play while character moving around, and its sound effect will pick based on the game object tag that found by a simple raycast.

As for the movement when staying on top of the platform, I translate character with the same velocity as the platform itself whenever
player found a platform by overlap testing around the feet of the character.

__Jumping__

There are low and high jump in this game.

This is possible by manipulate a gravity based on the way player press their jump button.

Normally, player will experience a normal gravity.

At the moment player starting to press the jump button, I will add the upward force to the character once.

After that, I still checking for the key press but it won't add the force
to the character anymore.

If player keep holding down the jump button until the character starting
to fall, player will experience the normal gravity.

But if player release their jump button before the character reach to the highest jump possible,
player will experience a higher gravity forcing the character to fall down sooner than it would normally be.

![jump curve](/dg-jump-curve.jpg)

As for the ground detection, I use overlap testing instead of the multiple
raycast.

The issue that come with this is I have to avoid unnecessary allocation at every fixed update cycle.

Because, I don't want a Garbage Collector (GC) to kick in cleaning the
stuff during gameplay making it look like a random freeze (spike) in the player perspective.

So, I have to pre-allocate, make a buffer and change the overlap testing method to the stuff that support a buffer.

The wall jumping mechanic is using a ray cast around a character feet to check if there is a wall at the same direction that player is currently
facing.

If it successfully detect the wall, player will get the additional jump at about 63 degree up.

One way collision is using a built in Unity solution.

__Push & Pull box__

![push and pull box](/dg-box-pull-push.png)

Push and pull box have two states.
(push pull box, overlap box (trigger area) to check player). Move by set its velocity)

1) 

2) 

#### Camera System (Camera Trigger (avoid blind jump in platformer & cinematic cutscene trigger), game cinematic)

(todo) A blue rectangle draw by Unity Gizmos.

![camera offset trigger](/dg-camera-offset-trigger.png)

__Game Cinematic:__(?)
(explain about this section)

#### World Wrapping Mechanic (Focus)(don't forget about the sprite mask, box effect by focus)
(This one is the most challenge things to implement, took me sometime to figure out.)

__Normal Mode__

__Move Mode__

__Edit Mode__

#### GamePad support

#### Game Progress

#### Door & Switch
![door and switch](/dg-door-and-switch.png)

#### Moving Platform
(todo) This is the small price I have to pay for using physics based platformer.

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
(Which is really time consuming process)

With this tools, we can check and re-generate every checkpoint ID in one click. This help ensuring our save and load system working properly.

One of the problem I came across is how to make Unity save the
change we made programmatically.

To make this tools work, I have to push our change to the Unity undo stack.

Once our change has pushed, It will set the Unity scene dirty and allow us
to save the scene. (I learn this the hard way...)

You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/SaveSetting.cs) .

#### Collision & Collider Plotter
![collision plotter](/dg-collision-plotter.png)

This tools help us create and delete our collision and collider quickly with ease.

We need this tools because of the problem about the way Unity resolve its collision.

Take a look at this example.

![sprite have its own collision](/dg-sprite-each-collision.png)

As you can see, each of the sprite has its own box collider.

The problem here is that, player will get stuck at some point when they try to walk on this type of the collider setup.

Despite how it look, a bunch of smaller box collider cannot provide a seemless ground surface.

Physics engine try to push away each pair of the collider when it detects the collision in each fixed update to prevent them from overlap to each other.

![player stuck](/dg-player-stuck-sample.png)

To keep things simple, let assume there are the two possibilities at the moment player collider overlap to the edge of the ground collider.

__Vertical Push__

![vertical push](/dg-push-to-vertical-arrow.png)

As you can see, the length of overlapping in the horizontal axis is greater than the vertical axis.

So, the quickest way to push each collider away from each other is by pushing them vertically.


__Horizontal Push__

![horizontal push](/dg-push-to-horizontal-arrow.png)

In this case, pushing each collider pair horizontally would be the quickest way to resolve this collision.

But this also the cause of the player getting stuck, because player keep getting push back to the opposite way of the way they want their character to be move.

For example, player might be able to walk in the right direction. But as soon as they start walking in the left direction, their character might get stuck.

The ideas box collider setup would be a big box collider to make physics engine resolve collision correctly.

![one big box collider](/dg-sprite-one-big-collision.png)

But, the box collider size also adjust to the same size of the sprite when you add its component to the sprite itself.

So we have to seperate the collider from the sprite by making an empty game object attached with the box collider component.

Placing this ideas box collider with an existing Unity tools is really time consuming.

Not only we have to keep our box collider size right, we have to place this in the proper distance (make it snap to the grid).

The way I do this is using the fact that I can construct a rectangle by using only two points.
![construct rectangle by two points](/dg-A-B.png)

And the grid itself is just the default Unity scene view grid.

<br/>
{{< youtube id="5gCgzt1DCqA" >}}

This tools also help us placing the necessary collider to represent the ground material to help playing the right sound of player footstep.

You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/Plotter/CollisionPlotter.cs) .

#### Sprite Plotter
![sprite plotter](/dg-sprite-plotter.png)

This tools help our artist to quickly place the tilesets.

Since we use a pixel art style for this game, We need to make sure each sprite will align properly within the grid.

At that time, Unity already have a tools to help place the tilesets. \
Unfortunately, We don't want to use its solution.

Not only it techically still in the beta, but also the amount of work we have to do in order to prepare our sprite is way too much for our use case.

We just want to snap our sprite to the grid. ( for god sake... )

That's why this tools was created. \
The placing sprite logic is similar to the __Collision & Collider
Plotter__.

One issue I have to deal is to prevent too much waste in the game object when placing the multiple sprites that use the same texture.

Lucky for me, I can change the sprite draw mode to tile to make texture repeat itself to fit the size of the sprite.

This help us reduce game object count to just one per multiple sprites.

<br/>
{{< youtube id="f7WA2ed93RI" >}}

Later on, I add the brush profile to help group our tilesets together.
Make it easier for the artist to choose the tilesets.

This is possible by the fact that the assets need to generate its own Global Unique Identifier (GUID) when import to the project. 

So, I just need to retreive its asset guid then save it in the [JSON](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/Plotter/Profile/dg_grid_preset.json) format. \
You can view the implementation [here](https://github.com/CSaratakij/DG-Script-Only/blob/develop/Assets/Editor/Plotter/SpritePlotter.cs) .

#### Scene Selector
![scene selector](/dg-scene-selector.png)

This tools help us play testing the game from the start without having to leave the current scene in the editor.

This feature is what I wish to be built in to the Unity Editor. \
But sadly, it's not...

So, I have to implement this myself.

Let's assume I set the scene name "Introduction" as the first scene in the build setting.

But I open the scene name "Level5" in the editor.

If I hit "Play" button in the editor, Unity by default will play a current scene.
In this case, It's a "Level5" scene.

If I want to start playing from the "Introduction" scene, I have to close
the "Level5" scene first and then have to manually open the "Introduction" scene before entering the play mode.

Which make me frustrated overtime. \
So, this tools keeping me sane while play testing dozen of times.

<br/>
{{< youtube id="PaeOfl0nNyM" >}}

You can view the implementation [here](https://github.com/CSaratakij/SceneSelector) .

## How we design a game pacing
Once we finished interating a lot of levels and decide which level to keep in the game, 
we connect all the levels based on their difficulty and start play testing from the beginning.

Not only to estimate how much time player need to complete this game, but also wanted to see how each level feels when connect to each other.

We re-arrange the order of each level if it necessary and take a note when player starting to feels repetitive.

Then we introduce the new ability or some shift in the game mechanic when player feels repetitive the most.

{{< figure src="/dg-pacing.jpg" caption="( This image is a rough approximate of how this game pacing works )" >}}

I strongly disagree about front-load tutorial, so the first two phase of the
game was crafted with extra care to make sure that I introduce player to the game properly.

The idea is simple, introduce player to the concept in the smallest fashion
rather than overwhelm player with too much information at the given time.

### Beginning Phase
The main goals of this phase is to make player familar with how to control their character.
No mind bending stuff or fancy mechanic would get introduce yet.

At first, I place the level in such a way that player will know exactly which way to proceed. (Just keep going right)

![beginning-0](/dg-0.png)

Then I make player warm up their jump by introduce a little platform.

![beginning-1](/dg-1.png)

Then, the game will play a tiny cutscene to remind player about going to the right.

![beginning-11](/dg-11.png)

Finally, make sure that player know about the high jump (hold a jump button) by placing a taller wall as an obstacle in which player need a box to support the height.

So, this introduce player about how to push and pull a box in the process.

![beginning-2](/dg-2.png)

### Tutorial Phase
This phase is to make player understand the basic principle of the main
mechanic. Especially, the focus ability.

This phase also explain the limit of the focus ability as well.

At first, I throw player in the situation where they need to use the focus
ability in order to proceed.

Not only it help player to understand what it capable of, but also how to use it to solve the puzzle as well.

![tutorial-0](/dg-3.png)

After that, I introduce its limitation right away. Player need to think
a little bit in order to proceed.

![tutorial-1](/dg-4.png)

Then, I teach player about how pressure switch works with a door.

![tutorial-2](/dg-5.png)

If player use the focus ability, it will wrap them back exactly at the top
of the switch. Introduce the pressure switch and how it work in the
process.

So, player will start to think about how useful the box would be.

![tutorial-3](/dg-6.png)

Later on, I occasionally remind player about how focus ability work.
And introduce other limitation of the focus ability.

In this case, It's about the position of the character when start using the focus ability matter.

![tutorial-4](/dg-7.png)

### Increase difficulty to the highest
After a lot of simple puzzle to solve that didn't involve the player
timing at all in the tutorial phase, things starting to shift.

At first is to play with the player timing, they need to use and disable
the focus ability at the right time in order to proceed.

Then things will slowly getting harder and weirder, slowly increasing the
difficulty to the highest possible.

Give player some new abilities to play along the way to keep the game fresh.

![difficulty-0](/dg-8.png)

And it keep getting harder and harder until the most challenge puzzle in the game.

![difficulty-1](/dg-9.png)

### Slowly Decrease difficulty to the end
After the most challenge puzzle, things will slowly decrease in the difficulty.

But the time player took from the most challenge puzzle to the final level
is not long enough and player need more time to cool their head after
those challenge.

So, I decide to just suddenly drop in difficulty in the final level.
Because in the player mind, they just got enough of the puzzle at this point.

In the end, it's coming together nicely.

The game took about 10 minutes to finish if player know how to solve those puzzles.

Not too long and not too short for the game session.

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

And the top piorities tasks will be tag with a label __"urgent"__.

With this board, we can finish most of the necessary task. \
You can view the board [here](https://trello.com/b/hVPRrx2p/dg-frame) .

## Conclusion
(TODO) Shout out to my team, This project won't be possible without them.

