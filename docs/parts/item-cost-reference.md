# ItemCost Reference

`ItemCost` is a helper class used to create item requirements for custom
parts.

It converts an item ID and an amount into the game's internal `ItemInstance`
format, which is then used by `PartDefinition.BuildingCost`.

Namespace:

```csharp
using StellarModdingAPI.Items;
```

## Creating an Item Cost

Item costs are created using:

```csharp
ItemCost.Of(uint id, int amount)
```

Example:

```csharp
ItemCost.Of(100, 5)
```

This creates a building requirement of:

- Item ID: `100`
- Amount: `5`

The item ID is used to find the corresponding item registered by StellarDrive.

For a list of available item IDs, see the [Item IDs reference](item-ids-reference.md).

## Multiple Costs

A part can require multiple items by adding multiple `ItemCost` entries.

Example:

```csharp
BuildingCost =
[
    ItemCost.Of(100, 5),
    ItemCost.Of(101, 2)
]
```

This creates a part requiring:

- 5 of item ID `100`
- 2 of item ID `101`

## Complete Example

```csharp
PartDefinition PottedPlantDefinition = new()
{
    Name = "Potted Plant",
    Description = "A decorative plant",

    Prefab = Loader.GetAsset<GameObject>("PottedPlant"),
    Thumbnail = Loader.GetAsset<Texture2D>("PottedPlantThumbnail"),

    Size = Vector3.one,
    Mass = 1f,

    Snapping = SnappingStyle.PreciseOnAny,

    BuildingCost =
    [
        ItemCost.Of(100, 2),
    ]
};
```