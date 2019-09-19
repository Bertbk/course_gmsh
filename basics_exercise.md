+++
title = "Exercices"

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
  parent = "basics"
  name = "Exercises"
  weight = 30

+++

## Stadium

Draw the following figure representing a stadium. Please remember that GMSH (and neither you!) can draw an arc of circle with an angle greater or equal to π: intermediate `Point` might be added and the half-circle should be split in (at least) two arcs.

{{<figure src="../stadium.svg"  title="Stadium">}}
