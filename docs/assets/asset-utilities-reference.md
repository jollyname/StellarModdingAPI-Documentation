# AssetUtilities

`AssetUtilities` automatically extracts asset names from a class or struct
containing asset keys, so you don't have to maintain the name list by hand
when calling `AssetCollection.LoadFrom()`.

Namespace:

```csharp
using StellarModdingAPI.Assets;
```

## Extracting Asset Keys

Example:

```csharp
public static class AssetKeys
{
    [AssetKey] public const string PottedPlant = "PottedPlant";
    [AssetKey] public const string PottedPlantThumbnail = "PottedPlantThumbnail";
}
```

The keys can then be extracted using:

```csharp
AssetUtilities.ExtractKeysFrom<AssetKeys>();
```

This returns:

```csharp
[
    "PottedPlant",
    "PottedPlantThumbnail"
]
```

In practice, you rarely need to call this yourself — `AssetCollection.LoadFrom<T>()`
calls `ExtractKeysFrom<T>()` internally, scanning `T` for `[AssetKey]`-marked
members automatically. Calling it directly is only useful if you want the
key list for something other than `LoadFrom<T>()`, e.g. logging or validation.

## Asset Key Attributes

Mark any constant string field or property with `[AssetKey]` to include it:

```csharp
public sealed class Mod : StellarMelonMod
{
    [AssetKey] public const string PottedPlant = "PottedPlant";
}
```

Only constant string fields (or `const`-equivalent members) can be used as asset keys.

!!! warning

    `ExtractKeysFrom<T>()` only picks up members explicitly marked with
    `[AssetKey]` — unmarked constants on `T` are ignored, so it's safe to
    mix asset keys with other constants on the same class (as in the
    `Mod` example on the [AssetCollection Reference](asset-collection-reference.md) page).