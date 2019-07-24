+++
title = "Spline and Extrusion by Rotation"

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
  parent = "occ"
  name = "Spline and Extrusion by Rotation"
  weight = 30


+++

## Extrusion par rotation

La syntaxe pour une telle extrusion est la suivante :
```cpp
Extrude { { xr, yr, zr }, {xp, yp, zp}, alpha } { Liste}
```
Avec (`xr`, `yr`, `zr`) un vecteur de l'axe de rotation, (`xp`, `yp`, `zp`) un point situé sur l'axe et `alpha` l'angle de rotation en radians (rappel: `Pi` pour π). La `Liste` contient les entités à extruder. Prenons un exemple simple :
```cpp
SetFactory('OpenCASCADE');
h = 0.1;
Point(1) = {0,1,0,h};
Point(2) = {0.5,0.5,0,h};
Point(3) = {0,0,0,h};
Line(1) = {1,2};
Line(2) = {2,3};
Extrude { {0,1,0}, {0,0,0}, 2*Pi}{Line{1,2};}
```
Vous devriez obtenir deux cônes l'un sur l'autre, un peu comme un diamant.


{{% alert exercise %}}
Avec cette méthode, dessinez un sapin de Noël comme sur la figure ci-dessous.

{{< figure src="../sapin.png" title="Sapin" >}}

{{% /alert %}}


## Spline

Une spline est une fonction définie par morceaux par des polynômes. Dans GMSH, les `BSpline` sont des `Spline` qui ne passent pas nécessairement par les points de contrôle (notez que les `BSpline` **ne sont pas** disponibles avec le moteur `OpenCASCADE`). La syntaxe des `Spline` et `BSpline` est la suivante

- `Spline(indice) = {p1, p2, ..., pN}` : Créer une Spline qui *passe* par les `Point` `p1`,`p2`,...,`pN`
- `BSpline(indice) = {p1, p2, ..., pN}` : Créer une B-Spline *à l'aide* des points de contrôle `p1`,`p2`,...,`pN`.

{{% alert warning %}}
Les `BSpline` **ne sont pas** disponibles avec `OpenCASCADE` !
{{% /alert %}}

Comme toujours, rien de tel qu'un exemple :
```cpp
SetFactory("OpenCASCADE");
h = 1;
Mesh.CharacteristicLengthMin = h;
Mesh.CharacteristicLengthMax = h;
Point(1) = {0,4,0,h};
Point(2) = {5,2,0,h};
Point(3) = {10,8,0,h};
Point(4) = {15,4,0,h};
Point(5) = {20,10,0,h};
Spline(1) = {1,2,3,4,5};
```
Rajoutons en plus de cela l'extrusion pour obtenir un joli vase (ouvert) (maillez en 2D pour voir le résultat):
```cppp
Extrude { {1,0,0}, {0,0,0}, 2*Pi} { Line{1} ;}
```
Pour fermer le vase, il faudrait rajouter deux lignes (en haut et en bas) et modifier la commande `Extrude` ainsi :
```cpp
Point(6) = {20,0,0,h};
Point(7) = {0,0 ,0,h};
Line(2) = {5,6};
Line(3) = {7,1};
Extrude { {1,0,0}, {0,0,0}, 2*Pi} { Line{1,2,3} ;}
```

{{% alert tips %}}
À l'aide de la fenêtre 'Visibility' (`Tools->Visibility`) pour retrouver les numéros des surfaces, construire le `Volume` associé au vase. Pour cela, construire une `Surface Loop` puis le `Volume`.
{{% /alert %}}


{{% alert exercise %}}
À vous de jouer : représentez une fiole à l'aide des `Spline`, avec ou sans support.

{{< figure src="../fiole.png" title="Fiole avec ou sans support" >}}

{{% /alert %}}

