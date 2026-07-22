# AssetLoader

`AssetLoader` loads assets from AssetBundles embedded in your mod assembly.

Namespace:

```csharp
using StellarModdingAPI.Assets;
```

## Creating an AssetLoader

Example:

```csharp
Loader = new AssetLoader(
    MelonAssembly.Assembly,
    LoggerInstance,
    new[]
    {
        "PottedPlant", "PottedPlantThumbnail"
    }
);
```

The names provided to `AssetLoader` are the asset names that your mod wants
to load.

In this example:

- `PottedPlant` is a GameObject prefab.
- `PottedPlantThumbnail` is a Texture2D image.

A logger isn't required. If you don't provide one, `AssetLoader` simply
won't log anything, but it will still load assets normally.

!!! note

    `AssetLoader` should be created after `OnLateInitializeMelon`. Creating
    it earlier may cause an error, since Unity's asset systems may
    not be fully initialized yet.

## Loading Assets

After creating an `AssetLoader`, assets can be retrieved using:

```csharp
Loader.GetAsset<T>("AssetName")
```

Example:

```csharp
GameObject prefab = Loader.GetAsset<GameObject>("PottedPlant");

Texture2D thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail");
```

The generic type should match the Unity asset type being loaded.

## Asset Requirements

Assets must:

- Be included in an AssetBundle
- Have a unique asset name
- Be included when creating the AssetLoader

If an asset cannot be found, `AssetLoader` will log an error and return null.

## Supported Asset Types

`AssetLoader` can load any Unity Object stored in an AssetBundle.

Examples:

| Type        | Usage                               |
| ----------- | ----------------------------------- |
| GameObject  | Part prefabs and models             |
| Texture2D   | Build menu thumbnails and textures  |
| Material    | Custom materials                    |
| AudioClip   | Custom audio                        |

!!! note

    As said before, those are examples, not every supported asset type!