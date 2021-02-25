---
title: Comm's Cafe - Highfield Hack
date: 2021-02-15 21:35:21
tags:
    - 3D Design
    - Hackathon
---

### Inspiration
During a pandemic with an ever shrinking variety of social interactions, a space that allows you to safely meet with your friends, spend time and enjoy yourself is well needed. We aim to blend the best of Discord and YouTube to give users an area to kick back and relax as they would when watching a movie or just spending time and talking.

### What it does
Comm's Cafe provides an online space that allows users to meet and join conversations in an authentic way to real world contact. The coffee shop has tables of varying sizes that can accommodate two, four or six users. This emulates a real café with the limited space.

### How we built it
The Comm's Cafe's player lobby was built in Unity game engine with free (and pre-purchased) low poly assets. This provided assets were combined and arranged to create a standard coffee shop with an adjoining home theatre.

The networking was built with mirror that uses a KCP connection over the network, to allow voice and text communications on the coffee shop tables. To implement the home theatre YouTube DL was used. YouTube DL pulls the raw URL of YouTube video and uses that to display it on a render texture in the game space.

### Challenges we ran into
We experienced issues implementing the network syncing. We struggled to sync the player models in the game space without experiencing large amounts of lag. RPC was used to broadcast between clients overcoming the networking issues faces.

The greatest challenge that our team faced was time. With only two members in the team, and with pre-existing arrangements to meet, we simply found that we did have enough person-hours to complete all the features that we aspired to include. If we were to do this again we would benefit greatly from an additional team member.

### Accomplishments that we're proud of
We are proud to have completed a online multiplayer game space experience in 48 hrs. This is a n impressive accomplishment even using existing network interfaces, game engine and assets.

We worked succinctly as a team. Although working on separate features at a time and remotely updating the project using Unity Hub's syncing feature. We found that neither our personalities nor our project implantation clashed at any point during the project.

### What we learned
From this project we have realised the importance of appropriately limiting the scope of a project or exercise. Given a completely free 48hrs this project would have been an achievable scale of project. However, owing to the existing arrangements that we both we bit off more than we could chew with this project.

Compromises had to be made implementing this project in the time scale. For instance, the implementation of the YouTube DL features had to be vastly simplified to be usable in this project.

### What's next for the Comm's Cafe
To further elaborate this project we will add minigames to the lobby for waiting players. These minigames will be of an arcade style and may include games like Pong and Snake.

#### Original post: https://devpost.com/software/cafe