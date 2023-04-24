# Entry 5: The Current Final Product
##### 4/23/23

I believe we began this journey in the late days of October, and my first Blog Entry was in the early days of November. 
Since then, I have made a lot of progress in my product, being able to finish a Minimum Viable Product (MVP) that I can showcase to others.
This Blog Entry is for going over the MVP I created, using new knowledge and tricks I used to create it, explaining how I was able to finish on time.

I would like to go over the two most important features of my current product: the Camera and the Tilemap.
- The Camera is self-explanatory, as the camera in most games follows the Player, and confines what the player can see as they travel through the world.
- In 2d games especially, a Tilemap is important for being the area in which the Player can travel through and is confined to.

Thus, the first section I would like to go over is the Camera, explaining how I set it up, and how it works.
Following this will be the Tilemap, how it interacts with the Player and Camera, and other settings that make the world feel more alive.
Buckle your seatbelts, the ride gets bumpy from here on out.

## The Camera Section
As stated many times previously, I learned much of how to code in Unity and C# through my [Udemy Course](https://www.udemy.com/course/unity2drpg/learn/lecture/12193818#content).
This course led me through how to create and manipulate the Camera in Unity, which I will go over now.
The first thing we need to understand about the Camera is what we want it to do.
In this case, we want the Camera to follow the position of the Player no matter where the Player travels.
Thus, we can set up a Transform variable in the CameraController script that we call "Target".
This Target is set to the Player, and any changes we make to the Players position will be made to the Camera as well.
```C#
public class CameraController : MonoBehaviour
{

    public Transform target;
    
    // Start is called before the first frame update
    void Start(){
        target = PlayerController.instance.transform;
    }
    
    // LateUpdate is called once per frame after Update
    void LateUpdate(){
        transform.position = new Vector3(target.position.x, target.position.y, transform.position.z);
    }
}
```
That was quite a lot, so let's try to go over it step by step. As mentioned, we have the Target variable, which we always set to the Player in every situation.
Then, when the current scene first starts, we set the Target variable to every instance of the Player transforming, also known as moving.
In the second method, LateUpdate, we change the position of the Camera every frame to the current position of the target, which is our Player.
For now, its quite simple, but with the later introductions of Tilemaps and Boundaries, it begins to get a lot more intense.

The next thing to worry about is the actual size of the Camera itself, how much of the screen do we want to show Players at any given time?
```C#
public class CameraController : MonoBehaviour
{

    public Transform target;
    
    private float halfHeight;
    private float halfWidth;
    
    // Start is called before the first frame update
    void Start(){
        target = PlayerController.instance.transform;
        
        halfHeight = Camera.main.orthographicSize;
        halfWidth = halfHeight * Camera.main.aspect;
    }
    
    // LateUpdate is called once per frame after Update
    void LateUpdate(){
        transform.position = new Vector3(target.position.x, target.position.y, transform.position.z);
    }
}
```
Here we set up two new float variables, halfHeight and halfWidth, which control the size of our Camera going forward.
HalfHeight is simple, it is simply half of the vertical viewing volume of the Players screen, which in a 16:9 aspect ratio would be 4.5.
OrthographicSize is reliant on the Aspect Ratio of the game.
HalfWidth is reliant on it's partner variable to function, which is then multiplied by the horizontal aspect, in this case 16, to showcase the horizontal size of the Camera.

As for the rest of the Camera explanation, we have to move onto the Tilemap to explain Camera and Player Boundaries.

## The Tilemap Section
The Tilemap is one of the more intriguing aspects of a 2D game, as it's basically nowhere to be seen on a 3D game.
The Tilemap is a grid of squares that encompasses the entire space that the Developer sees when making changes to the Scene.
When a Developer updates this map, they use different layers in order to showcase different things.
This map is usually used as the background, but is also used for collision when needed for certain objects.

