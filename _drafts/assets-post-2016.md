## Update

I'll continue my blog with a series of posts - discuss steps taken in asset development, helping me determine some sort of template/pipeline for future creation of assets. 

I'm now happily married. Goal is to make $10 dollars through the Unity Asset Store by 2017!

## Panel Meshes

I'll jump right into things. Don't focus on grand-scheme / presentation of this asset (persian jewel panels, configurable in textures, Editor Script to generate mesh connections, panels snap together like a puzzle). 

I need to get some mesh constructed in Blender properly setup with a UV map.

Things to consider:

1. Scales to Unity measurements (looks nice at character's eye-level)
2. Consider various adaptations of asset. Fits other graphic-styles offered by Unity
3. Consider other game engines (Corona)

**11/03/16**

1. In Substance, export heightmap to png. Import into Blender (images as planes). Apply Displace modifier to the Plane *Texture: \<heightmap\>*

**11/10/16**

2. From plane, arrange geometry so that 4 edges are coming from each corner and meeting at a vertex in the center of the plane. Like this: [X]
  (I have decided to go with this pyramid shape so that I can better setup this workflow. Examine when Jamin Shoulet adds geometry to his planes, a ridge, irregularity. How does he correct his UV unwrap to adapt to the texture being mapped to his mesh? How does he eliminate stretching? Modify his workflow so that it works for my pyramid shape.)

**11/11/16**

3. *(following Jamin workflow)* In Blender, select object in 3d View. In Materials (properties), create a new material. In Textures, create a new texture and assign an image. In the Textures properties, scroll down to Mapping and make sure Coordinates = UV. Back in 3d View, set Viewport Shading to Texture. For the object newly assigned a material/texture, enter Edit Mode and select all geometry. UV Mapping --> Unwrap.