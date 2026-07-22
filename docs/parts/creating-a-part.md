# Creating a Part

This guide will walk you through creating a custom part and adding it to the StellarDrive build menu.

A custom part requires:

- A model prefab
- A thumbnail
- Basic properties
- Optional building costs

Parts are created using a `PartDefinition`, registered through the API, and then added to the game's build menu.

## Loading Assets

Before creating a part, you need to load the assets used by the part.

The StellarModdingAPI provides `AssetLoader` for loading assets from Unity AssetBundles.

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

The asset names passed to `AssetLoader` are the names of the assets that your mod will use later.

In this example:

- `PottedPlant` is the part's model prefab.
- `PottedPlantThumbnail` is the image displayed in the build menu.

## Initializing the API

Before registering parts, initialize the StellarModdingAPI:

```csharp
StellarModdingAPI.Plugin.Initialize();
```

This prepares the internal registries required for custom content.

## Creating a Part

Parts are created using `PartDefinition`.

A `PartDefinition` contains all the information required to create a part.

Example:

```csharp
PartDefinition PottedPlantDefinition = new()
{
    Name = "Potted Plant",
    Description = "Potted plant!!!!!!!",

    Prefab = Loader.GetAsset<GameObject>("PottedPlant"),
    Thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),

    Size = Vector3.one,
    Mass = 1f,

    Snapping = SnappingStyle.PreciseOnAny
};
```

The `PartDefinition` contains the information required to create the part,
including its name, assets, size, mass, snapping behavior, and optional
building costs.

For a complete list of available properties, see:

[PartDefinition Reference](part-definition-reference.md)

## Registering Parts

Parts are registered using:

```csharp
StellarModdingAPI.Plugin.RegisterPart(PottedPlantDefinition);
```

Registered parts are stored as `PartDefinition` objects internally. When `Build()` is called, these definitions are converted into game-compatible parts and added to StellarDrive.

## Building the Part Registry

After registering all your parts, build the registry:

```csharp
StellarModdingAPI.Plugin.Build();
```

This converts the registered part definitions into game-compatible parts and adds them to StellarDrive's build menu.

## Cleaning the Registry

When leaving the game scene, clean the registry:

```csharp
StellarModdingAPI.Plugin.Clean();
```

This removes the generated parts from the current game session.

## Complete Example

A complete part mod looks like this:

```csharp
using MelonLoader;
using Ship.Interface.Model;
using StellarModdingAPI.Assets;
using StellarModdingAPI.Items;
using StellarModdingAPI.Parts;
using UnityEngine;

namespace SDAPITest
{
    public static class BuildInfo
    {
        public const string Name = "Example Parts Mod";
        public const string Description = "Stellar Modding API part test mod";
        public const string Author = "Jollyname";
        public const string Company = null;
        public const string Version = "1.0.0";
        public const string DownloadLink = null;
    }

    public class SDAPITest : MelonMod
    {
        public static AssetLoader Loader;

        public override void OnLateInitializeMelon()
        {
            Loader = new AssetLoader(
                MelonAssembly.Assembly,
                LoggerInstance,
                new[]
                {
                    "PottedPlant", "PottedPlantThumbnail"
                }
            );
        }

        public override void OnSceneWasLoaded(int buildIndex, string sceneName)
        {
            if (buildIndex != 1) return;

            StellarModdingAPI.Plugin.Initialize();

            PartDefinition PottedPlantDefinition = new()
            {
                Name = "Potted Plant",
                Description = "Potted plant!!!!!!!",

                Prefab = Loader.GetAsset<GameObject>("PottedPlant"),
                Thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),

                Size = Vector3.one,
                Mass = 1f,

                Snapping = SnappingStyle.PreciseOnAny,
                BuildingCost =
                [
                    ItemCost.Of(100, 2)
                ]
            };

            StellarModdingAPI.Plugin.RegisterPart(PottedPlantDefinition);

            StellarModdingAPI.Plugin.Build();
        }

        public override void OnSceneWasUnloaded(int buildIndex, string sceneName)
        {
            if (buildIndex != 1) return;

            StellarModdingAPI.Plugin.Clean();
        }
    }
}
```

**Congratulations**! You have created your first custom StellarDrive part.