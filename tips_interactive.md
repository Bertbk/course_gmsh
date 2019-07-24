+++
title = "Interactive GUI"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "tips&tricks"
  name = "Interactive GUI"
  weight = 5


+++

## Géométrie de référence

Reprenons le code modélisant un rectangle de dimension `lx` et `ly` :

```c++
h = 0.1; //Taille caractéristique des éléménts
lx = 2; // Longueur
ly = 1; // Largeur
Point(1) = {0, 0, 0, h};   // Construction des points
Point(2) = {lx, 0, 0, h};
Point(3) = {lx, ly, 0, h};
Point(4) = {0, ly, 0, h};
Line(1) = {1,2};   //Les lignes
Line(2) = {2,3};
Line(3) = {3,4};
Line(4) = {4,1};
Line Loop(1) = {1,2,3,4};   //Le  bord
Plane Surface(1) = {1};     //La surface du rectangle
Physical Surface(1) = {1};  //Ce que l'on souhaite sauvegarder
```

Ce code construit un rectangle.

### Rendu modifiable via l'interface graphique

Les paramètres `lx` et `ly` vont maintenant être rendu modifiable dans le GUI grâce à la commande `DefineNumber` de syntaxe :
```c++
DefineNumber[Var  = { DefautValue, ... options ...} ;
```
Un exemple valant mieux qu'un long discours, modifiez les lignes définissant `lx` et `ly` par :
```c++
lx = DefineNumber[2, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Lx"];
ly = DefineNumber[1, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Ly"];
```
Rechargez le fichier avec `GMSH`, vous devriez obtenir l'image ci-dessous. Sur la gauche, vous pouvez modifier en *live* les valeurs de `lx` et `ly`.


{{< figure src="../gui_interactive.png" title="Le GUI est maintenant interactif">}}



{{% alert note %}}
[Il existe de nombreuses options de personnalisation des variables](https://gitlab.onelab.info/doc/tutorials/wikis/ONELAB-syntax-for-Gmsh-and-GetDP)
{{% /alert %}}

{{% alert exercise %}}
Modifiez la ligne définissant `h` pour le rendre lui aussi modifiable par l'interface graphique avec par exemple un `Step` de 0.1 et tel que 0.1 < `h` < 1
{{% /alert %}}


## `DefineConstant`

Nous pouvons englober les `DefineNumber` à traver un `DefineConstant` global :
```c++
DefineConstant[
 // Attention, pas de virgule à la fin pour la derniere variable
  lx  = { 2, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Lx"},
  ly  = { 1, Min 0.5, Max 10, Step 0.1, Name "Geometrie/Ly"}
];
```

## `DefineString`

Cette commande fonctionne comme `DefineNumber` mais pour des chaînes de caractère
