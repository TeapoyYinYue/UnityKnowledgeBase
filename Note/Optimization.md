# Optimization

## Asset Post Process

* Asset Auditing

```csharp
//recevice callbacks on import
public class AssetAuditExample: AssetPostprocessor 
{
	public void OnPreprocessTexture()
    {
        
    }
    
    public void OnPreprocessModel()
    {
        
    }
}
```



## Asset bundle

* Cache to identify whether the resource is already loaded(to avoid duplicate instance with the same resource)
* a manager to manage load and unload bundle
* use memory profiler to identify the existence of the problem

## c#

### Boxing

```csharp

```



## UI

### Batching

* divide static elements and dynamic elements into different canvases.
  * if a child within a canvas needs to be rebatched, the entire Canvas will be rebatched. 
* when a canvas contains both static and dynamic elements, dynamic elements can slow down the rest of the UI even through static elements don't need to be regenerated.

* can use profiler to check whether static and dynamic Canvases are batched separately.
* ![img](https://connect-prd-cdn.unity.com/20200303/learn/images/5e190d00-323f-4722-86d7-93aceac78930_image4.png)

