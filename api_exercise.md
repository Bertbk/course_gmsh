+++
title = "Exercise"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=true
weight = 330
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  
# Add menu entry to sidebar.
[menu.gmsh]
  parent = "API"
  identifier="api_exercise"
  name = "Exercise"
  weight = 330

+++

## Discrete Function

Using GMSH API, build a code that:

- Discretizes the segment [0,2π] with a mesh size of your choice
- Computes, on each node of the mesh, the cosine of the 2*x-coordinate ($=\cos(2x)$)
- Plot this function using, for example, [Matplotlib](https://matplotlib.org/)

This program can obviously be done without GMSH (and way faster without!). It's just a toy program!

{{< figure src="../api-exercice-1d.png" title="Result" numbered="true">}}
