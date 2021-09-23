+++
title = "Graphical User Interface"
linktitle="Graphical User Interface"
summary=""
type="book"
icon=""
icon_pack=""
date = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false

math = true
weight = 6
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"
+++

{{% callout note %}}
Reminder: GUI = Graphical User Interface.
{{% /callout %}}

## General description

Accessing the option is done through the menu `Tools->Options` (or `shift + ctrl + N`). This menu is decomposed into numerous categories:

- `General`
- `Geometry`
- `Mesh`
- `Solver`
- `Post-Pro`
- Plus eventually the post processing views: `View [0]`, `View [1]`, ...

In GMSH, a `View` corresponds to a function visualized on a mesh.

## Camera

| Operation| Result|
| --- | --- |
| left click| Rotation|
| right click| Translation|
| wheel| Zoom|

Please use the bottom buttons `X`, `Y` and `Z` to reset the camera along an axis. The `1:1` button reset the scale (zoom).

{{< figure src="../../img//gui_camera.png" title="Camera" numbered="true">}}

## Options `General`

They are divided into different tabs.

### `General` (*yes, again*)

The options that can be usefull for this tutorial are:

- `Use dark interface`: better for our eyes (at least, mine)
- `Show bounding box`: draw corners (and coordinates) of the bounding box of the geometry

{{% callout tips %}}
To save your favorite options for next launch: `File->Save options as default`.
{{% /callout %}}

### `Axes`

You can here delete the small axis displayed on bottom right. This is particularly usefull when doing screen shot or saving nice figures.

### `Color`

It's here possible to move the light source (on top right). This lighting effect can also be deleted in the tab of each `View`.


## Options `Geometry`

The most usefull option in the tab `Visibility` is to display normal and tangeant vectors of surfaces, and the checkbox to display (or not) surfaces and volumes.


## Options `Mesh`

Again, the `Visibility` tab propose to display:

- 1D element (segment) which **are not displayed by default**. This can be sometimes  confusing when reading a 1D mesh.
- The face of the element to make then opaque
- Normal and Tangent vectors (bottom)

{{< figure src="../../img//gui_normals.jpg" title="Displaying normal vectors">}}


## `View` options (Post-Processing)

{{% callout warning %}}
Even if GMSH can do vizualisation and post processing, [Paraview](https://paraview.org) is designed for that and is way more powerful.
{{% /callout %}}

{{% callout warning %}}
For this menu to be visible, a finite element function must be opened, coming from example from [GetDP](http://getdp.info) or [FreeFem++](https://freefem.org).
{{% /callout %}}

The `View[0]` categorie contains different tabs:

- `General`
- `Axes`
- `Visibility`
- `Transfo`
- `Aspect`
- `Color`
- `Map`

It's possible to save the option associated to particular `View` using `File->Save Model Option`. A `.opt` file is then created and the next time GMSH open the post-processing file, the chosen options are again loaded (camera position, ...). 

### `General`

(Most) Interesting are:

- `Interval type`: **Never chose "numeric values"**, otherwise GMSH will probably crash due to too many number to display. `isovalues` or `filled isovalues` are really interesting for visualisation.
- `Range mode`:
  - `Per Time Step`
  - `Custom`
  - `Step` : In GMSH, `Step` are time step, but also real part (`Step 0`) and imaginary part (`Step 1`)

### Axes

You can try it.

### Visibility

-  `Draw element outlines` is used to superimposed (or not) the mesh onto the `View`
- `Time Display`: for complex time-independent function, select `Harmonic Data` to display "real part" and "imaginary part" in the title of `View` (instead of `Step`)


### Transfo

This is where the `View` (not the function!) can be transformed:

- Translation, Rotation, ... according to the matrix (rotation) and vector (translation)
- `Normal Raise`: plot a 2D solution in 3D (example below)

{{< figure src="../../img//gui_normalraise.png" title="`Normal Raise` example">}}


### Color

You can here desactivate the lighting effect with `Enable lighting`.


### Color Map

Some pre-defined colormaps are available by typing 0,1,2,3...,9.


## Some plugins

These plugins are accessible in `Tools->Plugins` menu. They can modify the data and make some simple operations on them. 

{{% callout warning %}}
Plugins obviously do not modify the content of the file, only the `View`
{{% /callout %}}

### ModulusPhase

Compute the modulus and the phase of a `View`. The `View` is here override.

It can be launched with the default parameter as long as the `View` to work on is selected (-1= current `View`).

### ModifyComponents

Compute a mathematical operation on the component (if vector data or tensor data) of the `View`.

## Delete a `View` from memory, reload it, ...

- A `View` can be set to visible or not using the checkbox close to its name
- Deleting a `View` from the memory: click on the arrow on the right of the name of the view. Then in the dropdown menu select `Remove`. Every `View` can be removed at once, too.
- Reloading a `View` can be done by typing `r` or in its dropdown menu by selecting `Reload`.



## Image exportation (png, jpg, ...)

In the menu, select `Files->Export` and then chose the desired output format, or type the full name of the output file and GMSH will guess its format from the extension you provide (png, jpg, ...).
