+++
title = "Un moteur CAO plus performant"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 30
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "occ"
  name = "A powerful CAD engine"
  weight = 10

+++

## Why?

GMSH's engine is great but cannot manage boolean operations, which is a classical tool in CAD (Computer Aided Design). The open-source and powerful [OpenCascade](https://www.opencascade.com/) engine can be used in GMSH which provides numerous advantages, for example:

- "Ready-to-use" **geometries**:
  - 2D : rectangle, disk, ...
  - 3D : box, sphere, tor, cone, cylinder, ...
- **Splines**
- **Boolean operations**: Union, Difference, Intersection, Fragmentation

The major drawback is that the index of created entities are managed by OpenCascade and hence **are no longer known a priori**, especially:

- For an anisotropic mesh (different mesh size per region): `Point` with different mesh size must be sough *a posteriori*
- During the design part, indices of the elementaty entities can be found via the GUI, leading to switching between code and GUI

It's however **highly** recommended to use this CAD engine instead of the native one. The few drawbacks listed above can moreover be easily circumvent.


## A short example

{{% alert note %}}
Numerous example using boolean operations can be found in the [`demos/boolean` folder in the source code of GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos/boolean).
{{% /alert %}}

An example of CAD object is provided on [the dediacted wikipedia page](http://en.wikipedia.org/wiki/Constructive_solid_geometry):


{{< figure src="../Csg_tree.png" title="Constructive solid Geometry (crédit: Wikipédia)" width="500" >}}

In GMSH and using OpenCascade, the file [`demos/boolean.geo`](https://gitlab.onelab.info/gmsh/gmsh/raw/master/demos/boolean/boolean.geo) reproduce it in a few lines only:

```cpp
SetFactory("OpenCASCADE");
// from http://en.wikipedia.org/wiki/Constructive_solid_geometry
Mesh.Algorithm = 6;
Mesh.CharacteristicLengthMin = 0.4;
Mesh.CharacteristicLengthMax = 0.4;

R = DefineNumber[ 1.4 , Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Box dimension" ];
Rs = DefineNumber[ R*.7 , Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Cylinder radius" ];
Rt = DefineNumber[ R*1.25, Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Sphere radius" ];

Box(1) = {-R,-R,-R, 2*R,2*R,2*R};
Sphere(2) = {0,0,0,Rt};

BooleanIntersection(3) = { Volume{1}; Delete; }{ Volume{2}; Delete; };

Cylinder(4) = {-2*R,0,0, 4*R,0,0, Rs};
Cylinder(5) = {0,-2*R,0, 0,4*R,0, Rs};
Cylinder(6) = {0,0,-2*R, 0,0,4*R, Rs};

BooleanUnion(7) = { Volume{4}; Delete; }{ Volume{5,6}; Delete; };
BooleanDifference(8) = { Volume{3}; Delete; }{ Volume{7}; Delete; };
```

{{% alert exercise %}}
Try it! Mesh in 2D and you should see the wanted geometry.
{{% /alert %}}