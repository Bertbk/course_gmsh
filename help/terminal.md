+++
title = "Terminal commands"
linktitle="Terminal commands"
summary=""
type="book"
icon=""
icon_pack=""
date = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false

math = true
weight = 8
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"
+++

## Help

```bash
gmsh --info
```

## Mesh and save without the GUI

```bash
gmsh carre.geo -2
```
The output file can be set by adding the option `-o FileName`. To mesh in 3D, replace `-2` by `-3`.

## Multi-Threading

If GMSH is compiled with OpenMP then mesh can be generated in parallel using `NUM_THREADS` threads:
```
gmsh file.geo -nt NUM_THREADS [...]
```

##  Modifying a value of a `Constant`: `-setnumber`

For example, to change the value of `h` to 10 :
```bash
gmsh L.geo -2 -setnumber h 10
```
To modify multiple values, the `-setnumber` option must be typed each time
```bash
gmsh L.geo -2 -setnumber h 10 -setnumber xmax 50
```

## Log file

```bash
gmsh L.geo -2 $1>logfile
```
