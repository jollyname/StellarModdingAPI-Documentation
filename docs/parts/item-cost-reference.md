# ItemCost Reference

`ItemCost` is a helper class used to create item requirements for custom parts.

It converts an `ItemID` and an amount into the game's internal `ItemInstance` format, which is then used by `PartDefinition.BuildingCost`.

Namespace:

```csharp
using StellarModdingAPI.Items;
```

## Creating an Item Cost

Item costs are created using:

```csharp
ItemCost.Of(ItemID id, int amount)
```

Example:

```csharp
ItemCost.Of(ItemIDs.Fuel, 5)
```

This creates a building requirement of:

* Item ID: `Fuel`
* Amount: `5`

The `ItemID` identifies the item registered by StellarDrive.

The API provides common item IDs through the `ItemIDs` class:

```csharp
ItemIDs.Fuel
ItemIDs.Iron
ItemIDs.Glass
ItemIDs.ExoticMatter
ItemIDs.Aluminum
```

For a list of available item IDs, see the [Item IDs reference](item-ids-reference.md).

## Multiple Costs

A part can require multiple items by adding multiple `ItemCost` entries.

Example:

```csharp
BuildingCost =
[
    ItemCost.Of(ItemIDs.Iron, 5),
    ItemCost.Of(ItemIDs.Glass, 2)
]
```

This creates a part requiring:

* 5 of `ItemIDs.Iron`
* 2 of `ItemIDs.Glass`

## Complete Example

```csharp
PartDefinition PottedPlantDefinition = new(
    Name: "Potted Plant",
    Description: "A decorative plant",

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
