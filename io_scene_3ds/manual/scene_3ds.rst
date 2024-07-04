************
Autodesk 3DS
************

.. reference::

   :Category: Import-Export
   :Menu: :menuselection:`File --> Import/Export --> Autodesk 3DS (.3ds)`
   :Version: 2.7.0
   :Blender: 4.2
   :Authors: Bob Holcomb, Campbell Barton, Sebastian Schrand
   :Maintainer: Sebastian Sille (NRGSille)
   :Support Level: Community


Usage
=====

This add-on can be used to import and export objects to/from the 3DS Studio file format,
the native file format of the 3D Studio DOS releases R1 to R4.
This format is one of the first 3D file formats beside OBJ
and was commonly used to exchange files from Autodesk\ :sup:`®` 3ds MAX\ :sup:`®`.


Properties
==========

Import
------

Importing .3ds files is also possible by simply drag & drop the file from a desktop window.
Hold shift or ctrl to select multiple files for import.

Include
^^^^^^^

Image Search
   This enables a recursive file search if an image file can't be found.

Object Filter
   The kind of objects to be imported, checked object types will be imported and unchecked not.
   Hold shift while selecting to check multiple object types.

Animation
   Reads the keyframe tracks from a 3ds file and transforms the objects to the data which was found.
   Usually only one frame is found in static scenes, it will be imported to the timeline.
   If the 3ds scene is animated, the complete animation will be imported to the timeline.

Collection
   Creates a new collection for the imported file. This is useful for importing multiple objects at once
   to get a better overview in the outliner.

Cursor Origin
   Reads the 3D cursor location chunk if one is found. Almost all valid 3ds files including this chunk,
   but with the location set to zero.

Transform
^^^^^^^^^

Constrain Size
   Scales the imported objects by 10 scene units until it reaches the size defined here.
   To disable set the *Size Constraint* to zero.

Scene Units
   Converts the scale of all objects to the scene unit length settings. Blender uses meter scale,
   but many 3ds files have millimeter unit scale, especially the ones exported from CAD applications.
   If millimeters are expected to import, set the scene unit length settings to *Millimeters*.
   The meshes can also be converted to imperial unit measures if this is enabled in the scene units.

Apply Transform
   Applies object transformations after importing. If unchecked, all transformations will be cleared
   and the objects will stay at its origins.

Forward / Up Axis
   Since many applications use a different axis for 'Up',
   these are axis conversions for Forward and Up axes -- By mapping these to different axes
   you can convert rotations between applications default up and forward axes.
   Blender uses Y forward, Z up (since the front view looks along the +Y direction).
   For example, its common for applications to use Y as the up axis, in that case -Z forward, Y up is needed.


Export
------

It is recommended to apply all transformations (especially rotation and scale) before exporting,
transformations in 3ds are part of animations and may not be correctly imported again without apply transform.

Include
^^^^^^^

Selection
   When checked, only selected objects are exported. Otherwise export all objects in the scene.

Object Filter
   The kind of objects to be exported, checked object types will be exported and unchecked not.
   Hold shift while selecting to check multiple object types.

Hierarchy
   Preserves the object hierarchy if no keyframe section is written. Blender can read the hierarchy chunks
   but most importers do not use them, therefore only recommended if the file is used in Blender only.

Animation
   Writes the keyframe section of a 3ds file and exports the animation if an action was found.
   The animation can be imported the same way, un-check if any importer crashes,
   not every application can handle the keyframe section.

Collection
   Instead of exporting the complete scene, only the active collection will be exported.

Cursor Origin
   Saves the current 3D cursor location of the scene to a chunk, the importer can read the location,
   if the option is enabled.

Transform
^^^^^^^^^

Scale Factor
   The global scale factor for export. There are no unit scale definitions in a 3ds file,
   only the float values are stored. Blender will use meters for export but many applications,
   like 3ds MAX\ :sup:`®`, are using millimeters. This option defines the scale factor to use for export.
   If millimeters are desired, the scale factor has to be setted to 1000.

Scene Units
   Takes the scene unit length settings into account to export the real size of the objects.
   If the settings are millimeters, the exported scene will be scaled up since Blender uses meters for unit scale.
   Also imperial unit measures are supported, the exporter will convert the mesh to the selected scene unit.

Apply Transform
   Applies object matrix transformations before exporting. If unchecked, no transformations will be applied
   to the objects.

Forward / Up Axis
   Since many applications use a different axis for pointing upwards,
   these are axis conversion for these settings, Forward and up axes -- By mapping these to different axes
   you can convert rotations between applications default up and forward axes. Blender uses Y forward,
   Z up (since the front view looks along the +Y direction).
   For example, it is common for applications to use Y as the up axis, in that case -Z forward, Y up is needed.


Materials
=========

Materials in 3ds are defined in various color and percent chunks which can include
either integer percent and 24bit color values or float color and percent values,
both can be read by the importer and will be converted to blender values.
The exporter uses the integer values, since this is used from 3ds version 3 and above.
The material definitions which Blender can use are the following:

