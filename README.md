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

You can define a set of positions on the map that your users are allowed to teleport to. To do this, you simply add a set of ```TeleportPoint``` prefabs to the scene. You can find these in the same folder that the Teleporter prefab was in. In your group, see if you can create a path of teleport points that lets your player navigate to the concrete platform that protrudes from one of the hills on the island.

If your teleporter can't reach the points, you can adjust the ```Arc Distance``` variable in the Teleport game object. However, be careful about making the distance too big, as aiming a long teleport arc gets difficult.

### Approach 2: Teleport Area

Sometimes you just want your player to be able to roam freely around your level, or at least a part of it. In order to do this, you can create a piece of geometry, such as a simple plane that overlays the floor of your level, and define it as a ```Teleport Area```. To do this, you simply:

1. Add a new Plane to your scene ```GameObject > 3D Object > Plane```
2. Position it in the location where you'd like the user to be able to teleport around
3. Add the ```TeleportArea``` prefab to it, which can be found in ```SteamVR/InteractionSystem/Teleport/Scripts```

Have a going at doing this, by creating a teleport area that lets the user explore the concrete platform.

Unfortunately, the current SteamVR implementation doesn't play nicely with Unity's inbuilt terrain. That's to say, you can't just stick a TeleportArea on your terrain and go exploring. Therefore, if you want to use the inbuilt teleporter with terrain you've got two options:

- Define paths of teleport points for when the user explores a terrain area
- Create a set of planar teleport areas that approximately follows the shape of your terrain (or an allowed path through it)
