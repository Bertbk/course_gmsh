+++
title = "Getting Started"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true
weight = 3
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"


# Add menu entry to sidebar.
[menu.gmsh]
  parent = "I. Introduction"
  name = "Getting Started"
  identifier="intro_start"
  weight = 3

+++

## What is it?

GMSH is an open-source software for 2D/3D mesh and data visualization, developped in Belgium by (mainly) [C. Geuzaine](https://geuz.org) and [J-F. Remacle](https://perso.uclouvain.be/jean-francois.remacle/).

## Install (without being root!)

{{% alert warning %}}
Linux user: please **do not install GMSH via package manager** (`apt-get`, ...): version is outdated.
{{% /alert %}}


### From scratch (advanced users)

Source code are available on [Onelab's Gitlab](https://gitlab.onelab.info/gmsh/gmsh) and GMSH can be compiled by hand. 

### Direct install (recommended)

The easiest way to install GMSH is to simply [download the binary files](https://gmsh.info) for Linux/Windows/Mac, either **Stable** or **Latest automatic Gmsh snapshot** (recommended (by me)). The binary file `gmsh` is located in `bin/gmsh` in the compressed downloaded file. You can launch it directly
```bash
cd folder/of/gmsh/bin
./gmsh
```

Apart from (maybe) the color theme, a new window should have been launched and looks like this

{{< figure src="img/gmsh_splash.png" title="GMSH splash screen. GMSH's version is displayed at the bottom of the window (here 4.5.0)" numbered="true" >}}

### Add `gmsh` to your `$PATH`

{{% alert warning %}}
**Never copy a binary file** in `usr/bin`. This folder is reserved to your package manager.
{{% /alert %}}

For Linux (and Mac) users, it might be convenient to be able to launch GMSH in a terminal and from whichever folder. To do that, simply add to the `$PATH` environment variable, the path to `gmsh`'s folder. This can be automated by adding a line in `$HOME/.bash_profile` (or `$HOME/.profile` or ...)
```bash
export PATH=$PATH:/path/to/gmsh
```
You must rerun your shell to get the changes applied (or usr `source` command).

### [Beginnner] How to manage your files?

A question that quickly arises is:

> Where and how do I store every binary and lib files without making my computer a full mess?

Good question.

There are multiple answers and here you'll find one which you may like (or not). I like to have a `$HOME/install` folder (local to my account, thus) which mimic the `/usr/` folder, *i.e.* containing `bin`, `lib`, `share` and `include` folders:

```bash
$HOME/install
      └── bin/      # binary files
          └── gmsh    #some binaries (gmsh, ...)
      └── lib/      # library files
          └── gmsh.py
          └── libgmsh.so
      └── share/      # dynamic libraries
      └── include/    # header files
```
I then set the `$PATH` environment variable to
```bash
export PATH=$PATH:$HOME/install/bin
```
Every time I add a new file to `$HOME/install/bin` it becomes accessible, without having to change the environment variable.

{{% alert note %}}
Actually I have three main folders

- `$HOME/src` : Source codes
- `$HOME/build` : Where the source codes are compiled
- `$HOME/install` : Installed software that I either compiled or downloaded

This way, source code, build process and installed file are well separated.
{{% /alert %}}


## API

{{% alert note %}}
Part of the tutorial is written in GMSH's language but using the API instead is direct and **highly recommended**.
{{% /alert %}}

GMSH provides a powerful API compatible with [Python](https://www.python.org/), [Julia](https://julialang.org/) or `C++`.  The [GMSH SDK](http://gmsh.info/)[^1] is available online and easy to install. 

### Local in the folder (meh)

For example on Unix system and for Python, a way to install it *locally* (without being root) and *by hand* ("*à la hussarde*") is to add this in this in the header of your Python file

```python
import sys
sys.path.insert(0, "/path/to/sdk/lib")
import gmsh
```

### Permanent linking (better)

A better method is to use the `$PYTHONPATH` environment variable and set it to the SDK's folder. Following the previous paragraph, create a folder `install` at your `$HOME` folder and copy the content of `lib` folder of the SDK's zip file inside `$HOME/install/lib`. You would then have this stucture

```bash
$HOME/install
      └── lib/      # library files
          └── gmsh.py
          └── libgmsh.so
```
You have now to add to Python the path:

```bash
export PYTHONPATH=$HOME/install/lib
```

A relaunch of the terminal might be necessary to take these changes into account. In your Python code, you juste have to import `gmsh` without any other condition

```python
import gmsh
```

{{% alert warning %}}
The binary file `bin/gmsh` contained in the SDK's zip file might not work as a standalone binary file. Please download and use the binary provided by GMSH's website for that use.
{{% /alert %}}


[^1]: Software Development Toolkit

