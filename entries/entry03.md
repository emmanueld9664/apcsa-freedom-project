# Entry 3: Creating & Testing a Prototype
##### 2/12/23

Since my previous Blog Entry, I have taken a few steps back in terms of the ambitious project I have taken into my hands. 
Previously, I wanted to work on my in-game sprites, while under the impression that I had done enough in my game to warrent them.
After a serious analysis of where I actually was in my process, I decided my best course of action would be to have a soft reset of sorts.
I decided to follow my previously introduced [Udemy Course](https://www.udemy.com/course/unity2drpg/) more in-depth, and step by step. There were many things I missed, and many that I needed to improve on.
Thus, while going through it again, I created a better prototype of my project with the given assets and wish to share my new, refreshed, knowledge and achievements with you now.

My first step in this soft-reset would be to restart my background and movement section. Which, over a few days, I finished. The background was simple, I only needed to pull the image into the Scene where the camera lied. 
Next would be to give the image it's own specific layer, which was called the Background Layer. That way, the image would not conflict with any other objects on the screen.
Here is a [screenshot](screenshot.png.png) of the described action.
After this, I decided my best course of action would be to follow-up and implement the Player Sprite & Player Movement as recommended by my course. My next section goes over the Sprite and then the following section goes over Movement & Animation.

When downloading the files and images given to me by the Udemy Course, I was given a image called the "Sprite Sheet", which is a collection of many different Sprites on a single image.
Uploading this image directly to the Scene would not work well, because it would show each individual frame of the Player character at the same time. 
So I had to find a way to separate all of the Sprites from the Sheet individually.
Thankfully, Unity has multiple ways of doing this. 
Firstly, I had to edit Unity's understanding of the image in order for it to work.
Unity views images as either Single, still images. Multiple, colliding images. Or as 3d Polygon objects.
A Sprite Sheet is seen as a Multiple separate images, so I changed the setting corresponding to this fact.
Next, like most 2d games, I needed to change the amount of Pixels Per Unit to 16. To both fit the Background and Unity's measure of space.
The last two parameters I needed to change were the image Filter, and the image Size.
Filter represents how the image is shown, with further settings showing more blurred images, I had to change this setting to Point, as to keep the Sprite work sharp and edged.
The size was simple, the image is 272x256 pixels, if I chose an image size smaller than this, it would end up blurry. Thus, I went with a 512x512 size in order to get the entire image to fit and not blur. You can see these parameters [here](parameters.png).

Now that the parameters were complete, it is important to go over how these individual Sprites are taken from the Sprite Sheet.
In the image seen previously, Unity has a function called the Sprite Editor. This editor allows the user to manipulate the shape and slice the Sprite Sheet how they please, with some automated options.
The option I needed was to slice the sheet by Cell Size, which would automatically cut around a 16x32 pixel area, to account for the width and height of most individual Sprites.
After telling the Sprite Editor to commit, it gave me [this](falseSpriteSheet.png).

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
