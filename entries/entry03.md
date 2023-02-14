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
Now that we, for the most part, have separated each Sprite individually, my next objective was to animate them. To ready myself, for when I would need to change or manipulate Sprites based on different parameters.
I first had to set up movement for my Player, which was relatively simple.
Your Player is divided into their parameters and information, to which you can add components.
One of these components is called the Rigidbody2D, which gives the Player model the ability to react to real-time inputs and responses.
Giving the Player Rigidbody, opens it up to Player inputs, such as the wasd or arrow keys respectively.
However, to give the Player inputs, we need to set up a C# file.

Setting up the file is simple, with a few clicks, and a new file called PlayerController.cs is created and ready to use.
For it to manipulate the Player, it has to be added as a Component to the Player.
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
To move, we need to access two outside variables.
- Velocity: The movement, in units per second of the character.
- Horizontal & Vertical Inputs: Our X & Y inputs for character direction.
Unity already has both of these, and both Horizontal and Vertical inputs are already set as either wasd or the arrow keys as mentioned before.
To call these variables, we need to use `theRB`, and much like a method, call `theRB`'s Velocity and set it to a specific value.

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
With this, our Player character can move about the screen, if it weren't for the fact that the raw inputs that Vector2 receives are extremely low.
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
For example, in this situation imagine our moveSpeed is set to 5. Now, when we press either a Horizontal or Vertical input, that value will multiply by 5, and our character will move 5 units per second.
There is a showcase of movement in my current prototype, linked below this blog entry on Google Classroom.
Now that we have finished the basics of movement, we can go into how to animate our Player with the rest of the unused Sprites from our Spliced Sprite Sheet.

## Movement Work Part 2: Animation Set-Up
In this section, I will go over how I set up the various directional sprites and their animations. 
As stated in previous sections, being able to splice each Sprite from a Sprite Sheet is vital to making animations.
While animations are not exactly needed, understanding how to set up animations in advance is a skill I wished to work on for Beyond my MVP.
To begin, we must understand that many Sprites are set in different directions: Up, Down, Left, and Right. Each corresponds to a specific movement.
There are two character states, moving and idle. It is these two states that will help us understand what Sprite goes where and how they function when animating.
Let's go over the simple stuff first, Idle animation.

Idle animations play only when the Player has made no input or has stopped creating inputs.
Thus, an Idle animation should play when the character has not moved. We should also take into account the four cardinal directions, and set up a map for each of these directions on Unity's [animation board](idleAnimationBoard.png).
When viewing this board, we see an animation duration slider on the right. This is to showcase how long a specific animation will last, in the case of an Idle animation, it will last only 1 second but continuously loop as long as no input is given.

After our Idle animations, comes our Movement animations, which are far less simple than inputting still Sprites at 1-second intervals.
As seen in our [Sprite Sheet](falseSpriteSheet.png), each direction has 4 Sprites for movement. Thus, for each direction, we need to make a new instance of a Movement animation.
While creating our Movement animations, we separate each Sprite on the duration slider by 5 seconds, ending at 16 seconds. The reason for ending at 16 is so that the final Sprite can last at least 1 second before looping the [animation](moveAnim.png).
However, there are a few flaws in our design here.
- The animation ends too fast, cutting past the final movement sprite, and immediately cutting to the beginning.
- The animation moves too fast, with the animation going at too high of a speed to be viewable.

Now, we need to fix this. Thankfully, there is a way, and it's surprisingly simple.
As I stated before, each direction has 4 individual movement Sprites, and our issue is that the animation never lasts on the final Sprite.
So, we can simply make the final Sprite play twice in the animation, which allows it to last the full 5 seconds we need.
Next, we simply need to extend the animation. Extending the gap between each Sprite to ten seconds forces it to take longer to finish the animation, and gives it more realism rather than pure speed.
Here is the final movement [animation board](finalMoveAnim.png).
After setting up all of the animation boards, for both Idle & Moving animations, we can finally build the connection between animation and input.

## Animation Work: The Link Between Input & Animation
We've reached the final step, and the final stage of my prototype, for now. This section will cover the connection between Player Input and Frontend animation.
The first thing we need to go over is how the animator for Unity works. It is divided into a few sections, but our main 3 go as follows:
- Animation Parameters: These are specific variables that act like conditionals when attached to an animation.
- Blend Trees: These are groups of animations, much like calling methods in Java, that work to call a specific animation when a specific parameter is met.
- Layers: Much like Ogre's, Unity's animations have many layers, and they can be held within Blend Tree layers to be called when that group receives an Input.

