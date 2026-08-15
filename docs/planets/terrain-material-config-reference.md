# TerrainMaterialConfig Reference

`TerrainMaterialConfig` describes a single terrain material: its textures, base color, and surface properties. A `PlanetSpawnRequest.Materials` array is made up of these.

Namespace:

```csharp
using Planet.Terrain.Material;
```

You can build a `TerrainMaterialConfig[]` in two ways:

- **Construct entries manually**, supplying your own textures (e.g. loaded via `AssetCollection`). Use this when you want fully custom materials.
- **Use `PlanetMaterialUtility.BuildFromDonors`**, which copies materials (and optionally re-tints them) from existing in-scene planets. Use this when you just want to reuse or recolor materials that already exist in the game.

Both approaches produce the same `TerrainMaterialConfig[]` type and can be assigned directly to `PlanetSpawnRequest.Materials`.

## Properties

### albedoTexture

```csharp
Texture2D albedoTexture
```

The base color texture for the material.

---

### normalMap

```csharp
Texture2D normalMap
```

The normal map texture used for surface detail lighting.

---

### roughnessTexture

```csharp
Texture2D roughnessTexture
```

The roughness texture controlling how matte or shiny the surface appears across the material.

---

### albedoColor

```csharp
Color albedoColor
```

A color multiplied against `albedoTexture`. Use `Color.white` to keep the texture's original color unchanged.

---

### roughnessFloor

```csharp
float roughnessFloor
```

The minimum roughness value for the material, regardless of what `roughnessTexture` specifies at a given point.

---

### metalness

```csharp
float metalness
```

The metalness value for the material, from `0` (non-metallic) to `1` (fully metallic).

---

### tile

```csharp
float tile
```

The texture tiling multiplier applied to the material's textures across the terrain.

## Example: Building Materials Manually

```csharp
var materials = new[]
{
    // Grass
    new TerrainMaterialConfig
    {
        albedoTexture = Assets.GetAsset<Texture2D>("Grass_Color"),
        normalMap = Assets.GetAsset<Texture2D>("Grass_Normal"),
        roughnessTexture = Assets.GetAsset<Texture2D>("Grass_Roughness"),
        albedoColor = Color.white,
        roughnessFloor = 0.3f,
        metalness = 0f,
        tile = 1f,
    },
    // Sand
    new TerrainMaterialConfig
    {
        albedoTexture = Assets.GetAsset<Texture2D>("Sand_Color"),
        normalMap = Assets.GetAsset<Texture2D>("Sand_Normal"),
        roughnessTexture = Assets.GetAsset<Texture2D>("Sand_Roughness"),
        albedoColor = Color.white,
        roughnessFloor = 0.3f,
        metalness = 0f,
        tile = 1f,
    }
};

request.Materials = materials;
```

This uses textures loaded through an `AssetCollection` rather than copying from an existing planet. See the [PlanetMaterialUtility Reference](planet-material-utility-reference.md) for the donor-based alternative.