- 3ds Diffuse Color <-> blender Base Color
- 3ds Specular Color <-> blender Specular Tint
- 3ds Ambient Color <-> blender Emission Color
- 3ds Mat Shininess <-> blender Roughness inverted
- 3ds Mat Shin2 <-> blender Specular Intensity
- 3ds Mat Shin3 <-> blender Metallic
- 3ds Mat Opacity <-> blender Alpha inverted
- 3ds Mat TransFalloff <-> blender Transmission
- 3ds Mat ReflectBlur <-> blender Coat Weight
- 3ds Mat TextureBlur <-> blender Sheen Weight
- 3ds Mat Bump PCT <-> blender Normal-map Strength
- 3ds Self Illumination PCT <-> blender Emission Strength


Textures
--------

Each 3ds material can include different texture mappings,
which are all imported to Blender material nodes including texture coordinates.
The 3ds exporter basically takes the images and coordinates,
which are directly connected to the Principled BSDF shader,
if an image is connected to a color-mix shader, it will exported as secondary texture.
Shininess maps to roughness and opacity to the alpha channel,
they must be color inverted afterwards to match with Blender definition.
The material mappings are defined as following:

- 3ds Diffuse Map <-> blender Base Color Texture
- 3ds Specular Map <-> blender Specular Tint Texture
- 3ds Shininess Map <-> blender Roughness Texture
- 3ds Reflection Map <-> blender Metallic Texture
- 3ds Opacity Map <-> blender Alpha Texture
- 3ds Self Illumination Map <-> blender Emission Texture
- 3ds Bump Map <-> blender Normal Map (tangent space)
- 3ds Tex2 Map <-> blender Color Texture (connect to mix-shader)

.. figure:: /images/addons_io_3ds_shader-nodes.jpg

   An example of a 3ds file with all image maps imported.

.. note::

   All texture filenames are limited to the 8.3 DOS format,
   means that the name of the image texture can only be 8 characters long, others will be stripped away.


Meshes
======

Meshes are made of triangles only, no quads are supported,
3ds Studio uses edge visibility flags to hide and show edges, many 3ds files use them to mark the quads.
The Blender 3ds importer and exporter will use those flags to mark edges sharp,
this can be used to convert the triangles back to quads.
The importer can read the smooth-chunk and shades a face smooth if it belongs to a smooth-group,
the exporter creates a smooth chunk if the mesh contains any smooth faces.
3ds only supports one pair of UV coordinates per vertex. If any vertex has more UVs, it will be duplicated.


Ambient
=======

Ambient chunks are interpreted as world nodes in blender. The importer creates a node setup for each chunk 
in order to reproduce the 3ds settings as accurately as possible. Ambient animation keyframes will be imported 
to the timeline, using the world color and a RGB node connected to a emission with a mixshader for the background color. 
The mix shader will be connected to the world output node. If a background image is found, it will be connected to the 
background node and if fog chunks are found, volume shaders with the fog settings will be connected to the world 
volume output. The exporter can export these settings by using a specific node for each chunk to export. Ambient color 
animations can primary be exported from the world color. If nodes are used, the exporter checks the RGB input node and 
the emission shader for color animations and writes an ambient track node chunk. Distance cue can be exported from a 
map range node using "From Min" for near distance, "From Max" for near dimming and "To Min" for far dimming and 
"To Max" for far distance. The following world nodes can be used for ambient chunk export, the output of the node 
has to be connected to a valid input:

- 3ds Ambient Light <-> blender World Color
- 3ds Ambient Keyframe <-> blender RGB Node
- 3ds Ambient Color <-> blender Emission Shader
- 3ds Solid Background <-> blender Background Shader
- 3ds Background Bitmap <-> blender Texture Environment
- 3ds Gradient Background <-> blender ColorRamp Node
- 3ds Fog Definition <-> blender Volume Absorption
- 3ds Layered Fog <-> blender Volume Scatter
- 3ds Distance Cue <-> blender MapRange Node

.. figure:: /images/addons_io_3ds_world-nodes.jpg

   An example of a 3ds file with all world nodes imported.


Lights
======

Lights in 3DS Studio can be a point source or a spotlight,
they use color and energy data and a target for the spotlight.
The color and position of a light can be animated, the spotlight additionally has a target, beam angle and hot-spot,
which can be animated. The lights and animation can be imported and exported, the spotlight can contain a projection
bitmap, if an image is connected to a emission or colormixer, it will be exported. If a projection image has been
found by the importer, it will be connected to a colormix node together with a RGB node for the color animation.
The x/y scale of a spotlight will be exported in an aspect ratio chunk,
the importer can calculate it back to x/y scale.
The target data is calculated to Z and X axis angle for pan and tilt, Y is used for the roll angle.

.. figure:: /images/addons_io_3ds_light-nodes.jpg

   An example of a 3ds file with all light nodes imported.


Cameras
=======

Cameras can be imported and exported to 3ds files.
They can be animated with field of view (converted to focal length), position and target data,
calculated to X and Z axis angle for pitch and yaw, Y is used for the roll angle.


Keyframes
=========

The importer can read the keyframes, they will be added to the timeline.
Most animations will play, but the transformations may not be correct,
some axes or rotations can be inverted. It depends on how it was exported from other applications.
The exporter can write the keyframes of the timeline to an animated 3ds file.
