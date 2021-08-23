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
* The scenes exist in a format friendly to all destination programs/simulators. (This was a large contingency point, which will be further discussed.)

... where did the scenes come from..?

## Work Done


## Lessons Learned


## Self-Evaluation