# Creating a Part

This guide will walk you through creating a custom part and adding it to the StellarDrive build menu.

A custom part requires:

* A model prefab
* A thumbnail
* Basic properties
* Optional building costs

Parts are created using a `PartDefinition` and registered through the API. The API automatically adds registered parts to StellarDrive's build menu.

## Loading Assets

Before creating a part, you need to load the assets used by the part.

The StellarModdingAPI provides `AssetLoader` for loading assets from Unity AssetBundles.

Example:

```csharp
Loader = new AssetLoader(
    MelonAssembly.Assembly,
    new[]
    {
        "PottedPlant", "PottedPlantThumbnail"
    },
    LoggerInstance
);
```

The asset names passed to `AssetLoader` are the names of the assets that your mod will use later.

In this example:

* `PottedPlant` is the part's model prefab.
* `PottedPlantThumbnail` is the image displayed in the build menu.

## Creating a Part

Parts are created using `PartDefinition`.

A `PartDefinition` contains all the information required to create a part.

Example:

```csharp
PartDefinition PottedPlantDefinition = new(
    Name: "Potted Plant",
    Description: "Potted plant!!!!!!!",
    Prefab: Loader.GetAsset<GameObject>("PottedPlant"),
    Thumbnail: Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),
    PhysicalSize: Vector3.one * 0.25f,
    Mass: 1f,
    LogicalSize: Vector3.one,
    Snapping: SnappingStyle.PreciseOnAny,
    BuildingCost:
    [
        ItemCost.Of(ItemIDs.Fuel, 2)
    ]
);
```

The `PartDefinition` contains the information required to create the part, including its name, assets, physical and logical size, mass, snapping behavior, and optional building costs.

For a complete list of available properties, see:

[PartDefinition Reference](part-definition-reference.md)

## Registering Parts

Parts are registered using:

```csharp
Plugin.RegisterPart(PottedPlantDefinition);
```

Once registered, the API automatically converts the part definition into the game's internal format and adds it to StellarDrive's build menu when appropriate. No additional setup is required.

## Complete Example

A complete part mod looks like this:

```csharp
using MelonLoader;
using Ship.Interface.Model;
using StellarModdingAPI.Abstract;
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
        public const string Version = "1.1.0";
        public const string DownloadLink = null;
    }

    public class SDAPITest : StellarMelonMod
    {
        public static AssetLoader Loader;

        public override void OnLateInitializeMelon()
        {
            Loader = new AssetLoader(
                MelonAssembly.Assembly,
                new[]
                {
                    "PottedPlant", "PottedPlantThumbnail"
                },
                LoggerInstance
            );
        }

        public override void OnSceneWasLoaded(int buildIndex, string sceneName)
        {
            if (buildIndex != 1) return;

            PartDefinition PottedPlantDefinition = new(
                Name: "Potted Plant",
                Description: "Potted plant!!!!!!!",
                Prefab: Loader.GetAsset<GameObject>("PottedPlant"),
                Thumbnail: Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),
                PhysicalSize: Vector3.one * 0.25f,
                Mass: 1f,
                LogicalSize: Vector3.one,
                Snapping: SnappingStyle.PreciseOnAny,
                BuildingCost:
                [
                    ItemCost.Of(ItemIDs.Fuel, 2)
                ]
            );

            Plugin.RegisterPart(PottedPlantDefinition);
        }
    }
}
```

**Congratulations!** You have created your first custom StellarDrive part.
