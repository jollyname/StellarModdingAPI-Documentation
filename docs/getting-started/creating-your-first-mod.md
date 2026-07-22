# Creating Your First Mod

In this guide, you will create a simple StellarDrive mod using the StellarDrive Mod Template.

The template contains a basic mod that writes a message to the MelonLoaderconsole when the game scene is loaded. We will configure it, build it, and verify that it is working.

## Setting Up the Template

Download the StellarDrive Mod Template:

[StellarDrive-ModTemplate](https://github.com/jollyname/StellarDrive-ModTemplate){ target="_blank" }

After downloading the template:

1. Open the solution in Visual Studio 2022.
2. Rename the solution and project to your mod's name.
3. Open `Main.cs`.
4. Rename the namespace to match your mod name.
5. Rename the main mod class to match your mod name.
6. Fill in the `BuildInfo` information.

Your `Main.cs` should look similar to:

```csharp
using MelonLoader;

namespace MyFirstMod
{
    public static class BuildInfo
    {
        public const string Name = "My First Mod";
        public const string Description = "My first StellarDrive mod";
        public const string Author = "Your Name";
        public const string Company = null;
        public const string Version = "1.0.0";
        public const string DownloadLink = null;
    }

    public class MyFirstMod : MelonMod
    {
        public override void OnSceneWasLoaded(int buildIndex, string sceneName)
        {
            // This checks if the game scene loaded
            if (buildIndex != 1) return;

            MelonLogger.Msg("This mod seems to work");
        }

        public override void OnSceneWasUnloaded(int buildIndex, string sceneName)
        {
            // This checks if the game scene unloaded
            if (buildIndex != 1) return;
        }
    }
}
```

## Updating AssemblyInfo

Open:

```
Properties/AssemblyInfo.cs
```

Replace the old template class names with your new mod class name.

For example:

```csharp
ModTemplate.BuildInfo
```

becomes:

```csharp
MyFirstMod.BuildInfo
```

and:

```csharp
ModTemplate.ModTemplate
```

becomes:

```csharp
MyFirstMod.MyFirstMod
```

This allows MelonLoader to correctly identify your mod.

## Understanding the Mod Class

Every StellarDrive mod inherits from `MelonMod`.

```csharp
public class MyFirstMod : MelonMod
{
}
```

MelonLoader automatically creates an instance of this class when the game starts.

## Responding to Game Events

The template overrides two methods:

```csharp
public override void OnSceneWasLoaded(int buildIndex, string sceneName)
{
    if (buildIndex != 1) return;

    MelonLogger.Msg("This mod seems to work");
}

public override void OnSceneWasUnloaded(int buildIndex, string sceneName)
{
    if (buildIndex != 1) return;
}
```

`OnSceneWasLoaded` is called whenever a scene is loaded.

The example checks that the loaded scene is the main game scene before running any code.

```csharp
if (buildIndex != 1) return;
```

This prevents your mod from running in other scenes, like the main menu.

## Testing the Mod

Build the project.

Copy the generated DLL into the `Mods` folder.

Launch StellarDrive.

If everything is working correctly, the MelonLoader console should display:

```text
This mod seems to work
```

**Congratulations**! You have successfully created and loaded your first StellarDrive mod.

In the next section, you'll use the StellarModdingAPI to create your first custom part.

[Creating a Part](../parts/index.md)