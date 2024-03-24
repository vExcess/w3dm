# W3DM
w3dm stands for "web 3d model". w3dm is a free and open source, simple, and compact 3D model file format.  

## Why
There are so many other 3D file formats already, why create a new one? Because in my opinion the other ones are too bloated or complicated.
| File Format  | My Opinion |
| ------------- | ------------- |
| OBJ | is simple and has a good amount of features, but it's a very bloated format and the data is not compact  |
| STL | can't encode color information |
| FBX | is proprietary |
| DAE | is too complicated and has unnecessary features like built-in physics |
| 3DS | is proprietary |
| IGES | the description said it uses circuit diagrams and that sounded scary |
| STEP | it's just a very complicated format in general |
| VRML | I don't need animation support |
| AMF | is XML based and XML is the opposite of compact |
| 3MF | it's basically just a .zip file and having to write an whole zip codec to read a file is too complicated |

Therefore I am making the W3DM file format which has the following features:
- Simple :: An single programmer can write a codec for it in less than a day
- Compact :: On it's own it'll be fairly compact, but if you need the extra savings you can always just gzip it
- Supports non-triangular faces
- Supports textures/colors
- Supports vertex normals and normal mapping

## Editor
Work in Progress; A file format is pretty useless unless there is an editor capable of working with them so I'll make a 3D Paint / Blender type program for working with w3dm files.

## Codec + Documentation
Work in Progress; I'll write a codec in both JavaScript and Zig and provide documentation for both when they are complete.

## Specification
Work in Progress
