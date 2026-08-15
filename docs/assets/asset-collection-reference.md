# AssetCollection

`AssetCollection` loads assets from AssetBundles embedded in your mod assembly.

Namespace:

```csharp
using StellarModdingAPI.Assets;
```

## Creating an AssetCollection

Rather than listing asset names manually, mark each asset key as a constant
with the `[AssetKey]` attribute, then load them all at once with
`AssetCollection.LoadFrom<T>()`:

```csharp
public sealed class Mod : StellarMelonMod
{
    public static AssetCollection? Assets { get; private set; }

    [AssetKey] public const string PottedPlantKey = "PottedPlant";
    [AssetKey] public const string PottedPlantThumbnailKey = "PottedPlantThumbnail";

    public override void LoadAssets()
    {
        Assets = AssetCollection.LoadFrom<Mod>(logger: LoggerInstance);
    }
}
```

`LoadFrom<T>()` scans `T` for members marked with `[AssetKey]`, pulls their
string values as the asset names to load, and infers the assembly from `T`
automatically — no need to pass `MelonAssembly.Assembly` or build the key
array yourself.

In this example:

- `PottedPlant` is a GameObject prefab.
- `PottedPlantThumbnail` is a Texture2D image.

A logger isn't required. If you don't provide one, `AssetCollection` simply
won't log anything, but it will still load assets normally.

!!! note

    `LoadAssets()` is called by the framework at the appropriate point in
    the mod lifecycle, so you don't need to worry about ordering it after
    `OnLateInitializeMelon` yourself — just do your asset loading inside
    the `LoadAssets()` override, not in the constructor or `OnInitializeMelon`.

### Manual loading

If your asset keys aren't declared on the type you're loading from (for
example, loading assets defined in a different class, or assembling the key
list dynamically), you can call the explicit overload instead:

```csharp
Assets = AssetCollection.LoadFrom(
    assembly: MelonAssembly.Assembly,
    assetKeys: AssetUtilities.ExtractKeysFrom<Mod>(),
    logger: LoggerInstance
);
```

This is equivalent to `LoadFrom<Mod>(logger: LoggerInstance)` and mainly
useful for these edge cases — for a typical mod, prefer the generic
shorthand above.

## Loading Assets

After creating an `AssetCollection`, assets can be retrieved using:

```csharp
Assets.GetAsset<T>("AssetName")
```

Example:

```csharp
GameObject prefab = Assets.GetAsset<GameObject>(Mod.PottedPlantKey);

Texture2D thumbnail = Assets.GetAsset<Texture2D>(Mod.PottedPlantThumbnailKey);
```

The generic type should match the Unity asset type being loaded. Referencing
the `[AssetKey]` constant (rather than retyping the string) avoids typos and
keeps the key defined in one place.

## Asset Requirements

Assets must:

- Be included in an AssetBundle
- Have a unique asset name
- Be marked with `[AssetKey]` on the type passed to `LoadFrom<T>()` (or
  included in the `assetKeys` collection, for manual loading)

If an asset cannot be found, `AssetCollection` will log an error and return null.

## Supported Asset Types

`AssetCollection` can load any Unity Object stored in an AssetBundle.

Examples:

| Type        | Usage                               |
| ----------- | ----------------------------------- |
| GameObject  | Part prefabs and models             |
| Texture2D   | Build menu thumbnails and textures  |
| Material    | Custom materials                    |
| AudioClip   | Custom audio                        |

!!! note

    As said before, those are examples, not every supported asset type!