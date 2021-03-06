# URDF Robot Environments

This repo contains the source and exports of various 3D environments designed for robot simulation and visualization. Scenes are designed and constructed in Blender and represented as robot models using the Phobos plugin.

_An example scene of a kitchen modeled in Blender_
![Alt text](catalog/scenes/Kitchen/thumbnail.png)

_Underlying structure of the scene_
![Alt text](example_2.png)

### Repo Structure
```
+ Catalog - contains source objects and files for the scenes
  | - Objects - objects to use in scenes, pulled from various sources. Stored as .blend files
  | - Scenes - master .blend files of scenes containing the highest level of description
  | - Textures - source of textures used in scenes and in objects
+ Export - exported scenes from Blender using Phobos (contains URDFs, SDFs, .dae's, ...)
+ urdf_tests - some tests of various urdf features
```
