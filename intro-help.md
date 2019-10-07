+++
title = "Help"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true
weight = 5
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="tutorial/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "tutorial/gmsh/"


# Add menu entry to sidebar.
[menu.gmsh]
  name = "Help"
  parent = "I. Introduction"
  weight = 5

+++


## Working Environement


### Text Editor

Text editor is the developper tool. As any tool, you must know how to use it **deeply**. This means I strongly advice you to take times (at least one hour!) only on your text editor. It's clearly not a loss of time: you will be more productive and efficient later.

There exists multiple open-source text editors such as the old-but-gold [Emacs](https://www.gnu.org/software/emacs/) and [VI/VIM](https://www.vim.org/) or more modern software as [VSCode](https://code.visualstudio.com/).

If you do not know which one to chose and even though Emacs and VIM are extremely powerful, I strongly **suggest you to pick up VSCode** because of its **fast learning curve** and the **numerous extension packages**. In short, you can have a powerful and efficient working environment in less than an hour.


### Syntax highlighting 

GMSH has its own parser and language, even though using the API is generally a better option (see below). If you want to work with `.geo` files (GMSH files), you might be interest in automatic indent and syntax highlighting:

- [VSCode](https://code.visualstudio.com/) through the [GMSH extension](https://marketplace.visualstudio.com/items?itemName=Bertrand-Thierry.vscode-gmsh).
- [Emacs](https://www.gnu.org/software/emacs/) via [this little file]([Emacs](https://github.com/Bertbk/emacs-mode). Note that this has not been updated for a while.
- $\LaTeX$ using the [`latex_gmsh` package](https://github.com/Bertbk/latex_gmsh). This home made package provides some commands as `\gmsh{blabla}` to display GMSH code in a nice way (using `lstlisting`).


### A closer look into VSCode

#### Extensions

I suggest you to install the following extensions, available on [the market](https://marketplace.visualstudio.com/) or directly in VSCode windows:

- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- [GMSH](https://marketplace.visualstudio.com/items?itemName=Bertrand-Thierry.vscode-gmsh)
- [Julia](https://marketplace.visualstudio.com/items?itemName=julialang.language-julia)
- [C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [VSCode icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons) or [Material icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager)

The first two are clearly useful for these tutorial and will provide numerous facilities. For example, working with the GMSH language (`.geo` file) would give you syntax coloration and automatic indent

![VSCode syntax coloration for GMSH](../vscode_gmsh.gif)

While working in Python with the API will provides you coloration, indent, suggestion and even help!

![VSCode syntax coloration for GMSH](../vscode_api.gif)

#### Terminal inside VSCode

Such as Emacs or VIM, a terminal can be opened *inside* VSCode by clicking on `Terminal` and `New Terminal`. This is very useful as you don't have to switch between text editor and the shell.


## Documentation

1. [Official documentation](http://gmsh.info/doc/texinfo/gmsh.html), also available as a [pdf file](http://gmsh.info/doc/texinfo/gmsh.pdf)
2. [GMSH's Gitlab](https://gitlab.onelab.info/gmsh/gmsh): for any issue or question
3. [Mailing list](http://www.geuz.org/mailman/listinfo/gmsh) (will probably be deprecated)
4. Numerous examples can be found in [the source code of GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos) or even in [the Onelab Models](https://gitlab.onelab.info/doc/models). For the API, [examples are provided](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos/api)
