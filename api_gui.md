+++
title = "API & GUI"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = true  # Is this a draft? true/false
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
  identifier= "api_gui"
  name = "GUI"
  weight = 320

+++

## Start GMSH GUI from Python

(probably best before `gmsh.finalize()`)

```
gmsh.fltk.run()
```

## Onelab server: interactive GUI

You saw previously how to make a parameter mutable through the GUI. For example, a parameter 
```cpp
R = DefineNumber[1, Min 0.1, Max 10, Step 0.1, Name "Geometry/Radius"];
```

Using the Python API, this commands becomes
```python
# Parameters
# set a single parameter
gmsh.onelab.set("""
{ "type":"number", "name":"Geometry/Radius", "values":[1], "step":0.1, "min":0.1, "max":10  }
""")
```

This variable can be then access through... [to be continued, sorry!]