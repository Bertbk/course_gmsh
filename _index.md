+++
title = "Getting Started"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true
weight = 1
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"


# Add menu entry to sidebar.
[menu.gmsh]
  name = "Getting Started"
  weight = 1

+++

## What is it?

Open-source software for 2D/3D mesh and data visualization, developped in Belgium by (mainly) [C. Geuzaine](https://geuz.org) and [J-F. Remacle](https://perso.uclouvain.be/jean-francois.remacle/).

## Install (without being root!)

{{% alert warning %}}
Linux user: please **do not install GMSH via package manager** (`apt-get`, ...): version is outdated.
{{% /alert %}}


Source code are available on [Onelab's Gitlab](https://gitlab.onelab.info/gmsh/gmsh) and GMSH can be compiled by hand. 

The easiest way to install GMSH is however to simply [download the binary files](https://gmsh.info) for Linux/Windows/Mac, either **Stable** or *Latest automatic Gmsh snapshot* (recommanded (by me)). The binary file `gmsh` is located in `bin/gmsh` in the compressed file.

For Linux user (and Mac), it might be convenient to launch GMSH from a terminal, wherever your are. To do that, simply add the path to `gmsh`'s folder to the `$PATH` environment variable. This can be automated by adding a line in `$HOME/.bash_profile` (or `$HOME/.profile` or ...)
```bash
export PATH=$PATH:/path/to/gmsh
```

You may need to relaunch the terminal but then you should be available to launch GMSH by typing
```bash
gmsh
```

Apart from (maybe) the color theme, a new window should have been opened

{{< figure src="gmsh_splash.png" title="GMSH splash screen. GMSH's version is displayed at the bottom of the window (here 4.5.0)" numbered="true" >}}

{{%alert note %}}
A not so stupid idea is to create a folder, for example `$HOME/install/bin`, where its path it stored in `$PATH` and then move `gmsh` binary file into it. This way, you can add other binary files to this local `bin` folder.
{{% /alert %}}

{{% alert warning %}}
**Never copy a binary file** in `usr/bin`. This is reserved to your package manager.
{{% /alert %}}


## Syntax highlighting

GMSH has its own parser and language, even though using the API is generally a better option (see below). If you want to work with `.geo` files (GMSH files), you might be interest in automatic indent and syntax highlighting:

- [VSCode](https://code.visualstudio.com/) through the [GMSH extension](https://marketplace.visualstudio.com/items?itemName=Bertrand-Thierry.vscode-gmsh).
- [Emacs](https://www.gnu.org/software/emacs/) via [this little file]([Emacs](https://github.com/Bertbk/emacs-mode). Note that this has not been updated for a while.
- $\LaTeX$ using the [`latex_gmsh` package](https://github.com/Bertbk/latex_gmsh). This home made package provides some commands as `\gmsh{blabla}` to display GMSH code in a nice way (using `lstlisting`).


## API

{{% alert note %}}
Part of the tutorial is written in GMSH's language but using the API instead is direct and **highly recommended**.
{{% /alert %}}

GMSH provides a powerful API compatible with [Python](https://www.python.org/), [Julia](https://julialang.org/) or `C++`.  The [GMSH SDK](http://gmsh.info/)[^1] is available online and easy to install. 

For example on Unix system and for Python, a way to install it *locally* (without being root) and *by hand* ("*à la hussarde*") is to add this in this in the header of your Python file

```python
import sys
sys.path.insert(0, "/path/to/sdk/gmsh_api/gmsh/lib")
import gmsh
```


[^1]: Software Development Toolkit


## Documentation

1. [Official documentation](http://gmsh.info/doc/texinfo/gmsh.html), also available as a [pdf file](http://gmsh.info/doc/texinfo/gmsh.pdf)
2. [GMSH's Gitlab](https://gitlab.onelab.info/gmsh/gmsh): for any issue or question
3. [Mailing list](http://www.geuz.org/mailman/listinfo/gmsh) (will probably be deprecated)
4. Numerous examples can be found in [the source code of GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos) or even in [the Onelab Models](https://gitlab.onelab.info/doc/models). For the API, [examples are provided](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos/api)
