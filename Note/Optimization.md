# Optimization

## Asset Post Process

* Asset Auditing
  * Texture
    * Disable Read/Write
    * Disable mipmap if possible (for UI elements, mipmap is unnecessary)
    * Make sure textures are compressed
    * Ensure sizes aren't too large
      * 2048x2048 or 1024x1024 for UI atlases
      * 512x512 or smaller for model textures
  * Models
    * Disable Read/Write
    * Disable Rig on non-character models
    * Copy avatars for characters with shared rigs
    * Enable mesh compression

```csharp
//recevice callbacks on import
public class AssetAuditExample: AssetPostprocessor 
{
	public void OnPreprocessTexture()
    {
        
    }
    
    public void OnPreprocessModel()
    {
        //disable readable to 
        ModelImporter modelImporter = (ModelImporter) assetImporter;
        if(modelImporter.isReadable)
        {
            modelImporter.isReadable = fasle;
            modelImporter.SaveAndReimport();
        }
    }
}
```



## Asset bundle

* Cache to identify whether the resource is already loaded(to avoid duplicate instance with the same resource)
* a manager to manage load and unload bundle
* use memory profiler to identify the existence of the problem
  * https://bitbucket.org/Unity-Technologies/memoryprofiler/src/default/

## c#

### Boxing

* happens when passing a value type as a reference type.
* Value is temporarily allocated on the heap

```csharp
//Example
int x = 1;
object y = new object();
y.Equals(x); // Boxes "x" onto the heap
```

* Also happens when using enums as Dictionary keys

```csharp
enum MyEnum{a, b, c};
var myDictionary = new Dictionary<MyEnum, object>();
myDictionary.Add(MyEnum.a, new Object()); //Boxes value "MyEnum.a"

//Implement IEqualityComparer class to workaround
```

* Instruments: Search for "::Box" 

  ![image-20200519150320129](C:\Users\w\Documents\GitHub\UnityKnowledgeBase\Note\image-20200519150320129.png)

### For each

* Allocates a Enumerator when loop begins 

### Unity APIs

* if a Unity API returns an array, it allocates a new copy
  * Every time it is accessed, even if the values do not change

```csharp
//one allocation
Touch[] touches = Input.touches;
for(int i = 0; i < touches.Length; ++i)
{
    Touch touch = touches[i];
}

// allocationless
int touchCount = Input.touchCount;
for(int i = 0; i < touchCount; ++i)
{
    Touch touch = Input.GetTouch(i);
}
```

### XML,JSON, & other  text formats

* parsing text is slow
* avoid parsers built on Reflection - extremely slow

Strategy

* Don't parse text
  * bake text data to binary ( Use `ScriptableObject`)
* Do less work
  * Split data into smaller chunks
  * Parse only the parts that are needed
  * Cache parsing results for later use
* Threads
  * Pure C# types only 
  * No Unity Assets(`ScriptableObjects`, Textures, etc.)
  * Be VERY careful

### The Resources Folder

* An index of Resources is loaded at startup
* Cannot be avoided or deferred

* Solution : Move assets from Resources to Asset Bundles

## UI

### Batching

* divide static elements and dynamic elements into different canvases.
  * if a child within a canvas needs to be rebatched, the entire Canvas will be rebatched. 
* when a canvas contains both static and dynamic elements, dynamic elements can slow down the rest of the UI even through static elements don't need to be regenerated.

* can use profiler to check whether static and dynamic Canvases are batched separately.
* ![img](https://connect-prd-cdn.unity.com/20200303/learn/images/5e190d00-323f-4722-86d7-93aceac78930_image4.png)

