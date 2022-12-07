# Practical: Prototyping VR in Unity with the HTC Vive

In the lecture, we saw how contemporary virtual reality headsets work from a visual and audio perspective. We also saw a number of “locomotion techniques” that allow us to move a player around a virtual world that they are experiencing using VR.

In this practical, we are going to learn how to get started with prototyping VR experiences using the HTC Vive in Unity. We will see how to use Vive's inbuilt implementation of locomotion and, then, learn how both Vive's interaction system works by having a go at implementing our own locomotion techniques from scratch!

You should work in groups of 2 around the VR PCs. When deploying your VR experiences, make sure that another team member ‘spots’ the user to make sure they don’t bump or trip into anything or fellow students. 

## Task 1: Your First Virtual Reality Project in Unity

We’ll be using Valve’s SteamVR Unity plugin to prototype for HTC Vive in this course. To help you get stared quickly, I've created a template project with everything installed and set up. You can access this project by making a copy of this repository in your personal git account by pressing the ```Use This Template``` button at the top right of this page, and then cloning your personal version of the respository onto the machine you're working on.

> **Note** As you're working as a group, you might want to push your code to multiple personal git repos at the end of the practical so you all have a copy. If you'd like to know how to do this, just ask one of the teaching staff and we'll show you.

Once you've opened the project, open up the ```Island Scene``` in the ```Scenes``` folder. You'll see a pop-up asking you to confirm some settings. Click ```Accept All```. Next, run the scene. You should find yourself in VR on the island! You should be able to look around and see two gloved hands where the controllers are held.

> ***Warning*** If the scene doesn't run, you might need to open the SteamVR Input Window (found under ```Window > SteamVRInput``` in the menu) and press the ```Save and Generate``` button. 

Once you’ve all had a go in your first VR experience, leave the matrix and look at the hierarchy of game objects that were created when you dragged the Player prefab into the hierarchy. See if your group can work out which of the game objects does what, and how they relate to the functionality of a VR headset that we learned about in the lecture by completing the following simple tasks:

- Make a floating sphere appear in the center of the user’s vision, no matter what direction they look
- Make a red cube appear in the user’s left hand and a blue cube in their right hand

## Task 2: Using Steam's Inbuilt Teleporter

As teleporting is such a common interaction in VR these days, SteamVR includes a really nice implementation that you can use in your scenes. To get started with teleporting in your scene all you need to do is drag the "Teleporting" prefab into the root of your scene's hierarchy. You can find it in the ```/Assets/SteamVR/InteractionSystem/Teleporting/Prefabs``` folder within the project.

Once you've added the teleporting prefab into the scene, you should be able to attempt to teleport by pressing the touchpad on the top of the controller. However, you'll notice that you can't yet teleport anywhere. This is because we need to tell Unity where you're allowed to teleport (where people are allowed to go is, of course, an important thing you want to control as a designer!). SteamVR let's us define where people can teleport in two ways.

### Approach 1: Teleport Points
