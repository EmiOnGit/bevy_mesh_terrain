
 ## Bevy Mesh Terrain


### Installing
```
cargo add bevy_mesh_terrain 
```


NOTE: Make SURE you copy the terrain.wsgl shader into your assets directory properly!  Otherwise your terrain will not render. 

 
### Description 

 A very bevy-centric terrain plugin that takes advantage of entities, components and systems as much as possible to be as easy to understand and interact with as possible. 
 
 You spawn an entity and give it the 'TerrainConfig' and 'TerrainData' components, and then the plugin systems will spawn child entities which are each of the rendered chunks. 
 In this way, it works similarly to a voxel chunking system ( a la minecraft) except using heightfields (2d) instead of voxels (3d). 
 

![image](https://github.com/ethereumdegen/bevy_mesh_terrain/assets/6249263/d43e21c6-d533-4195-94ab-fb40d78f404c)



  
 
 
 ## How it works 
 
 ( See examples folder )
 
 1. You load a heightfield image into bevy asset server (R16 format - single color channel of 16 bits per pixel) 
 
 2. Pass this handle into this terrain plugin so that it will generate the heightfield data (Note: you could also set the heightfield data yourself manually)
 
 3. The plugin systems automatically spawn 'chunk' entities by sampling the heightfield data.  Chunks are only built and spawned when they are near the TerrainViewer component so attach that to your main camera. 


## Texture Types 

*Height Map Texture*
The source of height ! This is a grayscale image in R16 format and if values are totally dead black then the shader will actually make alpha == 1 right there so you can blow holes in the terrain for caves and such (youll prob want to put rock doodads there to make it look nice!)

*Color/Diffuse Texture Array*
This is the source of the actual terrain textures. Up to 256 textures can be loaded in and painted with the splat map. So for example, grass, dirt, snow, and sand. You can edit that file of course for different looks. 

*Splat Map Texture*
This is an RGBA control map texture for the diffuse texture array using a special data protocol. This is what you edit in a 'terrain editor' to paint your grass in one area, dirt in another area, snow on mountains, etc.


## Splat Map Texture Protocol

The splat map now supports up to 255 textures in a single material by using inspiration from 'Microsplat' documentation.  The limitation is that only 2 textures can be blended together at any given pixel.

Splat map R channel - A float from 0.0 to 255.0 which is converted to an integer from 0-254 to specify the texture (of the 255) to use for this pixel on layer 0
Splat map G channel - A float from 0.0 to 255.0 which is converted to an integer from 0-254 to specify the texture (of the 255) to use for this pixel on layer 1
Splat map B channel - A float which is used to specify how much of the R channel layer to use here (0.0) versus how much of the G channel layer to use here (1.0).
Splat map A channel - not used. 

 


## Bevy versions

Currently supports bevy 0.12  

 

### Reference Material 
see https://github.com/bevyengine/bevy/blob/main/examples/shader/shader_material.rs

 
 
 
