# Project Report for End of Summer 2021
### _8/23/2021 - Sage Livingstone_

This document stands as both a report of progress and lessons learned, as well as containing useful information going forward as for how to expand and maintain the project. Further maintenance documentation will be written shortly to explain the process for taking a model from our Blender & extension environment into a simulator that reads URDFs/other robotics formats.

_The words 'scene' and 'environment' will be used interchangebly in this document._

## [1] Outcome & Results

### Project Summary

The result of this project is a toolchain that allows for straighforward and intuitive creation of robotics environments for use in simulators and visualizing programs. The toolchair uses common open-source software with extensions to allow for a new workflow from modeling and texturing to physical properties (such as mass and inertial values). While this kind of workflow is relatively new, it is well-embedded withing already existing software to ensure that the process does not become outdated. The end result is the ability to create scenes such as the following example:

![Image]()

More details on the process can be found in the documentation, however a quick outline of the steps in creating a scene are described here:

1. A layout for a scene (walls, large static objects, etc) is modeled in `Blender` and a static hierarchy is created for these objects.
2. Dynamic objects are added to the scene (either pre-made or modeled) and added to the hierarchy.

    2a. Objects with moving parts must have pieces detached and articulations defined. This requires some work to find suitable models, define constraints, and set inertial values. However, once created these objects can then be re-used in other scenes.

3. Textures are applied in `Blender` to any objects that do not have them, and care must be taken to ensure that UVs and images are properly attached to output models.
4. Masses, inertial matrices, and collision objects are defined for each object. The skeletal hierarchy that underlies the scene 'robot' must be checked for consistency, and objects from different layout groups are split into their own 'robots'. This is done with the help of the `Phobos` Blender add-on.
5. `Phobos` is then used to export the scene in a desired format. (eg. `URDF` or `SDF`) Care must be taken to ensure that all the files and folders are in a layout that the importing program expects.
6. The resulting files are then imported into the program of choice, and errors are checked to ensure that scene was created correctly. (Common errors include: bad relative file pathing convention, object clipping, bad inertial values, texture and UV issues, etc.)

This method is both much faster than any alternative methods found (most URDF-type scenes are assembled in XML by hand) and offers a greater degree of flexibily to creation (ability to iterate on models, many formats to export, ability to 'pre-rig' objects with articulation and import them into scene files). However, storing scenes as 'robots' (in reference to their data representation/file format) is a somewhat novel concept and can result in unexpected behavior at time, especially when importing into new un-tested programs. As described in a later section, this tradeoff was made simply because there was not a better option available, as a 'universal' format for scenes was a main point for this project.

Building these scenes as robots allows for great visualization of certain properties of the scene that are typically modified in a text editor.


## [2] Project Description & Work Done

The original proposal for this summer project was along the following:

    To create a library of virtual environments that could be used within future robotics simulations and tests.

This goal raises several basic requirements for a scene that could be used fluidly and intuitively within our simulators. It is essential that each scene:

* Contains sufficiently-detailed 3D models and textures to identify objects.
* Models are separated (detached) in such a way that objects may be moved around the scene freely.
* Actuated parts of the scene (doors, levers, buttons, ...) are described in the limits and parameters of their articulation. This includes detached objects which require mass and inertial values.
* Contents and layout of the scene must be easy to modify long after their creation.
* Scenes may contain different layouts of objects within them, for example a kitchen refridgerator may be empty, 50% full, or completely packed.
* The scenes exist in a format friendly to all destination programs/simulators. (This was a large contingency point, which will be further discussed.)

### The source of scenes

A direct question that immediatly arises from this proposal is the option to use existing scenes from databases around the internet. Much time was spent exporing possible datasets with the purpose of configuring them for our purposes, however after testing many of these they all failed more than one of the following ways:

1. Existed in a file format that was difficult/impossible to move between simulators.
2. Did not have articulation defined and/or objects were not separate from the environment (scene was all one mesh)
3. Had questionable quality of meshes, likely arising from most of these meshes deriving from 3D-Scans of actual rooms, whose point clouds were then converted to triangles (resulting in overly-complex geometry).

And lastly, (and most commonly,)

4. Contained little-to-no variety between scenes besides slight object variations and texture changes.

This last point was discussed early in the summer when determining the overall purpose of our scenes, and it became clear that having '1,500 variations of an American-style kitchen' was not a top priority for our experiments. For this reason, among the other problems listed, it was determined that we would need to create our own scenes to arrive at the desired variety in situations to run robotics experiments.

### The file format, and Blender

Of great importance to this project was the implications of transferring the scenes from wherever they were created, into whichever simulator or program we needed. A vast amount of time early in the project was spent exporing what file formats are common to the simulators, and testing the limitations of each of them. In summary, it was discovered that a single **universal** file format for robotics scenes *does not yet exist*, and most of the existing formats each have strict and binding limitations.

As such, and given the requirement of later modification to scenes, the accepted solution became to store the scenes in a master file at the highest-possible data description, and find a way to convert those scenes to any simulator as needed. The modeling program [Blender](https://www.blender.org/) was chosen to create the scenes, as it is an extremely powerful modeling program with add-ons that allow exporting to various robotics formats, importantly `URDF`. This allows us to store the scenes in the very-descriptive `.blend` format, and the Blender robotics exporter (Phobos) will pull as much important information from the scene as possible upon export to a different format.

### The Phobos Add-On

[Phobos](https://github.com/dfki-ric/phobos) is a community-driven Blender Add-on, created with the intention to allow easy creation of robots within the modeling program. Of interest, Phobos allows the description of robot `joints` and `links`, as well as allowing exports to formats such as `URDF`, `SDF`, and `SMURF`. 

The Phobos+Blender combo was chosen as there are very few other viable options for creating hierarchical models with articulation. These options included Solidworks (good alternative, but issues with more complex texture and scene features), and Unity (much better for import of URDF than export), however most simulated robots seem to be created by hand in the XML-style `URDF` format. As the desired environments would be complex and contain many objects, creating these URDF files by hand (without the aid of a 3D program) was unacceptable. Phobos was also chosen as it uses the flexible `Blender Python API`, which would allow for future modifications if needed.

### The URDF file format

A quick note must be made on the primary export file format, `URDF`. Chosen for its frequency as a supported format in many robotics simulators, URDF is the closest thing to a universal standard in the robots simulation world. However, this universality acts as both a blessing and a curse; The standards developers have chosen to be quite conservative with adding features to the format in order to preserve compatibility. 

Due to this restrictive nature of the format, many derivitive formats have been created that extend the features of URDF. (`SDF`, `SMURF`, `SRDF`, the `iGibson` format, many yaml spin-offs, ...) Therefore, it is safe to say that much information is lost in conversion to `URDF`, although it has thus far proven almost-sufficient for our scenarios. As a viable alternative does not yet exist, it will remain the primary target for exporting of our scenes.


## [3] Lessons Learned


## [4] Self-Evaluation