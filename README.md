# Practical: Prototyping VR in Unity with the HTC Vive

In the lecture, we saw how contemporary virtual reality headsets work from a visual and audio perspective. We also saw a number of “locomotion techniques” that allow us to move a player around a virtual world that they are experiencing using VR.

In this practical, we are going to learn how to get started with prototyping VR experiences using the HTC Vive in Unity. We will see how to use Vive's inbuilt implementation of locomotion and, then, learn how both Vive's interaction system works by having a go at implementing our own locomotion techniques from scratch!

You should work in groups of 2 around the VR PCs. When deploying your VR experiences, make sure that another team member ‘spots’ the user to make sure they don’t bump or trip into anything or fellow students.

Before you start the tasks, you can get VR up and runnning by following these instructions:

https://github.com/uoy-im-aitt/vive-instructions/

## Task 1: Your First Virtual Reality Project in Unity

We’ll be using Valve’s SteamVR Unity plugin to prototype for HTC Vive in this course. To help you get stared quickly, I've created a template project with everything installed and set up. You can access this project by making a copy of this repository in your personal git account by pressing the ```Use This Template``` button at the top right of this page, and then cloning your personal version of the respository onto the machine you're working on. If you're working in the Immersive Lab, then you should check your project out into a sub-directory of ```C:\Temp```. If you try and check it out to the default location (your H:\ drive) it'll be too slow.

> **Note** As you're working as a group, you might want to push your code to multiple personal git repos at the end of the practical so you all have a copy. If you'd like to know how to do this, just ask one of the teaching staff and we'll show you.

Once you've opened the project, open up the ```Island``` Scene in the ```Practical Assets``` folder. You'll see a pop-up asking you to confirm some settings. Click ```Accept All```. Next, run the scene. You should find yourself in VR on the island! You should be able to look around and see two gloved hands where the controllers are held.

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

## Task 3: Extending the Inbuilt Interaction System by Making a Simple Terrain-friendly Teleporter

The inbuilt interaction system we've been exploring so far is great, and supports a lot of the interactions you'll want to implement in your projects. However, sometimes you'll find you'll want to do something that the interaction system doesn't support. In this case, you'll need to implement some custom code. 

In this task, we're going to learn how to implement our own custom interactions within the context of the interaction system. We're going to do it by implementing our own (albeit basic) teleporter. What's cool about the teleporter we're going to make though, is that it works for terrain!

### Stage 1: Creating a new Action

One of the really nice features of the SteamVR interaction system is that its input system is based on abstract "Actions" that are not tied to any specific controller or device. This means that as a designer of a VR environment you can think on terms of the abstract things that the user does or can do (e.g. "Grab", "Jump", "Shoot", "Teleport") rather than specific manipulations that the user performs on the controller (e.g. "Pull Trigger", "Press Touchpad"). The aim behind this design decision is to allow designers to create experiences that'll work across multiple VR platforms without loads of extra coding.

To learn about Steam's actions, we're going to implement our custom teleporter as a new action. To do this, open up the SteamVR Input window (find it in ```Window > SteamVR Input```).  In the window you'll see a list of ```In``` actions, which include the ```Teleport``` action that we've already been performing in the previous task. Add a new action to this list by pressing the little ```+``` button at the bottom.

A little UI will appear to the right that let's you configure your new action. The first thing to do here is to give your action a name, which you should set as ```offroadteleport```. Next choose, the type of action from the dropdown. The type of action defines the kind of input you'll need from the user to make the action happen. We just want an on/off button press, so we will choose ```Boolean```. Finally, put the name of your action in the ```Localized String``` box as well. When you've done this, press the ```Save and generate``` button, which will create the code for your new action.

Finally, we need to setup a controller binding that'll tell Unity which button on the Vive handsets will trigger our action. To do this, press the ```Open Binding UI```. A webpage will pop-up that let's you assign controls to the different actions in your experience. For example, you'll see that the Trackpad is already configured to initiate the ```Teleport``` action when the trackpad is pressed.

To complete this sub-task, see if you can change the binding for the trackpad button so that it initiates your ```offroadteleport``` action rather than ```Teleport``` when pressed.

### Stage 2: Coding a custom action

Now we've set up all the complicated stuff behind the scenes, let's actually code our teleporter script. Your script should provide the following functionality:

1. Draw a ‘laser pointer’ line forwards from the controller when the touchpad is pressed & held by the user
2. When this line intersects a game object (i.e. the terrain) and the user releases the touchpad, they should be teleported (i.e. moved) to the position of the intersection

To make this work, you'll need to use a few new bits of code. Firstly, you'll need to import the code library that let's you access info on SteamVR's actions into your script. You can do this by adding the following import statement to the top of your script:

```c#
using Valve.VR;
```

You'll now be able to call functions that'll tell you about the state of the different actions in your experience. For example, the code below will let you find out when the trackpad is pressed and when it isn't:

