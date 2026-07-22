# Items

This page contains the item IDs available in StellarDrive.

Item IDs are used when creating building costs for custom parts with `ItemCost`.

Example:

```csharp
BuildingCost =
[
    ItemCost.Of(100, 5)
]
```

The first value is the item ID, and the second value is the required amount.

## Available Items

| ID  | Name          |
| --- | ------------- |
| 0   | Build Tool    |
| 1   | Recycler Tool |
| 2   | Mining Laser  |
| 100 | Fuel          |
| 101 | Iron          |
| 102 | Glass         |
| 103 | Exotic Matter |
| 104 | Aluminum      |

## Finding Item IDs

The StellarModdingAPI can display all available items and their IDs while the game is running.

Press: `F8`

The item list will be printed to the MelonLoader console.

Use these IDs with ItemCost.Of() when creating custom part building costs.