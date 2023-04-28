# Entry 4
##### 3/19/23

Since my last Blog Entry, I have made an effort into finishing the Test file I wished to work on following.
However, because of many personal issues concerning mental health and other issues, I haven't been able to work much towards my MVP.
I've made some progress, but it's mainly surface level, and any larger progress will have to be pushed over the following month.
Thus, in this Blog Entry, I would like to explore some progress made in my Test file of Unity.

I would like to explore some very interesting and important content, specifically Switching Scenes, with Map Manipulation.
Switching Scenes is integral for any multi-layered game. Switching Scenes includes entering new locations, entering combat, even small things like UI updates are done through scene switching.
It's a broad concept, that allows for almost any 2D Unity game to function.
Man Manipulation sort of falls into a subcategory of Scene Switching, due to the fact that in order to switch scenes, you need to have an available map open to switch into.
This being able to manipulate and pull specific maps for when they are needed is a big help.

## Switching Scenes
One of the most important parts of any game, that isn't a sidescroller, is being able to transition between different scenes. 
Transitioning between locations, be it through fast travel, loading into a house, or something as subtle as passing through a gate, is always one of the most important aspects of a game.
Such is why I would like to describe my process of developing my Scene Switching.

The first thing we would need to go over is setting up separate scenes, which is fairly one note.
All you have to do is create a new scene in your scene file, and to make sure that Unity preloads the scene by fixing the scene manager setting and adding the new scene to it.
Our next step is to make two C# Scripts, one we'll call AreaEnd and the other we'll call AreaEntrance.
As a basic synopsis, AreaEnd is the location that the Player enters to move to the next scene, while AreaEntrance is the location in which the Player is spawned into the next scene. 

## AreaEnd
Beginning with AreaEnd, we have to first go over how the scene will know what scene to load next.
This is a rather easy variable to set up, as it is a simple Public String that we can give the name of the next scene to while in the Unity editor.
Our next objective is to set up the areaTransitionName, which is a value that acknowledges the group of transitions we will make in the future.
We attach this value to the appropriate entrance, but I'll describe that in the following section.
```C#
public class areaEnd : MonoBehaviour
{
    public string areaToLoad;

    public string areaTransitionName;

    public areaEntrance theEntrance;

    // Start is called before the first frame update
    void Start()
    {
        theEntrance.transitionName = areaTransitionName;
    }
}
```
Our next goal is to set up the next area to load, and actually transitioning to that area. 
To do the first objective, we need to use the SceneManager and an appropriate method to make sure the current AreaEnd knows what the next location to load is.
After that, it is important to understand what a "trigger" is. 
Similarly to collision in many other games, a trigger is an area that detects for a specific event, in this case a Player entering the area. 
A trigger allows the Player in question to move through it, but in our situation we want to transition when the Player moves into our trigger. 
This also requires us to give the current instance of the Player the areaTransitionName, so that the Player properly moves into the group of areas to load. 
```C#
public class areaEnd : MonoBehaviour
{
    public string areaToLoad;

    public string areaTransitionName;

    public areaEntrance theEntrance;

    // Start is called before the first frame update
    void Start()
    {
        theEntrance.transitionName = areaTransitionName;
    }
    
    private void OnTriggerEnter2D(Collider2D other){
        if(other.tag == "Player"){
            SceneManager.LoadScene(areaToLoad);

            PlayerController.instance.areaTransitionName = areaTransitionName;
        }
    }
}
```
Here is how the trigger would look, upon entering, the code checks for the objects Tag.
If tagged as the Player, it loads the next designated scene, while also attaching the areaTransitionName to the Player so that they can return when they enter the next scenes AreaEnd.

## AreaEntrance
The next section to go over is our AreaEntrance script, and how it applies to our AreaEnd. 
The first thing to note is that before creating our AreaEntrance in the world via Unity's editor, our Player simply spawns in the same location they would be relative to the old scene.
To explain, if the Players coordinates in scene 1 are 25x, 75y when entering AreaEnd, that will be its exact same spawn location in scene 2.
Despite the fact that the AreaEnd location of scene 2 is in a different location.

This is the purpose of AreaEntrance, to give a location for the Player to spawn in when entering the new scene. 
Its actually extremely simple to code, as it only requires 3 lines to run properly.
```C#
public class areaEntrance : MonoBehaviour
{
    public string transitionName;

    // Start is called before the first frame update
    void Start()
    {
       if(transitionName == PlayerController.instance.areaTransitionName){
            PlayerController.instance.transform.position = transform.position;
        }
    }
}
```
The transitionName variable is set up to be the same as the AreaTransitionName, as mentioned, its a collective group of transitions.
Thus they all share the same transition name to make it easier to code. 
If the Player has the same transition name, their position is properly updated to where AreaEntrance is placed on the Unity editors map.

## T

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
