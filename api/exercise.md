+++
title = "Exercise"
linktitle="Exercise"
summary=""
type="book"
icon=""
icon_pack=""
date = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false

math = true
weight = 36
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"
+++

## Discrete Function

Using GMSH API, build a code that:

- Discretizes the segment [0,2π] with a mesh size of your choice
- Computes, on each node of the mesh, the cosine of the 2*x-coordinate ($=\cos(2x)$)
- Plot this function using, for example, [Matplotlib](https://matplotlib.org/)

This program can obviously be done without GMSH (and way faster without!). It's just a toy program!

{{< figure src="../../img/api-exercice-1d.png" title="Result" numbered="true">}}
