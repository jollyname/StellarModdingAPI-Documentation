# Creating AssetBundles

Before assets can be loaded by `AssetLoader`, they need to be packaged into an AssetBundle inside the Unity Editor.

## 1. Assign an AssetBundle Name

1. Select the asset in the Project window (e.g. a prefab, texture, or material).
2. In the Inspector, open the **AssetBundle** dropdown at the bottom.
3. Create a new bundle name (e.g. `pottedplant`) or select an existing one.

!!! tip

    Assets that should be loaded together can share the same bundle name.
    `AssetLoader` reads every embedded AssetBundle it finds, so you don't
    need a separate bundle per asset. Feel free to manage bundles however
    you want.

## 2. Set the Asset's Name

`AssetLoader` matches assets by their `Object.name`, which is normally just
the file name of the asset in the Project window. Rename the asset file to
match the key you plan to use, for example:

- PottedPlant.prefab
- PottedPlantThumbnail.png

## 3. Build the AssetBundle

Add a build script under an `Editor` folder in your Unity project:

```csharp
using UnityEditor;
using UnityEngine;
using System.IO;

public class BuildAssetBundles
{
    [MenuItem("Bundles/Build AssetBundles")]
    public static void BuildAllAssetBundles()
    {
        string outputPath = "Assets/AssetBundles";

        if (!Directory.Exists(outputPath))
            Directory.CreateDirectory(outputPath);

        BuildPipeline.BuildAssetBundles(
            outputPath,
            BuildAssetBundleOptions.None,
            BuildTarget.StandaloneWindows64
        );

        Debug.Log("AssetBundles built successfully!");
    }
}
```

Running this from **Bundles → Build AssetBundles** produces a file named
after your bundle (e.g. `pottedplant`) with no file extension. This is the
file you'll embed into your mod assembly.

!!! warning

    Build the AssetBundle against the same Unity version (and ideally the
    same target platform) as the game you're modding, otherwise Unity may
    fail to load it at runtime.

## Next Step

Continue to [Embedding Assets in Your Mod](embedding-assets.md) to include
the built AssetBundle in your mod's assembly.