# Planets

The StellarModdingAPI allows mods to create custom planets for StellarDrive by cloning an existing planet and customizing its terrain, materials, and position.

Planets are cloned from an existing in-scene planet, then registered with the game's client and server planet trackers so they behave like any other planet.

## Creating Planets

A custom planet is created using a `PlanetSpawnRequest`.

A `PlanetSpawnRequest` contains the information needed to create a planet, including:

- Source planet to clone
- New id, name, and radius
- Position offset from the source planet
- Terrain overrides
- Custom materials
- Ring removal and network registration options

Once a request is filled out, `PlanetFactory.Spawn` clones the source planet, applies the requested overrides, and registers it so it appears for players.

For simpler cases, `SimplePlanetFactory.Spawn` wraps `PlanetFactory` with a handful of terrain style presets and sensible defaults, so you don't need to build a `PlanetSpawnRequest` yourself.

## Planet Workflow

Creating a custom planet usually follows these steps:

1. Pick an existing planet to clone as your source
2. Build a `PlanetSpawnRequest` (or use `SimplePlanetFactory` for common cases)
3. Optionally build a custom material palette with `PlanetMaterialUtility`
4. Spawn the planet with `PlanetFactory.Spawn`

## Limitations

- No resource control, no support for adding resources to a clone, or stripping the ones it inherited from its source. This will get added later on.
- Position is relative to the source planet, not the star. PositionOffset and distanceFromSource place the clone relative to the source planet's own position. This is expected to move to a distance from star model in a future version.

## Guides

Start creating your first custom planet:

[Creating a Planet](creating-a-planet.md)