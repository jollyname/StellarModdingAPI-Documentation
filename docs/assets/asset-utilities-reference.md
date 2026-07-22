# AssetUtilities

`AssetUtilities` automatically extracts asset names from a class or struct
containing asset keys, so you don't have to maintain the name list by hand
when calling `AssetLoader`.

Namespace:

```csharp
using StellarModdingAPI.Assets;
```

## Extracting Asset Keys

Example:

```csharp
[AssetKeyCollection]
public static class AssetKeys
{
    public const string PottedPlant = "PottedPlant";
    public const string PottedPlantThumbnail = "PottedPlantThumbnail";
}
```

The keys can then be extracted using:

```csharp
AssetUtilities.ExtractAssetKeysFrom(typeof(AssetKeys));
```

This returns:

```csharp
[
    "PottedPlant",
    "PottedPlantThumbnail"
]
```

## Asset Key Attributes

Individual fields can also be marked using `[AssetKey]`, instead of marking
the whole class with `[AssetKeyCollection]`:

```csharp
public static class AssetKeys
{
    [AssetKey]
    public const string PottedPlant = "PottedPlant";
}
```

Only constant string fields can be used as asset keys.

!!! warning

    When a class is marked with `[AssetKeyCollection]`, **every** field on
    it is treated as an asset key, not just the ones you intend. If any
    field on that class isn't a `const string`, `ExtractAssetKeysFrom` will
    throw an exception.

    Keep `[AssetKeyCollection]` classes dedicated to asset keys only, or
    use `[AssetKey]` on individual fields in a mixed-purpose class instead.