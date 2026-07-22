# Parts

The StellarModdingAPI allows mods to create custom parts for StellarDrive.

Parts are registered through the API and added to the game's build menu, allowing players to use custom content alongside the game's existing parts.

## Creating Parts

A custom part is created using a `PartDefinition`.

A `PartDefinition` contains the information needed to create a part, including:

- Name and description
- Model and thumbnail assets
- Size and mass
- Snapping behavior
- Building costs

Once a part is registered, the API converts the definition into the game's internal part format and adds it to the build menu.

## Part Workflow

Creating a custom part usually follows these steps:

1. Load your custom assets
2. Create a `PartDefinition`
3. Register the part
4. Build the part registry
5. Test the part in-game

## Guides

Start creating your first custom part:

[Creating a Part](creating-a-part.md)