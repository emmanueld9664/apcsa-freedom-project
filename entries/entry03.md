# Entry 3: Creating & Testing a Prototype
### 2/12/23

Since my previous Blog Entry, I have taken a few steps back in terms of the ambitious project I have taken into my hands. 
Previously, I wanted to work on my in-game sprites, while under the impression that I had done enough in my game to warrant them.
After a serious analysis of where I was in my process, I decided my best course of action would be to have a soft reset of sorts.
I decided to follow my previously introduced [Udemy Course](https://www.udemy.com/course/unity2drpg/) more in-depth, and step-by-step. There were many things I missed, and many that I needed to improve on.
Thus, while going through it again, I created a better prototype of my project with the given assets and wish to share my new, refreshed, knowledge and achievements with you now.

My first step in this soft reset would be to restart my background and movement section. Which, over a few days, I finished. The background was simple, I only needed to pull the image into the Scene where the camera lay. 
Next would be to give the image its specific layer, which was called the Background Layer. That way, the image would not conflict with any other objects on the screen.
Here is a [screenshot](screenshot.png.png) of the described action.
After this, I decided my best course of action would be to follow up and implement the Player Sprite & Player Movement as recommended by my course. My next section goes over Sprite and then the following section goes over Movement & Animation.

## Sprite Work
When downloading the files and images given to me by the Udemy Course, I was given an image called the "Sprite Sheet", which is a collection of many different Sprites on a single image.
Uploading this image directly to the Scene would not work well, because it would show each frame of the Player character at the same time. 
So I had to find a way to separate all of the Sprites from the Sheet individually.
Thankfully, Unity has multiple ways of doing this. 
Firstly, I had to edit Unity's understanding of the image for it to work.
1. Unity views images as either Single or still images. Multiple, colliding images. Or as 3d Polygon objects. A Sprite Sheet is seen as Multiple separate images, so I changed the setting corresponding to this fact.
2. Next, like most 2d games, I needed to change the amount of Pixels Per Unit to 16. To both fit the Background and Unity's measure of space.

The last two parameters I needed to change were the image Filter and the Image Size.

3. Filter represents how the image is shown, with further settings showing more blurred images, I had to change this setting to Point, to keep the Sprite work sharp and edged.
4. The size was simple, the image is 272x256 pixels, if I chose an image size smaller than this, it would end up blurry. Thus, I went with a 512x512 size to get the entire image to fit and not blur. 
You can see these parameters [here](parameters.png).

Now that the parameters were complete, it is important to go over how these individual Sprites are taken from the Sprite Sheet.
In the image seen previously, Unity has a function called the Sprite Editor. This editor allows the user to manipulate the shape and slice the Sprite Sheet how they please, with some automated options.
The option I needed was to slice the sheet by Cell Size, which would automatically cut around a 16x32 pixel area, to account for the width and height of most individual Sprites.
After telling the Sprite Editor to commit, it gave me [this](falseSpriteSheet.png).

## Movement Work Part 1: Set-Up
Now that we, for the most part, have separated each Sprite individually, my next objective was to animate them. To ready myself for when I would need to change or manipulate Sprites based on different parameters.
I first had to set up movement for my Player, which was relatively simple.
Your Player is divided into their own parameters and information, in which you can add components.
One of these components is called the Rigidbody2D, which gives the Player model the ability to react to real-time inputs and responses.
Giving the Player Rigidbody, basically opens it up to Player inputs, such as the wasd or arrow keys respectively.
However, in order to actually give the Player inputs, we need to set up a C# file.

Setting upp the file is simple, a few clicks and a new file called PlayerController.cs is created and ready to use.
In order for it to actually manipulate the Player, it has to be added as a Component to the Player.
Now, we're ready to go in on physically moving the Player in the scene.
We start by creating a `public Rigidbody2D` variable, which we use to manipulate.

```C#
public class PlayerController : MonoBehaviour{

    public Rigidbody2D theRB;

    // Start is called before the first frame update
    void Start(){
        
    }

    // Update is called once per frame
    void Update(){
    
    }
}
```
This, `theRB`, variable allows us to manipulate the character's "body" whenever we do a specific action that we code in.
In order to move, we need to access two outside variables.
- Velocity: The movement, in units per second of the character.
- Horizontal & Vertical Inputs: Our X & Y inputs for character direction.
Unity already has both of these, and both Horizontal and Vertical inputs are already set as either wasd or the arrow keys as mentioned before.
In order to call these variables, we need to use `theRB`, and much like a method, call `theRB`'s Velocity and set it to a specific value.

```C#
public class PlayerController : MonoBehaviour{

    public Rigidbody2D theRB;

    // Start is called before the first frame update
    void Start(){
        
    }

    // Update is called once per frame
    void Update(){
      theRB.velocity = new Vector2(Input.GetAxisRaw("Horizontal"), Input.GetAxisRaw("Vertical"));
    }
}
```
Vector2 is a given directional input, in this situation the raw data from our Horizontal and Vertical inputs, giving a direction for the Velocity to travel in.
With this, our Player character is able to move about the screen, if it weren't for the fact that the raw inputs that Vector2 recieves is extremely low.
`GetAxisRaw` basically returns a 0 or 1, depending on if that key has been pressed.
Because of such a low value, our Player's movement speed is just as low. Moving barely 1 unit per second.
To change this, we can set up a float variable, which can be given a specific and unchanging value as our set speed.

```C#
public class PlayerController : MonoBehaviour{

    public Rigidbody2D theRB;
    public float moveSpeed;

    // Start is called before the first frame update
    void Start(){
        
    }

    // Update is called once per frame
    void Update(){
      theRB.velocity = new Vector2(Input.GetAxisRaw("Horizontal"), Input.GetAxisRaw("Vertical")) * moveSpeed;
    }
}
```
For example, in this situation imagine our moveSpeed is set to 5. Now, when we press either a Horizontal or Vertical input, that value will multiply 5, and our character will move 5 units per second.
There is a showcase of movement in my current prototype, linked below this blog entry on Google Classroom.
Now that we have finished the basics of movement, we can go into how to animate our Player with the rest of the unused Sprites from our Spliced Sprite Sheet.

## Movement Work Part 2: Animation Set-Up
[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
