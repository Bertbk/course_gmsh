+++
title = "Overview"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

# Add menu entry to sidebar.
[menu.gmsh]
  name = "Overview"
  weight = 1

+++

## GMSH

Logiciel open-source de visuliation et de maillage 2D/3D, développé en Belgique, par (entres autres) [C. Geuzaine](https://geuz.org) et [J-F. Remacle](https://perso.uclouvain.be/jean-francois.remacle/).

- Téléchargement de binaires sur [https://gmsh.info](https://gmsh.info), deux choix possibles :
  - *Current Stable Release*
  - *Latest automatic Gmsh snapshot* (préférez en général cette version)
- [Code source](https://gitlab.onelab.info/gmsh/gmsh)

## Comment débuter

- Lancez votre éditeur de textes préféré ([VSCode](https://code.visualstudio.com/), [emacs](https://www.gnu.org/software/emacs/), [vim](https://www.vim.org/), ...). Notez qu'il existe une coloration syntaxique pour [Emacs](https://github.com/Bertbk/emacs-mode) et [$\LaTeX$](https://github.com/Bertbk/latex_gmsh).
- Lancez GMSH, pour cela dans un terminal, tapez simplement `gmsh`
- Pour vous facilitez la vie, gardez ces deux fenêtres (éditeur de textes - GMSH) ouvertes, l'une à côté de l'autre (si l'écran n'est pas trop petit)
- Vous pouvez modifier les [options de visualisation de GMSH]({{< relref "tips_gui.md#general-oui-encore" >}})

## API GMSH

Au lieu d'utiliser le langage propre à GMSH, il est possible d'utiliser l'API de GMSH avec [Python](https://www.python.org/) ou [Julia](https://julialang.org/) en téléchargeant la [SDK de GMSH](http://gmsh.info/)[^1]. Dans ce tutoriel, nous utilisons le langage propre à GMSH, cependant la conversion à la SDK est directe.

[^1]: Software Development Toolkit

## Documentation

1. **Officielle :** relativement exhaustive, n'hésitez pas à lire la documentation officielle :
  - [En ligne](http://gmsh.info/doc/texinfo/gmsh.html)
  - [En PDF](http://gmsh.info/doc/texinfo/gmsh.pdf)
2. **Mailing list :** Vous pouvez aussi souscrire ou fouiller dans les archives de [la mailing-list de GMSH](http://www.geuz.org/mailman/listinfo/gmsh).
3. **Exemples :** De nombreux exemples se trouvent dans le [code source de GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos) ou éventuellement dans [les modèles Onelab](https://gitlab.onelab.info/doc/models).