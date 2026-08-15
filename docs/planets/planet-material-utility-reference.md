# PlanetMaterialUtility Reference

`PlanetMaterialUtility` provides helpers for building custom terrain material palettes for cloned planets by copying materials from existing planets. This is a convenience for reusing materials that already exist in the game; you can also construct [`TerrainMaterialConfig`](terrain-material-config-reference.md) entries directly if you want to supply your own textures instead.

Namespace:

```csharp
using StellarModdingAPI.Planets;
```

## BuildFromDonors

```csharp
TerrainMaterialConfig[] BuildFromDonors(params (string donorPlanetName, int materialIndex, Color? tint)[] picks)
```

Builds a material palette by copying individual terrain materials from one or more existing planets, optionally re-tinting each one.

Each entry in `picks` is a tuple:

| Value              | Type      | Description                                                                            |
| ------------------ | --------- | ---------------------------------------------------------------------------------------- |
| `donorPlanetName`   | `string`  | The name of an existing in-scene planet to copy a material from.                         |
| `materialIndex`     | `int`     | The index of the material on the donor planet's terrain to copy.                         |
| `tint`              | `Color?`  | Optional color to multiply against the material's albedo texture. Pass `null` to keep the donor's original color. |

Example:

```csharp
var materials = PlanetMaterialUtility.BuildFromDonors(
    ("442 Otrera b", 0, null),
    ("442 Otrera b", 1, null)
);

request.Materials = materials;
```

This copies material index `0` and `1` from `"442 Otrera b"` unchanged. The result is an array in the same order as `picks`, ready to assign to `PlanetSpawnRequest.Materials`.

!!! note

    Tinting multiplies the given color against the material's existing albedo texture rather than replacing it, so the result depends on the donor texture's base color.