```c#
if(SteamVR_Input.GetStateDown("offroadteleport", SteamVR_Input_Sources.LeftHand))
{
  print("Fire out the teleport line");
}

if (SteamVR_Input.GetStateUp("offroadteleport", SteamVR_Input_Sources.LeftHand))
{
  print("Teleport to the intersection point");
}
```

Using the code above, see if you can make the print statements appear when you press the trackpad on either of the controllers. Once you've got this working, look at the code. Discuss in your group what each bit does. Can you work out how to make your teleporter only work for the right hand or both hands?

> **Note** having your teleporter only work for one hand will make the code in the next section much simpler, so just stick with listening for actions on either the left or right hand for now. However, if you'd like to make code that's generic across both controllers, explore basing the parameter passed to ```GetStateDown/Up``` a public variable of type ```SteamVR_Input_Sources```. If you do this, you'll see something cool about how Unity handles public variables that are enumerations (i.e. variables that have a fixed set of possible values).

### Stage 3: Implementing an actual teleporter

So far, we're just printing stuff out. No one is teleporting anywhere. In the final task, we're going to build on the code in our script to implement "terrain-friendly" / "off road" teleporting.

You can implement the actual teleport interaction with the help of Unity’s ```Physics.Raycast``` function (http://bit.ly/2b8FoSP). Recall from last year, this function draws an imaginary line through a scene and returns true if it intersects an object. It also populates a RaycastHit object with information about the object that the ray has intersected with and the intersection point. For example, here you can see how to use the method to cast an imaginary directly forward from the position of a game object and print out the intersection point.

```c#
Ray raycast = new Ray(transform.position, transform.forward);
RaycastHit hit;

bool bHit = Physics.Raycast(raycast, out hit);
if (bHit)
{
  print(hit.point);
}
```

Once you’ve found the position that the player is pointing at, you can simply teleport them there by setting the position of the ```Player``` game object to the intersection point. You can find out the intersection point from the ```point``` parameter of the ```hit``` variable, as shown above. You should use the ```LineRenderer``` component to draw the laser pointer (http://bit.ly/2aU5FAY).

One you’ve got your basic teleporter working, you may wish to improve it by:

- Setting a maximum teleportation distance
- Changing the color of the line when it is not intersecting any objects
- Rendering an object at the point of intersection, to make it more visible to the user

### Task 4: More Realistic Spatial Audio

In the lecture we learned how realistic spatial audio could be created for VR by simulating the cues that the human mind uses to work out the direction and distance a sound is away from a listener. A standard AudioSource in Unity only simulates two of these cues: i) changes in level caused by distance and ii) inter-aural level differences. To experience the effect of these audio cues, open the ```Audio Experiment``` scene, add a Player pre-fab (found in ```SteamVR/InteractionSystem/Core/Prefabs```) to the centre of the floor and listen to how the sound changes you move your head.

Could you accurately determine the direction that sound is coming from the cues? The answer should be ‘not always’. As we saw in the lecture, the two cues Unity uses for its standard 3D sound are not sufficient to allow humans to properly work out the direction of an audio source. This is because it’s not possible for the brain to work out whether a sound is in front or behind us from inter-aural level differences alone. In order to solve this problem, we need to use a Head Related Transfer Function (HRTF) in order to simulate the changes in the frequency spectrum of a sound – which are caused by the body, head and ears of the listener when facing in different directions. Unity provides an audio spatializer plugin (as part of its Oculus Rift integration) that we can use to get this functionality in our VR scenes. We can turn on the spatializer for any audio source by following two steps:

1. Open the package manager ```Windows > Package Manager```
2. Select the “Oculus XR Plugin” in the list of packages, and click ```Install```
3. Enable the plugin by selecting ```OculusSpatializer``` from the ```Spatializer Plugin``` drop down menu in your project’s Audio settings ```Edit > Project Settings > Audio```)
4.	Select the ```Spatialize``` checkbox for any audio sources that you want to use the plugin with (tip: its right at the bottom)

Follow these instructions to enable spatialized audio for the ```AudioSource``` attached to the ```Speaker``` game object. Listen to the differences between the audio spatialization when the ```Spatialize``` checkbox is enabled or disabled. You may wish to have a team member tick and un-tick the box while another member listens, to see if you can tell which is which.

### Optional Extensions

If you complete all of the above tasks before the end of the practical, or would like to continue to develop your skills in your free study time, then you should consider experimenting with some of the following tasks:

- Create a simple VR racing game using one of Unity’s inbuilt racing cars (or adapt the one from the cameras practical in MPIE last year). Investigate whether adding a visible cockpit to the camera view changes the likelihood of the player becoming nauseous.
- Use Unity’s third person character controller pre-fab (in the standard character assets package) to implement the "Ghosting” locomotion technique that was described in the lecture.
