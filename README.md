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

## Documentation
##### JSON Format
- `vertices` is a flattened array of 3D vectors where each vector is a point in 3D space  
- `normals` is a flattened array of 3D vectors where each vector is a normal vector  
- `texIndices` is an array where each number at index A is the texture imageData index of vertex(faces[A])  
- `normalIndices` is an array where each number at index B is the index of a vector in `normals` where said vector is the normal of vertex(faces[B])  
- `normalMapIndices` is an array where each number at index C is the normal map imageData index of vertex(faces[C])  
- `faceLengths` is an array where each number at index D is the number of vertices in face(D)  
- `faces` is an array where each sequence of length faceLengths[D] contains the indices of the vectors in `vertices` that compose the face  

`W3DM.encode(model)` takes a model where `model` is a JS object in the format:
```js
{
	// vertex values are normalized and then scaled up and stored as signed 16-bit integers
    vertices: [
        x0, y0, z0,
        x1, y1, z1,
        x2, y2, z2,
        ...
    ],
    // normal values are mapped from range [-1.0, 1.0] and stored as unsigned 8-bit integers
    // if normalMapIndices is not undefined, then normals must be undefined
    normals: [
        x0, y0, z0,
        x1, y1, z1,
        x2, y2, z2,
        ...
    ] OR undefined,
    // texture values are stored as unsigned 24-bit integers
    texIndices: [
        idx0, idx1, idx2, ...
    ],
    // normal indices are stored as unsigned 16 bit integers
    // if normalMapIndices is not undefined, then normalIndices must be undefined
    normalIndices: [
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
    ] OR undefined,
    // normalMapIndices indices are stored as unsigned 24-bit integers
    // if normalMapIndices is undefined, normals and normalIndices must both not be undefined
    normalMapIndices: [
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
        0, 0, 0, 0,
    ] OR undefined,
    // face lengths are stored as unsigned 8-bit integers
    faceLengths: [
        4, 4, 4, 4, 4, 4
    ],
    // face values are stored as unsigned 16 bit integers
    faces: [
        0, 1, 2, 3,
        4, 5, 6, 7,
        0, 3, 7, 4,
        1, 2, 6, 5,
        0, 1, 5, 4,
        2, 3, 7, 6
    ]
}
```
