# Practical: Prototyping VR in Unity with the HTC Vive

In the lecture, we saw how contemporary virtual reality headsets work from a visual and audio perspective. We also saw a number of “locomotion techniques” that allow us to move a player around a virtual world that they are experiencing using VR.

In this practical, we are going to learn how to get started with prototyping VR experiences using the HTC Vive in Unity. We will see how to use Vive's inbuilt implementation of locomotion and, then, learn how both Vive's interaction system works by having a go at implementing our own locomotion techniques from scratch!

You should work in groups of 2 around the VR PCs. When deploying your VR experiences, make sure that another team member ‘spots’ the user to make sure they don’t bump or trip into anything or fellow students. 

## Task 1: Your First Virtual Reality Project in Unity

We’ll be using Valve’s SteamVR Unity plugin to prototype for HTC Vive in this course. To help you get stared quickly, I've created a template project with everything installed and set up. You can access this project by making a copy of this repository in your personal git account by pressing the ```Use This Template``` button at the top right of this page, and then cloning your personal version of the respository onto the machine you're working on.

> **Note**
>
> As you're working as a group, you might want to push your code to multiple personal git repos at the end of the practical so you all have a copy. If you'd like to know how to do this, just ask one of the teaching staff and we'll show you.

Once you've opened the project, open up the ```Island Scene``` in the ```Scenes``` folder. You'll see a pop-up asking you to confirm some settings. Click ```Accept All```. Next, run the scene. You should find yourself in VR on the island! You should be able to look around and see two gloved hands where the controllers are held.

If the scene won't run, you might need to open the SteamVR Input Window (found under  
