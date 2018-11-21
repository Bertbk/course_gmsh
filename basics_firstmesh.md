+++
title = "First Mesh"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "basics"
  name = "First mesh"
  weight = 1

+++

## Mon premier carré

1. Recopiez le code ci-dessous dans un fichier `carre.geo`

        h = 1; //Taille caractéristique des éléménts
        Point(1) = {0, 0, 0, h};   // Construction des points
        Point(2) = {10, 0, 0, h};
        Point(3) = {10, 10, 0, h};
        Point(4) = {0, 10, 0, h};
        Line(1) = {1,2};   //Des lignes
        Line(2) = {2,3};
        Line(3) = {3,4};
        Line(4) = {4,1};
        Line Loop(1) = {1,2,3,4};   //Définition d'un pourtour (d'un bord)
        Plane Surface(1) = {1};     //Définition d'une surface
        Physical Surface(1) = {1};  //A sauvegarder dans le fichier de maillage

2. Ouvrez votre fichier dans GMSH `files->open`, ou alors le raccourci clavier `CTRL+o`). On peut aussi taper la commande `gmsh carre.geo` directement dans un terminal (attention, cela ouvre une nouvelle instance de GMSH).
3. Dans l'interface graphique apparait le carré. Avec la souris, vous pouvez effectuer effectuer une rotation (`click gauche`) ou une translation (`click droit`) de la caméra.  Le zoom/dézoom est possible avec `la molette`. Sur la fenêtre GMSH, les boutons `X,Y,Z` situés en bas à gauche permettent de remettre la caméra sur les axes.
4. Mailler ensuite en appuyant sur `2` (ou `shift+2`) ou bien via l'interface graphique en cliquant  `Mesh->2D`.
5. Le domaine est maintenant triangulé !


GMSH dispose d'une console permettant de suivre l'avancement et/ou d'étudier les erreurs. Il suffit de cliquer sur la barre en bas (plutôt à droite) de la fenêtre. Dans notre cas et après maillage, par exemple, la sortie donne
``` bash
Info    : Reading 'carre.geo'...
Info    : Done reading 'carre.geo'
Info    : Finalized high order topology of periodic connections
Info    : Meshing 1D...
Info    : Meshing curve 1 (Line)
Info    : Meshing curve 2 (Line)
Info    : Meshing curve 3 (Line)
Info    : Meshing curve 4 (Line)
Info    : Done meshing 1D (0.000981 s)
Info    : Meshing 2D...
Info    : Meshing surface 1 (Plane, Delaunay)
Info    : Done meshing 2D (0.017519 s)
Info    : 142 vertices 286 elements
```
Nous constatons que GMSH commence par mailler les lignes (Meshing 1D...) une à une, puis la surface. Dans notre cas, 142 nœuds (Vertices, en anglais) ont été créés pour un total de 286 éléments.


{{% alert note %}}
Vous pouvez sauvegarder le maillage avec `CTRL + Shift + s` ou alors via l'interface graphique `file->save mesh`. Par défaut, GMSH sauvegarde le maillage dans un fichier d'extension `.msh` et de même nom que le fichier `.geo`.
{{% /alert %}}


{{% alert note %}}
[Le GUI est détaillé dans la section dédiée]({{< relref "/tips_gui.md">}}).{{% /alert %}}

## Analysons le code ligne par ligne

### Le paramètre `h`

La première ligne définie une variable, `h`, qui sera la taille caractéristique des mailles.
La taille des mailles est définies au niveau des `Point`.

{{% alert exercise %}}
Jouons avec la finesse de maille :

1.  Modifiez le paramètre `h`, remaillez et observez le résultat.
2.  Dans notre code, à chaque `Point` est affecté le même `h`. Modifiez le code pour que les mailles proches du `Point(2)` soient de taille `h/10`.
3.  Remaillez.
{{% /alert %}}

{{% alert tips %}}
Une fois le fichier texte modifié, pour mettre à jour GMSH appuyez sur la touche zéro `0` (ou possiblement `shift` + `0`). Cela remet GMSH à zéro et relit le fichier texte.
{{% /alert %}}


### Les `Point`

Dans GMSH, on commence par définir les éléments 0D (points), 1D (lignes, courbes, ...), puis 2D (surfaces) et enfin les éléments 3D (volumes). Pour définir un point, la syntaxe est la suivante
```cpp
Point(indice) = {x, y, z, h};
```
Avec comme paramètres :

- `indice` : Numéro du point. Il peut être défini en dur (1,2,13242, ...) ou alors GMSH peut utiliser le prochain indice libre en utilisant la commande `newp` (pour "New Point").
- `x`, `y`, `z` : les coordonnées cartésiennes du point.
- `h` : la finesse de maille autour du point.


{{% alert exercise %}}
Effectuez les changements suivants :

  1. Modifiez le carré pour en faire un rectangle (non carré)
  2. Modifiez le rectangle pour en faire un parallèlogramme non rectangulaire
{{% /alert %}}


### Les `Line` et les `Line Loop`

Les lignes, ou plutôt les segments, relient deux points et leur syntaxe est la suivante
```c++
  Line(indice) = {PointA, PointB};
```
Avec comme paramètres :

- `indice` : Numéro de la ligne. Comme pour les points, la commande `newl` fournit le prochain indice libre de ligne.
- `PointA` : indice du point de départ
- `PointB` : indice du point d'arrivée

{{% alert warning %}}
Une `Line` est orientée !
{{% /alert %}}

