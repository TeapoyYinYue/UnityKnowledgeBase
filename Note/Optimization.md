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

