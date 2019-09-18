+++
title = "First Mesh"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 10
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  
# Add menu entry to sidebar.
[menu.gmsh]
  parent = "basics"
  name = "First mesh"
  weight = 1

+++

## My first square

### Let's go

1. Copy/paste the following code in a new file, named *e.g.* `square.geo`:

        h = 1;                     // Characteristic length of a mesh element
        Point(1) = {0, 0, 0, h};   // Point construction
        Point(2) = {10, 0, 0, h};
        Point(3) = {10, 10, 0, h};
        Point(4) = {0, 10, 0, h};
        Line(1) = {1,2};            //Lines
        Line(2) = {2,3};
        Line(3) = {3,4};
        Line(4) = {4,1};
        Curve Loop(1) = {1,2,3,4};   // A Boundary
        Plane Surface(1) = {1};     // A Surface
        Physical Surface(1) = {1};  // Setting a label to the Surface

2. In GMSH, go to `files->open` (`CTRL+o`) and open the file, or type `gmsh square.geo` in a terminal (warning: this open a new instance of GMSH (which is very light by the way!)).
3. A square should have appear in GMSH's windows. The camera can be adjusted using the mouse: rotating (`left click`), translating (`right click`) or zooming (`wheel`). At bottom left of GMSH's windows, camera can be reseted using `X,Y,Z` and `1:1` (scale) buttons.
4. The square can now be meshed by typing `2` on the keyboard (or maybe `shift` + `2`) or using the menu: `Mesh->2D`

TODO: figure des boutons

TODO: figure du maillage du carré


### Log

GMSH is provided with a log console that can be displayed by clicking on the bottom banner of the window. For the square example, you should have something like

``` bash
Info    : Reading 'square.geo'...
Info    : Done reading 'square.geo'
Info    : Finalized high order topology of periodic connections
Info    : Meshing 1D...
Info    : Meshing curve 1 (Line)
Info    : Meshing curve 2 (Line)
Info    : Meshing curve 3 (Line)
Info    : Meshing curve 4 (Line)
Info    : Done meshing 1D (0.000981 s)
Info    : Meshing 2D...
Info    : Meshing surface 1 (Plane, Delaunay)
Info    : Done meshing 2D (0.017519 s)
Info    : 142 vertices 286 elements
```

This shows that GMSH starts by meshing the four lines (`Meshing 1D...`) then the surface. In this example, 142 vertices have been created for a total of 286 elements (lines and triangles).


{{% alert note %}}
Mesh can be saved using `CTRL + Shift + s` or using graphical interface (`file->save mesh`). By default, the extension of the mesh is `.msh` and of the same root as the `.geo` file (*i.e.* `square.geo` leads to `square.msh`).
{{% /alert %}}


{{% alert note %}}
[GUI is detailed in a next section]({{< relref "/tips_gui.md">}}).
{{% /alert %}}

## Code analyse

### Parameter `h`

On the first line of our code is defined a variable, `h`, which is the characteristic length of the elements.
This size is precised at each `Point` elementary entity.

{{% alert exercise %}}
Let us play with it (check the tips bellow first):

1. Modify the parameter `h`, remesh and observe the result
2. Anisotropic mesh: change only the mesh size of `Point(2)`, for example with `h/10`. Remesh and observe the result
{{% /alert %}}

{{% alert tips %}}
When the file has been modified, there is no need to relaunch GMSH: simply press `0` (zero) (or maybe `shift` + `0`). This forces GMSH to reset and re-read the current file.
{{% /alert %}}


### `Point` entity

In GMSH, a volume (3D) is defined by its surfaces, a surface (2D) by its (closed) curves and a curve (1D) by its endpoints. Each of these, `Volume`, `Surface`, `Curve` and `Point` are named **elementary entities** in GMSH. The first one that is studied here are the 0D entity: `Point`. They are defined as follows

```cpp
Point(index) = {x, y, z, h};
```
where the parameters are:

- `index`: unique identifier (`int` > 0) of the `Point`. This number has to be set by the user, either manually or using `newp` which returns the next available index for `Point` (`newp` = *NEW Point*).
- `x`, `y`, `z`: cartesian coordinate of the `Point`
- `h`: mesh element size around the `Point`


{{% alert exercise %}}
Apply the following change to `square.geo`:

1. Move some `Point` to obtain a rectangle (instead of a square)
2. Move some `Point` to get a quadrangle not rectangular at all
{{% /alert %}}


### `Line` and `Curve Loop` entities

#### `Line`

It is an oriented segment linking two `Point`:
```c++
  Line(index) = {PointA, PointB};
```
Avec comme paramètres :

- `index`: unique identifier (`int` > 0) of the `Line`. As for `Point`,this number has to be set by the user, either manually or using `newl` (next available index for `Line`).
- `PointA`: index of the starting `Point`
- `PointB`: index of the ending `Point`

{{% alert warning %}}
A `Line` is oriented!
{{% /alert %}}

#### `Curve Loop`