Une `Line Loop` est une union fermée de lignes (attention à l'orientation), c'est un "bord" :
```c++
Line Loop(indice) = {Line1, Line2, ..., LineN};
```
Avec :

- `indice` : Numéro de la `Line Loop` (`newll` pour le prochain indice libre)
- `LineX` : indice de la X<sup>ème</sup> `Line`. Cette quantité peut être signée (*e.g.* -10) afin de parcourir le segment dans l'autre sens.

{{% alert exercise %}}
Reprenez le carré du début, puis :

- Ajoutez des `Point` et des `Line` et modifiez la `Line Loop` de sorte que la géométrie ressemble à un "L" (voir ci-dessous) avec une taille de maille égale à 1. Les coordonnées des points sont données dans le tableau ci-dessous :

| x | y |
|-------|--------|
| 0 | 0 |
| 100 | 0 |
| 100 | 30 |
| 50 | 30 |
| 50 | 100 |
| 0 | 100 |

- Sur le point situé en (50,30), appliquez un raffinement de maillage 10 fois plus fin qu'ailleurs. Remaillez et observez.

{{< figure library="1" src="teaching/gmsh/L.svg" title="L Shape">}}

{{% /alert %}}



### Les `Surface`

Une `Surface` est définie par son bord, ou plutôt, ses `Line Loop`. La syntaxe est la suivante
```c++
Plane Surface(indice) = {LineLoop1, LineLoop2, ..., LineLoopN};
```
Où l'on a :

- `indice` : Numéro de la `Surface` (`news` pour le prochain indice libre de surface)
- `LineLoopX` : indice de la X<sup>ème</sup> text `Line Loop`. Cette quantité peut être signée (*e.g.* -10) afin de parcourir la `Line Loop` dans l'autre sens.

{{% alert exercise %}}
Nous allons rajouter un "trou" carré dans notre "L". Pour cela, dans le code et avant la définition de la `Line Loop`, nous ajoutons un carré, avec les sommets, comme indiqué sur le tableau ci-dessous :

| x     | y      |
|-------|--------|
| 0     |  0     |
| 100   | 0     |
| 100   | 30     |
| 50    | 30    |
| 50    | 100    |
| 0     | 100     |
| 20    | 10     |
| 30    |  10    |
| 30    | 20     |
| 20    | 20     |

1. Ajoutez les nouveaux `Point` et `Line` pour construire ce carré "interne"
2. Ajoutez la `Line Loop` correspondante
3. Modifiez la définition de la `Surface` pour prendre en compte cette nouvelle `Line Loop`
4. Admirez le résultat en vous félicitant

{{< figure library="1" src="teaching/gmsh/L_hole.svg" title="L-Shape avec un trou carré">}}

{{% /alert %}}



### Les `Elementary Entities` et les `Physical Entities`

Nous laissons la ligne `Physical Surface(1)={1};` de côté, ce sujet étant traité plus en détail dans [la section consacré au format de sortie](gmsh_meshformat.md).

## `Extrude`: du carré au cube

Construisons un cube à partir du carré à l'aide [d'une extrusion](https://fr.wikipedia.org/wiki/Extrusion) selon la direction z donc selon le vecteur (0,0,1). Ajoutons au bout du fichier cette portion de code :
```cpp
Extrude {0, 0, 11} { Surface{1}; }
```

{{< figure library="1" src="teaching/gmsh/cube.png" title="Maillage du cube" width="300">}}


## De cercles en cercles

Ouvrez un fichier `cercle.geo` et insérez le code ci-dessous :
```c++
DefineConstant[
  R = {1, Min 0.5, Max 10, Step 0.1, Name "R"},
  h = {0.1, Min 0.01, Max 10, Step 0.01, Name "h"}
];
xc = 0;
yc = 0;
Point(1) = {xc, yc, 0, h};
Point(2) = {xc + R, yc, 0, h};
Point(3) = {xc, yc + R, 0, h};
Point(4) = {xc - R, yc, 0, h};
Point(5) = {xc, yc - R, 0, h};
Circle(1) = {2,1,3};
Circle(2) = {3,1,4};
Circle(3) = {4,1,5};
Circle(4) = {5,1,2};
Line Loop(1) = {1,2,3,4};   //Définition d'un pourtour (d'un bord)
Plane Surface(1) = {1};     //Définition d'une surface
Physical Surface(1) = {1};  //A sauvegarder dans le fichier de maillage
```

Si tout va bien, GMSH affiche un cercle. La syntaxe de `Circle` est la suivante :
```c++
Circle(indice) = {PointDepart, Centre, PointArrivee};
```
Pour dessiner un cercle, nous pourrions nous restreindre à 3 points uniquement plutôt que 4.

{{% alert warning %}}
GMSH ne peut pas construire d'arc de cercle d'angle **égal** ou **supérieur** à π.
{{% /alert %}}

{{% alert note %}}
Les `Circle` et les `Line` partagent le **même compteur**. Autrement dit, il ne peut exister une `Line` avec le même numéro qu'un `Circle`.
{{% /alert %}}


{{% alert exercise %}}
À vous de tester :

1. Introduisez un cercle intérieur de centre (0,0) et de rayon `Rint`, [modifiable dans le GUI]({{< relref "tips_interactive.md">}}), de valeur minimale 0.1 et maximale 0.4, un `Step` de 0.1.
2. Modifiez ensuite la `Surface` pour qu'elle représente la couronne ainsi formée plutôt que le disque original.
{{% /alert %}}

## Exercices supplémentaires

{{% alert exercise %}}
Dessinez le stade ci-dessous de manière à pouvoir le mailler. Attention, nous rappelons que GMSH ne peut pas tracer d'arc de cercle d'angle égal à π ou supérieur à π, il faut donc rajouter des points "intermédiaires" et découper l'arc de cercle en (au moins) deux.

{{<figure library="1" src="teaching/gmsh/stadium.svg"  title="Stade MAIN5">}}
{{% /alert %}}

