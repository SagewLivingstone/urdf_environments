# Project Report for End of Summer 2021
### _8/23/2021 - Sage Livingstone_

This document stands as both a report of progress and lessons learned, as well as containing useful information going forward as for how to expand and maintain the project. Further maintenance documentation will be written shortly to explain the process for taking a model from our Blender & extension environment into a simulator that reads URDFs/other robotics formats.

_The words 'scene' and 'environment' will be used interchangebly in this document._

## Project Description

The original proposal for this summer project was along the following:

    To create a library of virtual environments that could be used within future robotics simulations and tests.

This goal raises several basic requirements for a scene that could be used fluidly and intuitively within our simulators. It is essential that each scene:

* Contains sufficiently-detailed 3D models and textures to identify objects.
* Models are separated (detached) in such a way that objects may be moved around the scene freely.
* Actuated parts of the scene (doors, levers, buttons, ...) are described in the limits and parameters of their articulation. This includes detached objects which require mass and inertial values.
* Contents and layout of the scene must be easy to modify long after their creation.
* Scenes may contain different layouts of objects within them, for example a kitchen refridgerator may be empty, 50% full, or completely packed.
* The scenes exist in a format friendly to all destination programs/simulators. (This was a large contingency point, which will be further discussed.)

#### The source of scenes

A direct question that immediatly arises from this proposal is the option to use existing scenes from databases around the internet. Much time was spent exporing possible datasets with the purpose of configuring them for our purposes, however after testing many of these they all failed more than one of the following ways:

1. Existed in a file format that was difficult/impossible to move between simulators.
2. Did not have articulation defined and/or objects were not separate from the environment (scene was all one mesh)
3. Had questionable quality of meshes, likely arising from most of these meshes deriving from 3D-Scans of actual rooms, whose point clouds were then converted to triangles (resulting in overly-complex geometry).

And lastly, (and most commonly,)

4. Contained little-to-no variety between scenes besides slight object variations and texture changes.

This last point was discussed early in the summer when determining the overall purpose of our scenes, and it became clear that having '1,500 variations of an American-style kitchen' was not a top priority for our experiments. For this reason, among the other problems listed, it was determined that we would need to create our own scenes to arrive at the desired variety in situations to run robotics experiments. 

## Work Done


## Lessons Learned


## Self-Evaluation