Closed loop of `Curve` (`Line` being a `Curve`) that represent a boundary (for a `Surface` generally):
```c++
Curve Loop(index) = {Curve1, Curve2, ..., CurveN};
```
Where:
- `index`: Unique identifier (`int` > 0) of the `Curve Loop` (`newll` can be used)
- `CurveX`: index of the X<sup>th</sup> `Curve` (or `Line`). If this quantity is negative (*e.g.* -10) then the `Curve` is oriented backwardly.

{{% alert exercise %}}
Let's do a "L-shape" geometry in a new file `L.geo` as proposed in the below figure (Set a **mesh refinement** to `h`=1). You can copy/paste the previous `square.geo` file to help yourself.
{{< figure src="../L.svg" title="L Shape geometry (with `h`=1 !)"  numbered="true">}}

{{% /alert %}}



### `Surface` entity

A `Surface` is defined by its boundaries: their `Curve Loop` (possibly more than one):
```c++
Plane Surface(index) = {CurveLoop1, CurveLoop2, ..., CurveLoopN};
```

- `index`: Unique identifier (`int` > 0) of the `Surface` (`news` can be used)
- `CurveLoopX`: index of the X<sup>th</sup> `Curve Loop`. As for the `Line`, this quantity can be negative (*e.g.* -10) to backwardly orient a `Curve Loop`.

{{% alert exercise %}}
Modify the "L-shape" geometry to add a hole inside as on the below figure. You will have to add `Point` and `Line` obvsiouly, but also a new `Curved Loop`. The `Surface` must then be defined by the 2 loops!

{{< figure src="../L_hole.svg" title="L-Shape avec un trou carré" numbered="true">}}

{{% /alert %}}


### `Elementary Entities` vs. `Physical Entities`

The last line of the file`square.geo` is:
```
Physical Surface(1)={1};
```
This is particular to GMSH and do not affect the geometry rather than the output mesh file. The command is the same for `Volume`, `Surface`, `Curve` and `Point`:

```
Physical Volume/Surface/Line/Point(index) = {entity1, entity2, ..., entityN};
```

Basically, GMSH can set a unique identifier (`index`) to a **set of entities** (to regroup them). This new identifier has nothing to do with the previous one you have defined. The output file do not contain the `elementary` index but only the `physical` ones. Moreover, only the entities that have a `physical` tag are saved to the disk! This is [explained in detail later](gmsh_meshformat.md) for that). 

## `Extrude`: from a square to a cube

[Extrusion](https://en.wikipedia.org/wiki/Extrusion) is a nice way to build 3D geometries from 2D. Start from the `square.geo` file and add this line before the `Physical Surface(1)...` line:
```cpp
Extrude {0, 0, 11} { Surface{1}; }
```
You should obtain a cube:

{{< figure src="../cube.png" title="Mesh of the cube" width="300"  numbered="true">}}


## De cercles en cercles

Ouvrez un fichier `cercle.geo` et insérez le code ci-dessous :
```c++
DefineConstant[
  R = {1, Min 0.5, Max 10, Step 0.1, Name "R"},
  h = {0.1, Min 0.01, Max 10, Step 0.01, Name "h"}
];
xc = 0;
yc = 0;
Point(1) = {xc, yc, 0, h};
Point(2) = {xc + R, yc, 0, h};
Point(3) = {xc, yc + R, 0, h};
Point(4) = {xc - R, yc, 0, h};
Point(5) = {xc, yc - R, 0, h};
Circle(1) = {2,1,3};
Circle(2) = {3,1,4};
Circle(3) = {4,1,5};
Circle(4) = {5,1,2};
Curve Loop(1) = {1,2,3,4};   //Définition d'un pourtour (d'un bord)
Plane Surface(1) = {1};     //Définition d'une surface
Physical Surface(1) = {1};  //A sauvegarder dans le fichier de maillage
```

Si tout va bien, GMSH affiche un cercle. La syntaxe de `Circle` est la suivante :
```c++
Circle(index) = {PointDepart, Centre, PointArrivee};
```
Pour dessiner un cercle, nous pourrions nous restreindre à 3 points uniquement plutôt que 4.

{{% alert warning %}}
GMSH ne peut pas construire d'arc de cercle d'angle **égal** ou **supérieur** à π.
{{% /alert %}}

{{% alert note %}}
Les `Circle` et les `Line` partagent le **même compteur**. Autrement dit, il ne peut exister une `Line` avec le même numéro qu'un `Circle`.
{{% /alert %}}


{{% alert exercise %}}
À vous de tester :

1. Introduisez un cercle intérieur de centre (0,0) et de rayon `Rint`, [modifiable dans le GUI]({{< relref "tips_interactive.md">}}), de valeur minimale 0.1 et maximale 0.4, un `Step` de 0.1.
2. Modifiez ensuite la `Surface` pour qu'elle représente la couronne ainsi formée plutôt que le disque original.
{{% /alert %}}

## Exercices supplémentaires

{{% alert exercise %}}
Dessinez le stade ci-dessous de manière à pouvoir le mailler. Attention, nous rappelons que GMSH ne peut pas tracer d'arc de cercle d'angle égal à π ou supérieur à π, il faut donc rajouter des points "intermédiaires" et découper l'arc de cercle en (au moins) deux.

{{<figure library="1" src="teaching/gmsh/stadium.svg"  title="Stade MAIN5">}}
{{% /alert %}}

