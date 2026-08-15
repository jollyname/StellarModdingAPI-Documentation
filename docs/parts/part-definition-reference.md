# PartDefinition Reference

`PartDefinition` contains all the information required by the StellarModdingAPI to create a custom StellarDrive part.

Namespace:

```csharp
using StellarModdingAPI.Parts;
```

Example:

```csharp
PartDefinition definition = new(
    Name: "Potted Plant",
    Description: "A decorative plant",
    Prefab: Assets.GetAsset<GameObject>("PottedPlant"),
    Thumbnail: Assets.GetAsset<Texture2D>("PottedPlantThumbnail"),
    PhysicalSize: Vector3.one * 0.25f,
    Mass: 1f,
    LogicalSize: Vector3.one,
    Snapping: SnappingStyle.PreciseOnAny
);
```

## Properties

### Name

```csharp
string Name
```

The display name of the part shown in the StellarDrive build menu.

Example:

```csharp
Name: "Potted Plant"
```

---

### Description

```csharp
string Description
```

The description displayed when viewing the part in the build menu.

Example:

```csharp
Description: "A decorative plant"
```

---

### Prefab

```csharp
GameObject Prefab
```

The Unity `GameObject` used as the visual representation of the part when placed in the world. It should only contain a `MeshRenderer`, its materials, and a `MeshFilter`.

It can be loaded from an AssetBundle using `AssetCollection`.

Example:

```csharp
Prefab: Assets.GetAsset<GameObject>("PottedPlant")
```

---

### Thumbnail

```csharp
Texture2D? Thumbnail
```

The image displayed for the part in the build menu.

This value is optional. If `null`, no thumbnail will be displayed.

Example:

```csharp
Thumbnail: Assets.GetAsset<Texture2D>("PottedPlantThumbnail")
```

---

### PhysicalSize

```csharp
Vector3 PhysicalSize
```

Defines the physical dimensions of the part used for collision and placement in the world.

Example:

```csharp
PhysicalSize: Vector3.one * 0.25f
```

---

### Mass

```csharp
float Mass
```

Defines the physical mass of the part.

Example:

```csharp
Mass: 1f
```

---

### LogicalSize

```csharp
Vector3 LogicalSize
```

Defines the space the part occupies in the building grid and how it snaps to other parts.

Example:

```csharp
LogicalSize: Vector3.one
```

---

### Snapping

```csharp
SnappingStyle Snapping
```

Controls how the part snaps to other building pieces.

Defaults to:

```csharp
SnappingStyle.PreciseOnAny
```

Example:

```csharp
Snapping: SnappingStyle.PreciseOnAny
```

---

### BuildingCost

```csharp
List<ItemInstance> BuildingCost
```

Defines the resources required to build the part.

Example:

```csharp
BuildingCost:
[
    ItemCost.Of(ItemIDs.Fuel, 2)
]
```

See the [ItemCost Reference](item-cost-reference.md) for more information.