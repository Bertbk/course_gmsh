+++
title = "Spline and Extrusion by Rotation"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 130
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
  name = "Spline and Extrusion by Rotation"
  weight = 130


+++

## Extrusion by rotation

```cpp
Extrude { { xr, yr, zr }, {xp, yp, zp}, alpha } { List }
```
with (`xr`, `yr`, `zr`) a vector of the rotation axis, (`xp`, `yp`, `zp`) a point located on the axis and `alpha` the rotation angle (in rad, recal that π=`Pi`). The `List` argument contains entities to be extruded. Take a simple example:
```cpp
SetFactory('OpenCASCADE');
h = 0.05;
Point(1) = {0,1,0,h};
Point(2) = {0.5,0.5,0,h};
Point(3) = {0,0,0,h};
Line(1) = {1,2};
Line(2) = {2,3};
```
This code creates a broken line looking like this symbol "<":

{{< figure src="../occ-diamond-pre.png" title="Broken line (before extrusion)" numbered="true" >}}

Now this brokent line is extruded by a rotation of 2π around the z-axis:
```
Extrude { {0,1,0}, {0,0,0}, 2*Pi}{Line{1,2};}
```
This results in 2 cones, one on each other, like a diamond:

{{< figure src="../occ-diamond.png" title="Extruded broken line and meshed (2D)" numbered="true" >}}

{{% alert exercise %}}
By extruding a single (broken) line, draw a nice Christmas tree as on the figure below. Do not create any cone or box! The key idea is to find the line that can generate the whole geometry.

{{< figure src="../sapin.png" title="Christmas tree" >}}

{{% /alert %}}


## Spline

A spline is a function piecewise polynomial. In GMSH, `BSpline` are `Spline` that do not necessarily cross their control points (please note that `BSpline` **are not yet** available with `OpenCASCADE` engine). Syntax for `Spline` and `BSpline` is the following

|Syntax|Definition|
|---|---|
|`Spline(indice) = {p1, p2, ..., pN}` | Create a spline that goes **through** control `Point` `p1`,`p2`,...,`pN`|
|`BSpline(indice) = {p1, p2, ..., pN}` | Create a B-spline **thanks to** control `Point` `p1`,`p2`,...,`pN`|

{{% alert warning %}}
Reminder: `BSpline` **are not yet** available with `OpenCASCADE`!
{{% /alert %}}

As usual, there is nothing more useful than an example:
```cpp
SetFactory("OpenCASCADE");
h = 1;
Mesh.CharacteristicLengthMin = h;
Mesh.CharacteristicLengthMax = h;
Point(1) = {0,4,0,h};
Point(2) = {5,2,0,h};
Point(3) = {10,8,0,h};
Point(4) = {15,4,0,h};
Point(5) = {20,10,0,h};
Spline(1) = {1,2,3,4,5};
```
This code create a spline:

{{< figure src="../occ-jar-pre.png" title="A spline" numbered="true" >}}

Now, extrude this spline using a 2π rotation around the x-axis
```cppp
Extrude { {1,0,0}, {0,0,0}, 2*Pi} { Line{1} ;}
```
And we get a cute open jar. To get a closed one, two lines should be added (on top and on bottom). The whole code would then be:
```cpp
SetFactory("OpenCASCADE");
h = 1;
Mesh.CharacteristicLengthMin = h;
Mesh.CharacteristicLengthMax = h;
// Spline for the jar
Point(1) = {0,4,0,h};
Point(2) = {5,2,0,h};
Point(3) = {10,8,0,h};
Point(4) = {15,4,0,h};
Point(5) = {20,10,0,h};
Spline(1) = {1,2,3,4,5};
// Lines to close the jar
Point(6) = {20,0,0,h};
Point(7) = {0,0 ,0,h};
Line(2) = {5,6};
Line(3) = {7,1};
Extrude { {1,0,0}, {0,0,0}, 2*Pi} { Line{1,2,3} ;}
```

{{< figure src="../occ-jar.png" title="Extruded spline resuling in a jar" numbered="true" >}}

{{% alert exercise %}}
Using the `Visibility` window (`Tools->Visibility`) to get back indices of the surfaces, create the `Volume` associated to the jar. To do, you must add a `Surface Loop` and then the `Volume`.
{{% /alert %}}


## Exercise 

{{% alert exercise %}}
Time to play: draw a flask using `Spline` and `Extrude`, with or without support.

{{< figure src="../fiole.png" title="Flask with or without support" >}}

{{% /alert %}}

