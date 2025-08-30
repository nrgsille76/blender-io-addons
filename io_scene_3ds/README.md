# Autodesk 3D Studio (.3ds)


---  


**Category**  
&nbsp;&nbsp; Import-Export  

**Menu**  
&nbsp;&nbsp; `File --> Import/Export --> Autodesk 3DS (.3ds)`  

**Version**  
&nbsp;&nbsp; 2.8.0  

**Authors**  
&nbsp;&nbsp; Bob Holcomb, Campbell Barton, Sebastian Schrand  

**Maintainer**  
&nbsp;&nbsp; Sebastian Sille (NRGSille)  

---  

## Usage

### Export:
```python
from .io_scene_3ds.export_3ds import save_3ds

save_3ds(context, filepath="", collection="", items=[], scale_factor=1.0, global_matrix=None,
         use_selection=False, use_apply_transform=True, object_filter=None, use_invisible=False,
         use_keyframes=True, use_hierarchy=False, use_cursor=False)
```
PARAMETERS:
+ `filepath` (string, (optional, never None)) – File Path, Filepath used for exporting the file
+ `collection` (string, (optional, never None)) – Source Collection, Export only objects from this collection (and its children)
+ `items` (list, (optional, never None)) - List of objects, if not empty collection will be ignored
+ `scale_factor` (float in [0.001, 10000], (optional)) – Scale Factor, Scale all data by this factor
+ `global_matrix` (mathutils.Matrix(), (optional)) - Matrix to apply, if `None`, an identity matrix is taken
+ `use_selection` (boolean, (optional)) – Selected Objects, Export selected and visible objects only
+ `use_apply_transform` (boolean (optional)) - Apply matrix transformations before export
+ `object_filter` (enum in [`WORLD`, `MESH`, `LIGHT`, `CAMERA`, `EMPTY`, `OTHER`], (optional)) - Object types to export
  - if `object filter` is `None` all types are taken, except of `WORLD`
+ `use_invisible` (boolean (optional)) - Export invisible objects
+ `use_keyframes` (boolean (optional)) - Export animation keyframes
+ `use_hierarchy` (boolean (optional)) - Export object hierarchy (use this option to keep hierarchy without keyframes)
+ `use_cursor` (boolean (optional)) - Export 3D cursor position

### Import:  
```python
from io_scene_3ds.import_3ds import load_3ds

load_3ds(filepath, context, CONSTRAIN=10.0, UNITS=False, IMAGE_SEARCH=True,
         FILTER=None, KEYFRAME=True, APPLY_MATRIX=True, CONVERSE=None, CURSOR=False)
```
PARAMETERS:
+ `filepath` (string, (optional, never None)) – File Path, Filepath used for importing the file
+ `CONSTRAIN` (float in [0.000, 10000], (optional)) – Constrain size, Scale all data to the contrain size
+ `UNITS` (boolean (optional)) - The scale unit to convert the masterscale
+ `IMAGE_SEARCH` (boolean (optional)) - Search for associated image textures
+ `FILTER` (enum in [`WORLD`, `MESH`, `LIGHT`, `CAMERA`, `EMPTY`], (optional)) - Object types to import
+ `KEYFRAME` (boolean (optional)) - Import animation keyframes
+ `APPLY_MATRIX` (boolean (optional)) - Apply matrix transformations after import
+ `CONVERSE` (mathutils.Matrix(), (optional)) - Matrix to apply, if None, an identity matrix is taken
+ `CURSOR` (boolean (optional)) - Import 3D cursor position

---

The add-on has been moved from [Blender Add-ons repository](https://projects.blender.org/blender/blender-addons) and it will now be maintained here by [Sebastian Sille](https://projects.blender.org/NRGSille) and the Blender community.  
Bug reports will be tracked on the [Issue page](https://projects.blender.org/extensions/io_scene_3ds/issues) on this website. 
<br>

The add-on was part of the [Blender bundled add-ons](https://docs.blender.org/manual/en/4.1/addons). 
It is also available as an extension on the [Extensions platform](https://extensions.blender.org/add-ons/autodesk-3ds-format).  
<br>

The manual is no longer available on [Blender manual website](https://docs.blender.org/manual/en/dev/addons/import_export) instead it has been moved to this repository.  
-->[Autodesk 3DS IO manual](https://projects.blender.org/extensions/io_scene_3ds/wiki)<--  

Add on release notes are no longer available on [Blender developer docs](https://developer.blender.org/docs/release_notes) instead they can be found here:  
-->[Autodesk 3DS IO release notes](https://projects.blender.org/extensions/io_scene_3ds/src/branch/main/release_notes.md)<--


---
