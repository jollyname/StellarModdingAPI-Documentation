# Terrain Styles Reference

`TerrainStyle` is a set of built-in terrain presets used by `SimplePlanetFactory.Spawn`.

Namespace:

```csharp
using StellarModdingAPI.Planets;
```

## Values

| Value          | Description                                                          |
| -------------- | ---------------------------------------------------------------------- |
| `EarthLike`    | Boring terrain with moderate mountains, perfect climate for beans. The default style. |
| `Mountainous`  | Tall mountains.                                                       |
| `Desert`       | Low, wide dunes.                                                      |
| `IcyFlat`      | Mostly flat terrain.                                                  |
| `Alien`        | Exaggerated, chaotic terrain, doesn't look like Earth at all.         |

Example:

```csharp
SimplePlanetFactory.Spawn(
    sourcePlanetName: "442 Otrera b",
    name: "Bean Planet",
    radius: 320_000f,
    terrainStyle: TerrainStyle.Alien
);
```

!!! note

    Each preset also clamps the resulting terrain height to a style-appropriate range (for example, `Desert` and `IcyFlat` stay much flatter than `Mountainous` or `Alien`).

!!! note

    If you need more control than these presets offer, use `PlanetSpawnRequest.TerrainOverride` directly with `PlanetFactory.Spawn` instead of `SimplePlanetFactory`.
