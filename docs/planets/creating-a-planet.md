# Creating a Planet

This guide will walk you through creating a custom planet by cloning an existing one and customizing its terrain, position, and materials.

A custom planet requires:

* An existing planet to clone from
* A unique id
* A radius
* A position offset from the source planet

Planets are created using a `PlanetSpawnRequest` and spawned through the API. The API automatically clones the source planet, applies your overrides, and wires the clone into the game's networking and planet trackers.

## The Quick Way: SimplePlanetFactory

For common cases, `SimplePlanetFactory.Spawn` builds the request for you and offers a handful of built-in terrain style presets.

Example:

```csharp
GameObject clone = SimplePlanetFactory.Spawn(
    sourcePlanetName: "442 Otrera b",
    name: "Bean Planet",
    radius: 320_000f,
    terrainStyle: TerrainStyle.Mountainous,
    distanceFromSource: 8_000_000f
);
```

This clones the planet named `"442 Otrera b"`, applies the `Mountainous` terrain preset, and places the clone 8,000,000 meters away from `442 Otrera b` along the Z axis.

If `id` is omitted, `SimplePlanetFactory` automatically assigns one starting at `600,000,000` and incrementing for each planet spawned this way. If you're also assigning ids yourself elsewhere, make sure they don't collide with this range.

See the [Terrain Styles Reference](terrain-styles-reference.md) for the available presets.

## The Advanced Way: PlanetFactory

For full control, custom terrain tweaks, position offsets on any axis, or custom materials, build a `PlanetSpawnRequest` yourself and pass it to `PlanetFactory.Spawn`.

Example:

```csharp
var request = new PlanetSpawnRequest
{
    SourcePlanetName = "442 Otrera b",
    Id = 691_346_321,
    Name = "Bean Planet",
    Radius = 320_000f,
    PositionOffset = new Vector3(0f, 0f, 8_000_000f),
    TerrainOverride = config =>
    {
        config.noiseSteps[0].noiseConfig.strength = 900f;
    }
};
```

`PlanetFactory.Spawn` returns the cloned planet's `GameObject`, or `null` if `SourcePlanetName` couldn't be found in the scene.

For a complete list of available properties, see:

[PlanetSpawnRequest Reference](planet-spawn-request-reference.md)

!!! note

    `PositionOffset` is relative to the source planet's own position. A zero offset spawns the clone directly on top of the source, so you'll always want to set this to something.

## Custom Materials

By default, a cloned planet keeps the source planet's own textures. To use different materials, assign a `TerrainMaterialConfig[]` to `PlanetSpawnRequest.Materials`. There are two ways to build one:

**Construct materials yourself**, supplying your own textures (for example, loaded through an `AssetCollection`):

```csharp
var materials = new[]
{
    new TerrainMaterialConfig
    {
        albedoTexture = Assets.GetAsset<Texture2D>("Grass_Color"),
        normalMap = Assets.GetAsset<Texture2D>("Grass_Normal"),
        roughnessTexture = Assets.GetAsset<Texture2D>("Grass_Roughness"),
        albedoColor = Color.white,
        roughnessFloor = 0.3f,
        metalness = 0f,
        tile = 1f,
    }
};

request.Materials = materials;
```

**Or copy materials from existing planets** with `PlanetMaterialUtility.BuildFromDonors`:

```csharp
var materials = PlanetMaterialUtility.BuildFromDonors(
    ("442 Otrera b", 0, null),
    ("442 Otrera b", 1, null)
);

request.Materials = materials;
```

Each entry in `BuildFromDonors` picks a material by index from a donor planet's terrain, optionally tinting it. Use this when you want to reuse materials that already exist in the game rather than supplying your own textures. See the [PlanetMaterialUtility Reference](planet-material-utility-reference.md) and the [TerrainMaterialConfig Reference](terrain-material-config-reference.md) for details.

Both approaches also work with `SimplePlanetFactory.Spawn`, which accepts an optional `materials` parameter:

```csharp
SimplePlanetFactory.Spawn(
    sourcePlanetName: "442 Otrera b",
    name: "Bean Planet",
    radius: 320_000f,
    terrainStyle: TerrainStyle.Desert,
    distanceFromSource: 8_000_000f,
    materials: PlanetMaterialUtility.BuildFromDonors(
        ("442 Otrera c", 0, null),
        ("442 Otrera c", 0, null)
    )
);
```

## Complete Example

A complete planet mod looks like this:

```csharp
using MelonLoader;
using StellarModdingAPI.Abstract;
using StellarModdingAPI.Planets;
using UnityEngine;

namespace SDAPITest
{
    public static class BuildInfo
    {
        public const string Name = "Example Planets Mod";
        public const string Description = "Stellar Modding API planet test mod";
        public const string Author = "Jollyname";
        public const string Company = null;
        public const string Version = "1.1.0";
        public const string DownloadLink = null;
    }

    public class SDAPITest : StellarMelonMod
    {
        public override void OnSceneWasLoaded(int buildIndex, string sceneName)
        {
            if (buildIndex != 1) return;

            SimplePlanetFactory.Spawn(
                sourcePlanetName: "442 Otrera b",
                name: "Bean Planet",
                radius: 320_000f,
                terrainStyle: TerrainStyle.Mountainous,
                distanceFromSource: 8_000_000f
            );
        }
    }
}
```

**Congratulations!** You have created your first custom StellarDrive planet.