Knowing this, how can we manipulate Blend Trees and Inputs to get the right animations for the right situations?
Well, we can start where most Players would start, an Idle animation.

Assuming the Player did not immediately press any movement key, they would start in an Idle position. In which our parameters, `MoveX` and `MoveY` are set to 0.
This is where our first conditional comes into play. Our Player_Idle Blend Tree checks if any parameter is greater than 0.1, or less than -0.1 if moving down or left.
If this condition is true, then the animations will transition to our secondary Blend Tree, Player_Movement.
To check our `MoveX` and `MoveY`, we need to update our PlayerController.

```C#
public class PlayerController : MonoBehaviour{

    public Rigidbody2D theRB;
    public float moveSpeed;

    public Animator myAnim;

    // Update is called once per frame
    void Update(){
        theRB.velocity = new Vector2(Input.GetAxisRaw("Horizontal"), Input.GetAxisRaw("Vertical")) * moveSpeed;

        myAnim.SetFloat("MoveX", theRB.velocity.x);
        myAnim.SetFloat("MoveY", theRB.velocity.y);
    }
}
```
Here, we create a new variable called myAnim, which holds all of our Animator parameters and values.
With this, we can influence and change our parameters for every frame that the game updates itself.
The line ```C#myAnim.SetFloat("MoveX", theRB.velocity.x);``` sets the `MoveX` parameter to the current velocity of the Player in that frame.
Which is then checked in our conditions. If, for example, this value is greater than 0.1, then we will transition into the Player_Movement Blend Tree, and start the animation corresponding to an x-value greater than 0.1.

We know this works, as I have shown it working previously in my Movement Showcase. The more difficult subject concerns returning to our Idle animation.
You may notice that as our code and process is now, there is no way to return to our Idle animation from our Movement animations.
There is, however, a good fix for this. We need to create new parameters in our Animator that store a final movement value.

To explain, this new parameter would store the value of the last Input at the end of a frame. For example, if I held down the right arrow key, this new `LastMoveX` parameter would store 0.1 for each frame I continued to hold the right arrow key down.
However, if we were to do it this way, for every frame, it would simply return `LastMoveX` and `LastMoveY` to 0 when a button had not been held.
Thus, we set up a conditional that checks if any Input is being given, and if one is, then that's when those parameters are changed.
They will not be changed outside of when a button is being pressed, thus a value that holds our final facing direction is stored, the code goes as follows:

```C#
public class PlayerController : MonoBehaviour{

    public Rigidbody2D theRB;
    public float moveSpeed;

    public Animator myAnim;

    // Update is called once per frame
    void Update(){
        theRB.velocity = new Vector2(Input.GetAxisRaw("Horizontal"), Input.GetAxisRaw("Vertical")) * moveSpeed;

        myAnim.SetFloat("MoveX", theRB.velocity.x);
        myAnim.SetFloat("MoveY", theRB.velocity.y);

        if(Input.GetAxisRaw("Horizontal") == 1 || Input.GetAxisRaw("Horizontal") == -1 || Input.GetAxisRaw("Vertical") == 1 || Input.GetAxisRaw("Vertical") == -1){
            myAnim.SetFloat("LastMoveX", Input.GetAxisRaw("Horizontal"));
            myAnim.SetFloat("LastMoveY", Input.GetAxisRaw("Vertical"));
        }
    }
}
```
With all of this considered and taken into account. We have finished work on all the animations, Sprites, and kinks that needed work.

## Final Notes
This Blog Entry went over Stages 5 and 6 of the Engineering Design Process, creating and testing a prototype for my game. I believe that this soft reboot of my progress was a large help, and allowed me to look back on what was needed rather than what was coolest.
In the time since my last Blog Entry, I have worked extensively on two skills: How to Learn, and Consideration.
- How to Learn: Previously, I had mainly skimmed through my Udemy Course to speed through to things I thought were important. However, since then, I have been going through each lesson slowly and taking in the information better, rather than skimming through it. It has allowed me to understand more, and open myself to more concepts and ideas that I would likely not have thought of otherwise.
- Consideration: Taking the time to reboot my project took a lot of time to consider. I mainly thought about what I wanted my project to be, rather than what it would likely be. I understand now that I was likely too ambitious with my previous ideas, and considering the idea of stepping back to scope out what I needed to do compared to what I wanted to do helped me in that regard. I considered what was more important, a messy, unpolished ambitious game, or a workable, and small fan project. Considering that fact helped me understand more of what I need in my project.

Now, I must continue to work on my prototype for a few more days. I want to work out any more lessons I likely skimmed over, and do them more in-depth, gain a better understanding of my material, and work on it from there.
Thank you for reading.

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
