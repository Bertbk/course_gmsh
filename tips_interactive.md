+++
title = "Interactive GUI"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 60
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "tips&tricks"
  name = "Interactive GUI"
  weight = 5


+++

## Reference geometry

Let us consider the rectangle of width `lx` and `ly` in the respectively x- and y-direction:

```c++
SetFactory("OpenCASCADE");
h = 0.1;
Mesh.CharacteristicLengthMin = h;
Mesh.CharacteristicLengthMax = h;
lx = 2;
ly = 1;
Rectangle(1)= {0, 0, 0, lx, ly};
Physical Surface(1) = {1};
```

## `DefineNumber` Adding parameters in the menu

Parameters `lx` and `ly` will now be mutable in the GUI thanks to the command `DefineNumber`:
```c++
DefineNumber[Var  = { DefautValue, ... options ...} ;
```
To understand it better, change the lines defining `lx` and `ly` by:
```c++
lx = DefineNumber[2, Min 0.5, Max 10, Step 0.1, Name "Geometry/Lx"];
ly = DefineNumber[1, Min 0.5, Max 10, Step 0.1, Name "Geometry/Ly"];
```
Reload the file using `GMSH` and your screen should looks like the figure below. On your left, you are now able to modify the value of `lx` and `ly` interactively (without reloading).


{{< figure src="../gui_interactive.png" title="GUI is now interactive with your script">}}


{{% alert note %}}
[Numerous customisation parameters are available!](https://gitlab.onelab.info/doc/tutorials/wikis/ONELAB-syntax-for-Gmsh-and-GetDP)
{{% /alert %}}

{{% alert exercise %}}
Make `h` editable in the GUI with a `Step` of 0.1 and such that 0.1 < `h` < 1.
{{% /alert %}}


## `DefineConstant`

Every `DefineNumber` can be merged into a `DefineConstant` environment:
```c++
DefineConstant[
 // Attention, pas de virgule à la fin pour la derniere variable
  lx  = { 2, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Lx"},
  ly  = { 1, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Ly"}
];
```

## `DefineString`

In the same way as `DefineNumber`, this command returns a string.
