# Assets

The StellarModdingAPI provides tools for loading custom Unity assets from AssetBundles included with a mod.

Assets are commonly used when creating custom parts, such as:

- Part prefabs
- Part thumbnails
- Custom models and textures
- Other Unity objects supported by AssetBundles

## Asset Workflow

Using custom assets usually follows these steps:

1. [Create an AssetBundle in Unity](creating-asset-bundles.md)
2. [Embed the AssetBundle in your mod assembly](embedding-assets.md)
3. Create an [`AssetCollection`](asset-collection-reference.md)
4. Retrieve assets by name using `GetAsset<T>()`

Optionally, you can use [`AssetUtilities`](asset-utilities-reference.md) to generate the asset name list automatically instead of writing it by hand.

## Related Guides

For an example of using assets in a custom part, see:

[Creating a Part](../parts/creating-a-part.md)