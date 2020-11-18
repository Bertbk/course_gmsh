+++
title = "Access the mesh elements"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 320
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"
  
# Add menu entry to sidebar.
[menu.gmsh]
  parent = "V. API"
  identifier="api_first_mesh"
  name = "Access the mesh elements"
  weight = 320

+++
{{% callout note %}}
Every function available in the API is well documented in [the file `gmsh.py`](https://gitlab.onelab.info/gmsh/gmsh/blob/master/api/gmsh.py). Here some of these functions are detailed with some examples. Actually, definition shown here are (truncated) copy/paste from the official documentation.
{{% /callout %}}

{{% callout note %}}
I also remind you that in VSCode (+ Python extension), the definition of the function [are displayed in the text editor]({{<relref "intro-help.md#extensions">}}): extremly convenient!
{{% /callout %}}


Now that you know how to generate a mesh with GMSH API, you probably wants to *play* with the resulting mesh. For example, if you want to program a finite element solver, you probably needs to access each elements and nodes of the mesh and compute some quantity on it. In this section, you will learn the basic functionality to do that withing Python. For Julia and C++, the syntax changes but the idea is the same.

## Reference problem

Let us consider a unit square with 0.1 as mesh size. The `Surface` has a `Physical Tag` equal to 10, the bottom boundary `Line` has `Physical Tag`=1 and the three other are regrouped in the same `Physical Tag` 2

```python
# File square.py
import gmsh
import sys
gmsh.initialize(sys.argv)
gmsh.option.setNumber("General.Terminal", 1)
gmsh.option.setNumber("Mesh.CharacteristicLengthMin", 0.1);
gmsh.option.setNumber("Mesh.CharacteristicLengthMax", 0.1);
# Model
model = gmsh.model
model.add("Square")
# Rectangle of (elementary) tag 1
factory = model.occ
factory.addRectangle(0,0,0, 1, 1, 1)
# Sync
factory.synchronize()
# Physical groups
gmsh.model.addPhysicalGroup(1, [1], 1)
gmsh.model.addPhysicalGroup(1, [2,3,4], 2)
gmsh.model.addPhysicalGroup(2, [1], 10)
# Mesh (2D)
model.mesh.generate(2)
# ==============
# Code to test mesh element access will be added here !
# ...
# ==============
#Save mesh
gmsh.write("square.msh")
# Finalize GMSH
gmsh.finalize()
```
Running it in a terminal an opening it with GMSH should open a windows as below
```bash
python square.py
gmsh square.msh
```

{{<figure src="../img/msh-square.png" title="Meshed square" numbered="true">}}


## Nodes


{{% callout note %}}
Methods to get and modify mesh element are placed in the `gmsh.model.mesh` namespace. For example, to call `getNode(12)`, you should type `gmsh.model.mesh.getNode(12)`.
{{% /callout %}}
### Basics

| Name                      | Definition / Remark |
| ------------------------- | ------------------- |
| `getNode(tag)` | Get the coordinates and the parametric coordinates (if any) of the node with tag `tag`.<br>Return `coord`, `parametricCoord`. Generally, the first quantity of the returned array is what you want.|
| `getNodes(dim=-1, tag=-1, includeBoundary=False, returnParametricCoord=True)` | Get the nodes classified on the entity of dimension `dim` and tag `tag`.<br>Return `nodeTags`, `coord`, `parametricCoord`.|
| `getNodesForPhysicalGroup(dim, tag)`|Get the nodes from all the elements belonging to the physical group of      dimension `dim` and tag `tag`.<br>Return `nodeTags`, `coord`|
| `getNodesByElementType(elementType, tag=-1, returnParametricCoord=True)`|Get the nodes classified on the entity of tag `tag`, for all the elements of type `elementType`. The other arguments are treated as in `getNodes`.<br>Return `nodeTags`, `coord`, `parametricCoord`.|

### Example

Start from [the code meshing a square]({{<relref "#reference-problem.md">}}) that has been given above and override these line
```python
# ==============
# Some portions of code will be added here !
# ...
# ==============
```
by the following line, where we ask GMSH to first provide the tag of the nodes on `Physical Line(1)` and second, their coordinate:
```python
print("Nodes tag on Physical Line 1")
print(model.mesh.getNodesForPhysicalGroup(1,1)[0])
print("Nodes coordinates on Physical Line 1")
print(model.mesh.getNodesForPhysicalGroup(1,1)[1])
```
You should get the following messages (not necessarily the same number/coordinate!) in your terminal
```bash
Nodes tag on Physical Line 1
[ 1  6  2  7 11  9 10  8  5 12 13]
Nodes coordinates on Physical Line 1
[0.  0.  0.  0.2 0.  0.  1.  0.  0.  0.3 0.  0.  0.7 0.  0.  0.5 0.  0. 
0.6 0.  0.  0.4 0.  0.  0.1 0.  0.  0.8 0.  0.  0.9 0.  0. ]
```

## Elements

### Basics

| Name                      | Definition / Remark |
| ------------------------- | ------------------- |
| `getElement(elementTag)`|Get the type and node tags of the element with (**elementary**) tag `tag`. This is a sometimes useful but inefficient way of accessing elements, as it relies on a cache stored in the model.<br>Return `elementType`, `nodeTags`.|
| `getElements(dim=-1, tag=-1)` |Get the elements classified on the (**elementary**) entity of dimension `dim` and (**elementary**) tag `tag`.<br>Return `elementTypes`, `elementTags`, `nodeTags`|
| `getElementTypes(dim=-1, tag=-1)` | Get the types of elements in the (**elementary**) entity of dimension `dim` and (**elementary**) tag `tag`.<br>Return `elementTypes`.|
|`getElementsByType(elementType, tag=-1, task=0, numTasks=1)`|Get the elements of type `elementType` classified on the entity of tag `tag`.<br>Return `elementTags`, `nodeTags`.|
|`getElementProperties(elementType)`|Get the properties of an element of type `elementType`: its name(`elementName`), dimension (`dim`), order (`order`), number of nodes(`numNodes`), coordinates of the nodes in the reference element(`nodeCoord` vector, of length `dim` times `numNodes`) and number ofprimary (first order) nodes (`numPrimaryNodes`).<br>Return `elementName`, `dim`, `order`, `numNodes`, `nodeCoord`, `numPrimaryNodes`.|

### Example

#### Element type

As for the node, you can copy/paste these lines before `gmsh.write("square.msh")`:
```python
print("Element type of Line(1) (segment has type 1)")
print(model.mesh.getElementTypes(1,1))
print("Element type of Surface(1) (triangle has type 2)")
print(model.mesh.getElementTypes(2,1))
prop = model.mesh.getElementProperties(1)
print("Properties of a segment element (type = 1)")
print("  Element Name      : "+str(prop[0]))
print("  Dim               : "+str(prop[1]))
print("  Order             : "+str(prop[2]))
print("  Num of Nodes      : "+str(prop[3]))
print("  Node Coord        : "+str(prop[4]))
print("  Num Prim. Nodes   : "+str(prop[5]))
```
You should get:
```bash
Element type of Line(1) (segment has type 1)
[1]
Element type of Surface(1) (triangle has type 2)
[2]
Properties of a segment element (type = 1)
  Element Name      : Line 2
  Dim               : 1
  Order             : 1
  Num of Nodes      : 2
  Node Coord        : [-1.  1.]
  Num Prim. Nodes   : 2
```

#### Loop on elements

Now we want to loop on the elements composing the `Line` of elementary tag 1, and for each element, display the tag of each of its vertices (starting and ending point):
```python
segments = model.mesh.getElements(1,1)[1][0]
print("Segment tags composing Line(1)")
print(segments)
nodes = model.mesh.getElements(1,1)[2][0]
nodes_per_elem = model.mesh.getElementProperties(1)[3]
print("Number of nodes per segment: " +str(nodes_per_elem))
for j in range(len(segments)):
  print("Element Tag("+str(segments[j])+")=["+str(nodes[2*j])+", "+str(nodes[2*j+1])+"]")
```
And this gives the following result (again, indices can differ on your side)
```bash
Segment tags composing Line(1)
[ 1  2  3  4  5  6  7  8  9 10]
Number of nodes per segment: 2
Element Tag(1)=[1, 5]
Element Tag(2)=[5, 6]
Element Tag(3)=[6, 7]
Element Tag(4)=[7, 8]
Element Tag(5)=[8, 9]
Element Tag(6)=[9, 10]
Element Tag(7)=[10, 11]
Element Tag(8)=[11, 12]
Element Tag(9)=[12, 13]
Element Tag(10)=[13, 2]
```

### Integration points, jacobian, FEM basis functions, ...

Numerous useful functions are provided by GMSH API, even Lagrange (and their Gradient) basis function.

## Physical entities to Elementary entities

### Basics

| Name                      | Definition / Remark |
| ------------------------- | ------------------- |
|`getPhysicalGroups(dim=-1)`| Get all the physical groups in the current model. If dim' is >= 0, return only the entities of the specified dimension.<br>Return `dimTags`.|
|`getEntitiesForPhysicalGroup(dim, tag)`|Get the tags of the model entities making up the physical group of dimension `dim` and tag `tag`.<br>Return (**elementary**) `tags`|

### Example

This portion of codes loops on each `Physical` entities and for each of them, display every `Elementary` entities and the type of elements that is used to describe them:
```python
# taken from /demos/api/poisson.py
vGroups = model.getPhysicalGroups()
for iGroup in vGroups:
    dimGroup = iGroup[0] # 1D, 2D or 3D
    tagGroup = iGroup[1]
    namGroup = model.getPhysicalName(dimGroup, tagGroup)
    vEntities = model.getEntitiesForPhysicalGroup(dimGroup, tagGroup)
    for tagEntity in vEntities:
        dimEntity = dimGroup
        vElementTypes = model.mesh.getElementTypes(dimEntity,tagEntity)
        print("Physical Tag="+str(tagGroup)+", Entity Tag="+str(tagEntity)+", Element Type="+str(vElementTypes))
```
On my computer, this gives the following result
```bash
Physical Tag=1, Entity Tag=1, Element Type=[1]
Physical Tag=2, Entity Tag=2, Element Type=[1]
Physical Tag=2, Entity Tag=3, Element Type=[1]
Physical Tag=2, Entity Tag=4, Element Type=[1]
Physical Tag=10, Entity Tag=1, Element Type=[2]
```
