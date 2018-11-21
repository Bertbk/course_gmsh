+++
title = "Physical Entity and mesh format (v2)"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  issue = "https://github.com/Bertbk/course_gmsh/issues"
  prose = "https://prose.io/#Bertbk/course_gmsh/edit/master/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "basics"
  name = "Physical Entity and mesh format (v2)"
  weight = 10

+++

{{% alert warning %}}
Si vous utilisez GMSH en version 4 ou supérieur, vous devez ajouter ceci dans vos fichiers `.geo` :
```
Mesh.MshFileVersion = 2.2;
```
Nous nous restreignons au format de maillage 2.2 qui est plus facile à comprendre. 
{{% /alert %}}

## Format de sortie de maillage v2

Le format de sortie v2 est le suivant (sans les annotations) :

```bash
$MeshFormat
2.2 0 8         # version de format du fichier
$EndMeshFormat
$Nodes
Ns              # Nombre de sommets
1 x_1 y_1 z_1   # Numéro et coordonnées du 1er sommet
2 x_2 y_2 z_3   # ...
3 x_3 y_2 z_3
...
i x_i y_i z_i
...
Ns x_Ns y_Ns z_Ns
$EndNodes
$Elements
Ne              # Nombre d'éléments (tout type)
...
i        TypeElem NTags tag1 tag2 ... tagNTags v1 v2 ... vN
...
$EndElements
```
- `Ns`: Nombre de sommets du maillage
- `Ne`: Nombre d'éléments du maillage (tout type confondu)
- `TypeElem` : le type (=entier) de l'élément, nous retiendrons juste :
  - `TypeElem` = 15 : Point
  - `TypeElem` = 1  : Segment (à 2 sommets)
  - `TypeElem` = 2  : Triangle (à 3 sommets)
- Tags : Par défaut, `NTags`=2 avec `tag1` représentant le numéro `Physical` et `tags2` le `Partition Number` (pour la méthode de décomposition de domaines).
- `v1, v2, ... vN` : les indices des sommets de l'élément

## Exemple

1. Ouvrez un fichier `carre.geo` et copier/coller le code suivant :

    <!-- language: lang-c++ -->
        h = 1;
        Point(1) = {0, 0, 0, h};
        Point(2) = {2, 0, 0, h};
        Point(3) = {2, 2, 0, h};
        Point(4) = {0, 2, 0, h};
        Line(1) = {1,2};
        Line(2) = {2,3};
        Line(3) = {3,4};
        Line(4) = {4,1};
        Line Loop(1) = {1,2,3,4};
        Plane Surface(1) = {1};
        Physical Surface(1) = {1};

2. Lancez GMSH, maillez et sauvegardez le maillage. Avec un terminal, cela se fait rapidement

    <!-- language: lang-bash -->
        gmsh carre.geo -2 -o carre.msh

3. Ouvrez le fichier de maillage `carre.msh` avec un éditeur de textes. Vous devriez obtenir quelque chose comme ceci (sans les annotations !):

```bash
$MeshFormat
2.2 0 8
$EndMeshFormat
$Nodes
13        # 13 noeuds
1 0 0 0   # Les 4 premiers sont les "nôtres"
2 2 0 0
3 2 2 0
4 0 2 0
5 1.000000002713193 0 0
6 2 1.000000002713193 0
7 0.9999999951861425 2 0
8 0 0.9999999951861425 0
9 0.9999999994748339 0.9999999996061253 0
10 0.4999999975930711 1.499999997593071 0
11 1.499999998665244 1.50000000057983 0
12 0.5000000005470067 0.4999999986980669 0
13 1.500000000547007 0.5000000005798295 0
$EndNodes
$Elements
16                 # 16 éléments
1 2 2 1 1 4 8 10   # un triangle (type =2)
2 2 2 1 1 3 7 11   # 2eme triangle
3 2 2 1 1 1 5 12   # ...
4 2 2 1 1 2 6 13
5 2 2 1 1 7 10 9
6 2 2 1 1 8 9 10
7 2 2 1 1 8 12 9
8 2 2 1 1 7 9 11
9 2 2 1 1 5 9 12
10 2 2 1 1 6 11 9
11 2 2 1 1 13 9 5
12 2 2 1 1 6 9 13
13 2 2 1 1 3 11 6
14 2 2 1 1 2 13 5
15 2 2 1 1 4 10 7
16 2 2 1 1 1 12 8
$EndElements
```


{{% alert note %}}
Seuls des triangles ont été sauvegardés, aucun élément de bord ne l'a été. Cela est du aux `Physical Entities`.
{{% /alert %}}

## `Elementary Entities` vs. `Physical Entities`

Une `Elementary Entity` est : un `Point`, une `Line` (droite ou courbe), une `Surface` ou un `Volume` et possède un numéro unique, correspondant au numéro fournit lors de la création d'une entité, par exemple 10 dans
``` c++
Line(10) = {1,2}; // Le numéro de l'entité élémentaire est 10
```

Une `Physical Entity` englobe une ou plusieurs `Elementary Entities` et c'est ce label qui sera enregistré dans le fichier de maillage.
Autrement dit, c'est grâce à que l'assembleur éléments finis (comme par exemple [FreeFem++](http://www.freefem.org) ou [GetDP](https://getdp.info)), saura discrimer les éléments selon leur emplacement (par exemple si le triangle appartient à $\Omega_1$ ou $\Omega_2$).

{{% alert warning %}}
La règle est la suivante :

- Si **aucune** `Physical Entity` n'est définie alors GMSH sauvegardera **toutes les entités** et imposera un label `Physical` égal au numéro `Elementary`.
- Si (au moins) une `Physical Entity` est définie alors GMSH **ne sauvegardera que** les entités définies dans une `Physical Entity`.
{{% /alert %}}

{{% alert note %}}
Le numéro `Physical` est le premier `Tag` sauvegardé dans le fichier de maillage.
{{% /alert %}}


{{% alert note %}}
Un corrolaire des règles ci-dessus : si l'on souhaite que le fichier de maillage contienne des éléments de bords (lignes, ...) pour pouvoir par exemple y appliquer une condition de Neumann, il faut alors créer une `Physical Entity` (par exemple `Physical Line` pour une ligne).
{{% /alert %}}


{{% alert tips %}}
Il est toujours préférable de spécifier les `Physical` pour garder le contrôle des `Tags`. Il est également conseillé de définir les `Physical Entities` **uniquement à la fin** du fichier `.geo` et non au milieu (au cas où certaines entités sont détruites).
{{% /alert %}}

{{% alert exercise %}}
Modifiez le fichier ainsi :

- Supprimez la ligne  `Physical Surface(1) = {1};`, maillez et sauvegardez. Rouvrez le fichier de sortie de maillage : vous devriez voir apparaître des éléments non triangulaires.
- Ajoutez maintenant `Physical Line(2)={1};` à la fin de votre code et sauvegardez le maillage. Re-ouvrez le fichier de maillage : vous ne devriez voir que des éléments segments.
{{% /alert %}}

## Autres formats de sortie

GMSH fournit un grand nombre de formats de sortie de maillage comme par exemple Matlab ou VTK (ParaView). D'autre part, des convertisseurs sont disponibles [dans les sources de GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/utils/converters) pour, par exemple, Python.
