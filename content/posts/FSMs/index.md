---
title: "GDevelop Enemy AI and Finite State Machines"              # The page title, shown in the browser and in listings
description: "The second post of periphery"         # Meta description for SEO and social sharing
date: 2025-07-20T11:30:03+00:00    # Publication timestamp, used for sorting and display
author: "periphery"               # Author name; can also be a list for multiple authors
showtoc: false                     # Show Table of Content (toc) at top of post. Default false

draft: true                      # Hugo: if true, page is omitted unless built with --buildDrafts
searchHidden: true               # PaperMod: omit this page from client-side search

comments: true
bluesky_post_uri: ""
bluesky_post_author: "volution.bsky.social"

cover:                            # Page cover image settings
  image: ""       # Path or URL to the cover image
  alt: ""               # Alt text for accessibility
  caption: ""               # Caption displayed under the cover
  relative: false                 # true if image is in a page bundle; false for static files
  hidden: false                   # true to hide the cover only on this page

---

## GDevelop Enemy AI and Finite State Machines

These are some general thoughts, mainly “notes to ourselves”. Some of these may admittedly show a lack of understanding of the systems in GDevelop, but they are also representing some workable practices we arrived at for our specific game and systems. 

## Working with Instances

We tried to set up a FSM for an enemy with the purpose of then dropping several instances of the enemy object into a scene and this way having several enemies acting individually, but based on the same FSM. 
We could not get this to work. Our enemies are complex. They consist of an object, that most of the logic is applied to. This object loads the sprite, that gets animation directions via the object. It also loads two different kind of hitboxes. On top of that a complex FSM is attached to the object.
Even though we carefully added “For Each Object X” layers around all code sheets, when placing two objects of an enemy in the scene, only one would even load the sprite. I am sure for simpler enemy objects having several instance would be no problem, but not with our complex enemies.
A limiting factor of the “for each” command in GDevelop is also, that “Trigger Once” commands are not usable there. Something we realized only recently and might account for a lot of the problems. At the same time, how to make a system such as an enemy AI FSM work without using “Trigger Once” is hard to imagine at this point.
If you set up your game levels as External Sheets (as opposed to separate scenes), you run into the additional problem, that you do not have the option to use the “At the beginning of the scene”-command.
One approach would be to retrofit a unique instance ID to each enemy object. But as we understand, this is not how object picking is meant to be handled in GDevelop. 

Our workaround is the following: Luckily we do not need many enemies active on screen at any given moment. The field of view into our isometric world is relative small. The fights with the enemies are also supposed to be challenging. Either they shoot you very quickly, or the fights become drawn out by taking cover, being injured and so on. Having too many enemy instances on screen would be not necessarily the game experience we are going for. 
We will always only have one enemy of a kind on screen. Upon its death it will (if so defined) respawn outside of view of the camera. As a player it appears as simply the next enemy that runs into view. We will set up a handful of enemies with different AIs. One enemy soldier will be free roaming. They will chase the player, shoot at it standing up and laying down, and evade after an attack. Another enemy soldier will be more static. It will take cover behind objects and only pop up to shoot at the player. When we have 3-4 of these separate enemy systems, there will be more than enough action on our small screen.

## State Separation

The overall biggest issue with FSMs is to keep the states truly separate from each other. Our objects have a “State” variable, that is called, when certain conditions are true. For example, if the player is less than 500px distance to the enemy, the enemy will run towards the player. If the distance is less than 300 px the enemy is supposed to stop and attack. In the moment that the enemy is closer than 300px both conditions in this example are true however. The running behavior needs to be limited to the range between 500 and 300px distance. We found that simply changing the “state” to attack in the closer range, is sadly not always enough to “trap” the object in a certain state. Sometimes this exclusionary code still needs to be written, even though it is the purpose of a FSM to avoid long lists of exclusionary conditions like that. Again - this may just show the lack of our understanding of FSMs and GDevelop at this point.

## FSM networks vs flowers

To simplify our FSMs we choose an approach that avoids a too complex network shape with many states being able to transition into each other. Instead we favor a more "flower"-like approach. A the center of the flower is a central state, usually idle, that acts as a transitional state to most of the other states. As an example: you could set up a system that allow states transitioning attack directly to walking and running and taking cover. Better would be to always transition to the idle state right after an attack and from there go to walking, running, etc. separate states you have, the more individual exit conditions you would need to write for each of those. This way we can avoid writing three exit conditions for running and need only one back to idle, that then allow to go to the others in turn. 

## FSM in one sheet?

For our player we set up an FSM with 20+ states in separate sheets. Maintaining, changing and troubleshooting this is not easy. Some things need to be kept consistent among sheets, others, like exit conditions to other states are highly individual. We easily found ourselves with countless sheets open, trying to keep everything consistent and logical.

To our understanding the loading of external sheets based on state conditions does not really help with state separation, because once more and more conditions of the object become true (from idle, to walk, to run, to aim, to shoot) - all those external sheets are also loaded and “active” with all the potential “state overlap” problems.

For one of the simpler enemies we now tried an approach to set up the whole FSM inside one sheet. The state conditions simply separate various code folders. For a simple enemy, that hinds behind a wall and pops out to shoot at the player and takes cover again, this works well. But also here we could not do it, without a few exclusionary conditions, to prevent the machine reacting the the wrong triggers.

## “When animation has finished” - nah

Maybe due to the setup of our objects carrying a separates sprite containing all the animations, we could not make this condition work at all in our FSM exit states. Maybe the sprite is not properly picked and GDevelop does not know which animation is addressed in this statement. Our workaround for this is to call an animation and then simply add a wait, to make sure the animation is played, before moving onwards to the next state. This is not beautiful, but at the moment our only solution.

We will read this in a couple of weeks or months and realize how little we understood at the time of writing this. Or we pad ourselves on the back how well we have duct taped all the issues we ran into. :)


