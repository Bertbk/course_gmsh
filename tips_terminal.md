+++
title = "Terminal commands"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  issue = "https://github.com/Bertbk/course_gmsh/issues"
  prose = "https://prose.io/#Bertbk/course_gmsh/edit/master/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "tips&tricks"
  name = "Terminal commands"
  weight = 10

+++

## Mailler et sauvegarder sans lancer l'interface graphique (pratique pour des scripts)

```bash
    gmsh carre.geo -2
```
On peut définir le fichier de sortie en rajoutant `- NomDuFichier` Pour un maillage 3D, remplacer `-2` par `-3`

##  Modifier la valeur d'une `Constant`: `-setnumber`

Par exemple, si l'on veut modifier la valeur de `h` à 10 :
```bash
    gmsh L.geo -2 -setnumber h 10
```
Pour modifier plusieurs valeurs, il faut ajouter l'option `-setnumber` à chaque fois.
```bash
    gmsh L.geo -2 -setnumber h 10 -setnumber xmax 50
```

## Rediriger la sortie vers un fichier

```bash
    gmsh L.geo -2 $1>NomDuFichier
```
