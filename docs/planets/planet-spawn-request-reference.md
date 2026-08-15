# PlanetSpawnRequest Reference

`PlanetSpawnRequest` contains all the information required by the StellarModdingAPI to clone and spawn a custom StellarDrive planet.

Namespace:

```csharp
using StellarModdingAPI.Planets;
```

Example:

```csharp
var request = new PlanetSpawnRequest
{
    SourcePlanetName = "442 Otrera b",
    Id = 691_346_321,
    Name = "Bean Planet",
    Radius = 320_000f,
    PositionOffset = new Vector3(0f, 0f, 8_000_000f)
};
```

## Properties

### SourcePlanetName

```csharp
string SourcePlanetName
```

The name of the existing in-scene planet to clone.

Required. If no loaded planet matches this name, `PlanetFactory.Spawn` logs an error and returns `null`.

Example:

```csharp
SourcePlanetName = "442 Otrera b"
```

---

### Id

```csharp
ulong Id
```

The id assigned to the new planet.

Must not collide with any existing planet's id.

Example:

```csharp
Id = 691_346_321
```

!!! note

    If you're using `SimplePlanetFactory`, this is optional; ids are auto-assigned starting at `600,000,000`.

---

### Name

```csharp
string Name
```

The name given to the cloned planet's `GameObject`.

Example:

```csharp
Name = "Bean Planet"
```

---

### Radius

```csharp
float Radius
```

The radius of the new planet. The clone's terrain is rebuilt to match this radius.

Example:

```csharp
Radius = 320_000f
```

---

### PositionOffset

```csharp
Vector3 PositionOffset
```

Offset from the source planet's own position. A zero offset spawns the clone directly on top of the source planet.

Example:

```csharp
PositionOffset = new Vector3(0f, 0f, 8_000_000f)
```

---

### TerrainOverride

```csharp
Action<TerrainConfig> TerrainOverride
```

A callback used to modify the cloned planet's terrain configuration before the terrain is rebuilt.

Optional. If `null`, the clone keeps the source planet's terrain shape (aside from the radius change).

Example:

```csharp
TerrainOverride = config =>
{
    config.noiseSteps[0].noiseConfig.strength = 900f;
}
```

---

### Materials

```csharp
TerrainMaterialConfig[] Materials
```

A replacement material palette for the cloned planet's terrain.

Optional. If omitted, the clone keeps the source planet's own textures.

Each entry is a [`TerrainMaterialConfig`](terrain-material-config-reference.md). You can construct these yourself with your own textures, or use `PlanetMaterialUtility.BuildFromDonors` to copy (and optionally re-tint) materials from existing in-scene planets. See the [PlanetMaterialUtility Reference](planet-material-utility-reference.md).

Example, using your own textures:

```csharp
Materials = new[]
{
    new TerrainMaterialConfig
    {
        albedoTexture = myGrassColor,
        normalMap = myGrassNormal,
        roughnessTexture = myGrassRoughness,
        albedoColor = Color.white,
        roughnessFloor = 0.3f,
        metalness = 0f,
        tile = 1f,
    }
}
```

Example, using donor planets:

```csharp
Materials = PlanetMaterialUtility.BuildFromDonors(
    ("442 Otrera c", 0, null)
)
```

---

### RemoveRings

```csharp
bool RemoveRings
```

Whether to remove the source planet's ring, if it has one, from the clone.

Defaults to `true`.

!!! note

    Removing the ring currently does not remove the ring's asteroids.

Example:

```csharp
RemoveRings = false
```

---

### RegisterAsServer

```csharp
bool RegisterAsServer
```

Whether to register the clone with the server-side planet tracker and spawn its `NetworkObject` on the network.

Defaults to `true`.

---

### RegisterAsClient

```csharp
bool RegisterAsClient
```

Whether to register the clone with the client-side planet tracker.

Defaults to `true`.

!!! note

    In most cases you'll want both `RegisterAsServer` and `RegisterAsClient` enabled so the planet is fully tracked on both sides.