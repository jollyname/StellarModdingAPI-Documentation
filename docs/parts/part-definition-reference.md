# PartDefinition Reference

`PartDefinition` contains all information required by the StellarModdingAPI
to create a custom StellarDrive part.

Namespace:

```csharp
using StellarModdingAPI.Parts;
```

Example:

```csharp
PartDefinition definition = new()
{
    Name = "Potted Plant",
    Description = "A decorative plant",

    Prefab = Loader.GetAsset<GameObject>("PottedPlant"),
    Thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),

    Size = Vector3.one,
    Mass = 1f,

    Snapping = SnappingStyle.PreciseOnAny
};
```

## Properties

### Name

```csharp
string Name
```

The display name of the part shown in the StellarDrive build menu.

Example:

```csharp
Name = "Potted Plant"
```

---

### Description

```csharp
string Description
```

The description displayed when viewing the part in the build menu.

Example:

```csharp
Description = "A decorative plant"
```

---

### Prefab

```csharp
GameObject Prefab
```

The Unity `GameObject` used as visuals when the part is placed in the world, it should only contain a mesh renderer with materials and a mesh filter.

It can be loaded from an AssetBundle using `AssetLoader`.

Example:

```csharp
Prefab = Loader.GetAsset<GameObject>("PottedPlant")
```

---

### Thumbnail

```csharp
Texture2D Thumbnail
```

The image displayed for the part in the build menu.

Example:

```csharp
Thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail")
```

---

### Size

```csharp
Vector3 Size
```

Controls the occupied building space of the part in the building grid.

Example:

```csharp
Size = Vector3.one
```

`Vector3.one` creates a part occupying a `1x1x1` building space.

---

### Mass

```csharp
float Mass
```

Defines the physical mass of the part.

Example:

```csharp
Mass = 1f
```

---

### Snapping

```csharp
SnappingStyle Snapping
```

Controls how the part snaps to other building pieces.

Example:

```csharp
Snapping = SnappingStyle.PreciseOnAny
```

---

### BuildingCost

```csharp
ItemCost[] BuildingCost
```

Defines the resources required to build the part.

Example:

```csharp
BuildingCost =
[
    ItemCost.Of(100, 2)
]
```

See the [ItemCost Reference](item-cost-reference.md) for more information.