![Tile_Palette](https://user-images.githubusercontent.com/73564442/233901045-7081b6ba-0c23-4714-8f13-7df6308f0ee8.png)

Showcased above is what the Tile Palette for a Developer may look like, separated individually into squares, each one representing a different object that can be placed within the world. 
You may notice the phrasing "Active Tilemap" just above the actual Palette. 
This notes the current layer of Tilemap that is being manipulated when one of the items in the Palette is moved out into the actual map.
My Tilemap Grid is split into 3 distinct maps, each one serving a different purpose. 

![Grid_Layers](https://user-images.githubusercontent.com/73564442/233901532-7f66a301-a1f9-4f0b-ad62-3b2249ff2fa2.png)

Above is the Grid, which has 3 different layers of Tilemaps: The Ground, Above Ground, and Solid Ground.
We can start with the Ground layer, which is placed as -1 in the Background Layer order. This simply means that it is the very bottom of the background, and that no other object in the Background Layer will be underneath it.
The Ground layer has no collision, and serves as the very basic layer for the Player to walk on.
Similarly, the Above Ground also has no collision, but is placed at 0 in the order, so that the Sprites may be placed on top of the Ground layer and not have a transparent effect to them.
Finally, is the Solid Ground layer, placed last on the order bracket at 1, and having the most unique purpose, actual collision.

![Solid_Ground](https://user-images.githubusercontent.com/73564442/233902330-e1f215ea-ef09-4300-b77a-97f3185a3b32.png)

Here we can see some of the Components to the Solid Ground layer, which includes a Composite Collider, Tilemap Collider and Rigidbody, much like our Player.
Unlike our Player, however, the Rigidbody is set to Kinematic, so that it is not affected by outside forces or the natural gravity given to all Rigidbody objects.
The colliders allow the Player to be stopped by those collisions, and actually wraps around the real shape of the object, giving the illusion of well-defined collision detection.

With all this, we can begin to set up what a true Tilemap may look like, placing each layer appropriately and setting up a world for our Player to traverse through.

![Tilemap](https://user-images.githubusercontent.com/73564442/233902810-41b19049-7466-4e81-bc2a-1a4110aea0b8.png)

Here lies the Tilemap I made for my current MVP, and I will say I think I did a wonderful job with it.

Next comes the most important part, and one I alluded to previously, Camera and Player Boundaries. 
We don't exactly want our Camera or Player to run off the screen into the forever blue abyss, do we?
That's why we need to set exact boundaries for both of these objects, so that we're limited to what the current Tilemap shows us. 
We can do this for the Camera by Clamping the Cameras position to the boundaries of one of our Tilemaps.
```C#
public class CameraController : MonoBehaviour
{
    public Tilemap theMap;
    private Vector3 bottomLeftLimit;
    private Vector3 topRightLimit;

    // Start is called before the first frame update
    void Start(){
        bottomLeftLimit = theMap.localBounds.min + new Vector3(halfWidth, halfHeight, 0f);
        topRightLimit = theMap.localBounds.max + new Vector3(-halfWidth, -halfHeight, 0f);

        PlayerController.instance.SetBounds(theMap.localBounds.min, theMap.localBounds.max);
    }

    // LateUpdate is called once per frame after Update
    void LateUpdate(){
        // Keep the camera inside the bounds
        transform.position = new Vector3(Mathf.Clamp(transform.position.x, bottomLeftLimit.x, topRightLimit.x), Mathf.Clamp(transform.position.y, bottomLeftLimit.y, topRightLimit.y), transform.position.z);
    }
}
```
To make it easier to read, I cut out the information explained in the Camera section.
Above we can see three new variables, a Tilemap variable, and two Vector3 variables.
The Tilemap variable is easy to explain, as it represents the Tilemap that we want to limit the Camera and Player by.
Both Vector3 variables represent a corner of the chosen Tilemap, and are used to stick the Camera into these closed off boundaries.
Then, in LateUpdate, you force the position of the Camera to stay within the range of the limits of the chosen Tilemap.

## Final Notes
This Blog Entry went over the seventh stage of the Engineering Design Process, improving my prototype as needed. 
While not exactly the product I wished to have when I began my Freedom Project, I believe I ended up with a product that is still interesting, and personally fun to develop.
I believe that the two skills I worked the most on since beginning my Freedom Project were having a Growth Mindset and having Creativity.
- In terms of a Growth Mindset, I understand that I have had many difficulties with managing my time, along with other difficulties I have faced in the past few months.
Even still, I pushed through those difficulties to make a product that I enjoy, and a product I would gladly show to others.
- Being Creative is the lifeblood of any game developer, and while the resources are not my own, the way I used them to create an interesting locale to traverse makes it all the more meaningful.
Being creative is all about how I use the tools I'm given, and I think I used them in a very creative way. 

For now, I believe I will continue to work on the project more, in order to push out a more unique game that has more features and unique interactions that I cna come up with.
Thank you for your time, have a wonderful day.

[Previous](entry04.md) | [Next](entry06.md)

[Home](../README.md)
