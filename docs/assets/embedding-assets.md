# Embedding Assets in Your Mod

`AssetLoader` expects each AssetBundle to be included in your mod's assembly
as an **Embedded Resource**. This packages the bundle directly into your
compiled `.dll`, so no extra files need to be shipped alongside it.

!!! info

    This guide assumes you're using the
    [StellarDrive-ModTemplate](https://github.com/jollyname/StellarDrive-ModTemplate),
    which is an SDK-style project (`<Project Sdk="Microsoft.NET.Sdk">`).

## 1. Add the AssetBundle File to Your Project

Copy the AssetBundle file you built in Unity (e.g. `pottedplant`) into
your mod project's folder, for example:

```
ModTemplate/
├── Assets/
│   └── pottedplant
├── Main.cs
└── ModTemplate.csproj
```

## 2. Embed It via the .csproj

Open `ModTemplate.csproj` and add an `EmbeddedResource` entry pointing at
the file:

```xml
<ItemGroup>
  <EmbeddedResource Include="Assets\pottedplant" />
</ItemGroup>
```

The full file will look something like this:

```xml hl_lines="11 12 13 15 16 17"
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
    <LangVersion>latest</LangVersion>
    <Nullable>enable</Nullable>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <OutputPath>$(SolutionDir)Output\</OutputPath>
  </PropertyGroup>

  <ItemGroup>
    <None Remove="Assets\pottedplant" />
  </ItemGroup>

  <ItemGroup>
    <EmbeddedResource Include="Assets\pottedplant" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ReferencedAssemblies\MelonLoader.dll" />
	<Reference Include="ReferencedAssemblies\UnityEngine.CoreModule.dll" />
	<Reference Include="ReferencedAssemblies\UnityEngine.dll" />
  </ItemGroup>

</Project>
```

## Next Step

Continue to the [AssetLoader Reference](asset-loader-reference.md) to load
the assets from the embedded bundle.
