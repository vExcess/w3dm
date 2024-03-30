# W3DM
w3dm stands for "web 3d model". w3dm is a free and open source, simple, and compact 3D model file format.  

## Why
There are so many other 3D file formats already, why create a new one? Because in my opinion the other ones are too bloated, too simple, or too complicated.
| File Format  | About |
| ------------- | ------------- |
| OBJ | the text version is simple and has a good amount of features, but is very bloated format and not compact; the binary version is proprietary  |
| STL | can't encode color information |
| FBX | is proprietary |
| DAE | is too complicated and has unnecessary features like built-in physics |
| 3DS | is proprietary |
| IGES | uses circuit diagrams which means it's too complicated |
| STEP | it's just a very complicated format in general |
| VRML | I don't need animation support |
| AMF | is XML based and XML is the opposite of compact |
| 3MF | it's basically just a .zip file and having to write an whole zip codec to read a file is too complicated |

Therefore I am making the W3DM file format which has the following features:
- Simple :: By avoiding sophisticated compression techniques and fancy features the format is very easy to understand
- Compact :: Because it's a binary format it will be much more compact than a text based OBJ file, however because it doesn't have built in compression you can gzip the file for better compression
- Supports non-triangular faces (up to 255 vertices per face)
- Supports storing texture mappings (the texture itself however is stored externally)
- Supports vertex normals and normal mapping (the normal map itself is also stored externally)

## Editor
Work in Progress; A file format is pretty useless unless there is an editor capable of working with them so I'll make a 3D Paint / Blender type program for working with w3dm files.

## Documentation
##### JSON Format
- `vertices` is a flattened array of 3D vectors where each vector is a point in 3D space  
- `normals` is a flattened array of 3D vectors where each vector is a normal vector  
- `normalIndices` is an array where each number at index B is the index of a vector in `normals` where said vector is the normal of vertex(faces[B])  
- `normalMapIndices` is an array where each number at index C is the normal map imageData index of vertex(faces[C])  
- `textureIndices` is an array where each number at index A is the texture imageData index of vertex(faces[A])  
- `faceLengths` is an array where each number at index D is the number of vertices in face(D)  
- `faces` is an array where each sequence of length faceLengths[D] contains the indices of the vectors in `vertices` that compose the face  

##### Length Requirements
- `normalIndices.length` must equal faces.length (for per vertex normals) or equal facesLengths.length (for per face normals)
- `normalMapIndices.length` must equal 0 or faces.length
- `textureIndices.length` must equal 0 or faces.length
- `sum(faceLengths)` must equal faces.length OR `faceLengths.length` can equal 0 if all faces are triangles

##### Range Requirements
- `face[i]` must be in the range [0, vertices.length)
- `normalIndices[i]` must be in the range [0, normals.length)

##### Binary Format
| # of Bytes  | Description |
| ------------- | ------------- |
| 4 | the magic number 1932805328 (W3DM in binary) |
| 3 | vertices.length |
| 3 | normals.length |
| 3 | normalIndices.length |
| 3 | facesLengths.length |
| 3 | faces.length |
| vertices.length * 2 | vertices |
| normals.length | normals |
| normalIndices.length * 2 | normalIndices |
| normalMapIndices.length * 3 | normalMapIndices |
| textureIndices.length | textureIndices |
| faceLengths.length | faceLengths |
| faceLengths.length * 2 | faces |

##### Encode Model
`W3DM.encode(model) -> Uint8Array` takes a model where `model` is a JS object in the format:
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
    normals: [
        nx0, ny0, nz0,
        nx1, ny1, nz1,
        nx2, ny2, nz2,
        ...
    ],
    // normal indices are stored as unsigned 16 bit integers
    normalIndices: [
        idx0, idx1, idx2,
        idx3, idx4, idx5,
        ...
    ],
    // normalMapIndices indices are stored as unsigned 24-bit integers
    normalMapIndices: [
        idx0, idx1, idx2,
        idx3, idx4, idx5,
        ...
    ],
    // texture values are stored as unsigned 24-bit integers
    textureIndices: [
        idx0, idx1, idx2,
        idx3, idx4, idx5,
        ...
    ],
    // face lengths are stored as unsigned 8-bit integers
    faceLengths: [
        l0, l1,
        ...
    ],
    // face values are stored as unsigned 16 bit integers
    faces: [
        v0, v1, v2, v3,
        v4, v5, v6, v7,
        ...
    ]
}
```

