+++
title = "Geometries & Operations"

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
  parent = "occ"
  name = "Geometries & Operations"
  weight = 15

+++

## Géométries

### Exemple : un pavé

Ouvrez un fichier, par exemple `pave.geo` et placez le code suivant au tout début du fichier:
```cpp
  SetFactory("OpenCASCADE");
```
Cette commande spécifie à GMSH que l'on utilise dorénavant le moteur CAO OpenCascade. Pour celui-ci, la taille des mailles n'est plus définie au niveau des `Point` (bien que l'on doive/puisse continuer à le faire). Pour garder le contrôle sur la finesse de maille, nous devons utiliser les commandes `Mesh.CharacteristicLengthMin` et `Mesh.CharacteristicLengthMax`. Ajoutez à la suite du fichier ceci :
```cpp
h = 0.1; // ou ce que vous voulez
Mesh.CharacteristicLengthMin = h;
Mesh.CharacteristicLengthMax = h;
```
Au lieu d'ajouter des `Point` et des `Line`  et autres entités pour construire notre pavé, nous utilisons la commande `Box`
```cpp
Box(indice) = {x, y, z, lx, ly, lz};
```
où `indice` sera l'indice du `Volume` (uniquement !), `x`,`y`,`z` les coordonnées du point "en bas à gauche", tandis que `lx`,`ly`,`lz` sont les dimensions respectivement en x, y et z du cube. *Et c'est tout !* (⌐■_■).

{{% alert exercise %}}
À vous de jouer !

1. Créez un fichier `pave.geo` et réalisez cube unitaire (ou pas) à l'aide de cette commande
2. Maillez en 2D
3. Vérifiez [l'orientation des normales]({{< relref "tips_gui.md#options-mesh" >}})
4. Maillez en 3D
5. Admirez la simplicité
{{% /alert %}}


{{% alert note %}}
À l'aide de l'interface graphique nous pouvons vérifier [l'orientation des normales]({{< relref "tips_gui.md#options-mesh" >}}) (`Tools→Option→Mesh` puis spécifier une valeur (*e.g* 50) à Normals (en bas à gauche))
{{% /alert %}}

### Quelques géométries disponibles

Voici quelques géométries disponibles et leur syntaxe :

- `Disk(indice)= {x,y,z,R}` : Disque de rayon `R` et de centre `x`,`y`,`z`
- `Rectangle(indice)= {x,y,z,lx,ly}` : Rectangle dans le plan *(x,y)* dont le point "en bas à gauche" est de coordonnées `x`,`y`,`z` , et de dimension `lx` et `ly` .
- `Sphere(indice)= {x,y,z,R}` : Sphère de rayon `R` et de centre `x`,`y`,`z` .
- `Cylinder(indice)= {x,y,z, xv, yv, zv, R, alpha}` : Cylindre de rayon `R` à partir d’un cercle de centre `x`,`y`,`z` extrudé selon le vecteur `xv`,`yv`,`zv` . Le paramètre `alpha` est optionnel et représente l’ouverture angulaire.
- `Torus(indice)= {x,y,z,R1, R2}` : Tore de rayons `R1` et `R2` et de centre `x`,`y`,`z` .
- `Cone(indice)= {x,y,z, dx,dy,dz,R1, R2}` : Cône de centre `x`,`y`,`z` , de direction `dx`,`dy`,`dz` et de rayons `R1` et `R2` (qui peuvent être nuls).


## Opérations booléennes ou la CAO like a boss (⌐■_■)


Au nombre de 4, les opérations ont toute la même syntaxe et prennent deux arguments : un `objet` (1<sup>er</sup> argument) et un `outil` (2<sup>nd</sup> argument) :

```cpp
//Deux syntaxes possibles (attention au point virgule (ou pas)) :
// 1. Numéro de l'entité créée non connu  /!\ pas de ";" /!\
BooleanOperation{Object_List; Delete; }{Tool_List; Delete;} 
// 2. Entitée crée aura pour label "indice" (si possible) /!\ se termine par ";" /!\
BooleanOperation(indice) = {Object_List; Delete; }{Tool_List; Delete;} ;
```
Les `List` sont des listes d'entités (`Point, Line, Surface, Volume`). Par exemple, la liste des `Line` numéro 2, 3 et 6 ainsi que des `Surface` de tous les numéros de 42 à 50 et des `Volume` 11 et 15 s'écrira :
```cpp
Line{2,3,6}; Surface{42:50}; Volume{11,15};
```

{{% alert tips %}}
`Delete;` est optionnel mais souvent conseillée : quand cette option est spécifié, les entités mises en argument sont supprimées. Autrement, il y a un risque (non nul) de doublon !
{{% /alert %}}

Les 4 opérations possibles sont :

1. `BooleanIntersection{Liste; Delete;}{Liste; Delete;}`: Intersection entre l'`objet` et l'`outil`
2.  `BooleanUnion{Liste; Delete;}{Liste; Delete;}` : Union entre l'`objet` et l'`outil`
3. `BooleanDifference{Liste; Delete;}{Liste; Delete;}` : Soustraction de l'`outil` à l'`objet`
4. `BooleanFragments{Liste; Delete;}{Liste; Delete;}` : Calcul de chaque fragments résultant de l'intersection des entités de l'`objet` et de l'`outil` et **suppression de toute entité en doublon** en rendant **chaque interface unique**. 

{{% alert tips %}}
Souvent sous-estimée, `BooleanFragments` est probablement une des opérations les plus utiles.
{{% /alert %}}


### Un exemple

Rien de tel qu'un exemple pour prendre en main ces commandes avec celui du dé à 6 faces dont le code est le suivant (copiez/collez le dans un fichier `de.geo` par exemple) :
```cpp
SetFactory('OpenCASCADE');
Box(1) = {-0.5,-0.5,-0.5, 1, 1, 1};
Sphere(2) = {0,0,0, 0.65};
BooleanIntersection{ Volume{1}; Delete;}{ Volume{2}; Delete;}
```
Analysons ligne par ligne ce code. Tout d'abord, nous imposons le moteur de CAO OpenCascade, puis nous construisons un cube (`Box`) et une sphère (`Sphere`), dont les entités `Volume` associées auront pour numéro 1 et 2 respectivement. Ensuite, nous lançons la commande
```cpp
BooleanIntersection{ Volume{1}; Delete;}{ Volume{2}; Delete;}
```
L'intersection a lieu entre le `Volume{1}`, le cube, et le `Volume{2}`, la sphère. Notons que les deux `Volume` seront détruits après la commande, du fait des deux "`Delete;`". La fonction `BooleanIntersection` calcule l'intersection des deux `Volume`. Pour cela, GMSH va créer un nouveau `Volume` ainsi que les éventuelles `Surface` et `Line` et les numéroter. Ces numéros sont choisis comme les premiers indices disponibles. Comme nous avons détruit les `Volume` 1 et 2, il y a de forte chance que  l'indice du nouveau `Volume` soit 1 (le label 1 étant devenu disponible).

{{% alert warning %}}
Attention ! La numérotation a changé ! Il faut donc être **très vigilant** puisque la numérotation des entités (`Line`, `Surface`, `Volume`, ...) est **modifiée** par l'utilisation des `Delete` dans les opérations booléennes !
{{% /alert %}}


{{% alert warning %}}
La grande difficulté pour l'utilisateur réside maintenant dans la numérotation des entités. Il faut faire très attention : en cas d'utilisation de commandes booléennes, GMSH est parfois obligé de modifier, de manière importante, la numérotation des entités. Il est dès lors conseillé après avoir effectué une telle opération, de regarder **manuellement** les numéros des entités (nouvelles et anciennes) dans GMSH dans `tools→Visiblity` puis de choisir `Elementary Entities` dans le menu déroulant situé en bas à gauche. La fenêtre peut aussi s'ouvrir par le raccourci clavier `ctrl + shift + v`.
{{% /alert %}}


{{% alert note %}}
Sans la commande `Delete`, vous constaterez dans la fenêtre de visualisation (`Tools→Visiblity`), qu'il existe alors plus qu'un `Volume`. Cependant, ils se "chevauchent" certainement...
{{% /alert %}}



{{% alert exercise %}}
Réalisez une tête de bonhomme avec un chapeau conique comme sur l'image ci-dessous. Veillez à ce que, au final, il n’y ait qu’un seul `Volume`. Pour simplifier : la sphère est de centre (0,0,0) et de rayon 1 tandis que le cône est de paramètre (0, 0, 0.5, 0, 0, 1, 1.5, 0).
{{% /alert %}}


{{< figure src="../bonhomme.png" title="Bonhomme avec un chapeau conique" width="300" >}}


{{% alert exercise %}}
Réalisez une tasse comme sur l'image ci-dessous. Nous pouvons utiliser la rotation :
```cpp
Rotate{{x,y,z}, {xp,yp,zp}, a}{liste}
```
où `x`,`y`,`z` est le vecteur de l'axe de rotation, `xp`,`yp`,`zp` un point quelconque de l'axe de rotation, et `a` l'angle (en radian) de rotation. La `liste` contient les entités à modifier (`Line`, `Surface`, `Volume`, ...). La quantité π dans GMSH est obtenue par la commande `Pi`.
{{% /alert %}}


{{< figure src="../tasse.png" title="Tasse" width="300" >}}


###  Un mot sur `BooleanFragments`

Probablement la plus utile et la moins intuitive des fonctions, `BooleanFragments` effectue deux opérations :

- **Fragmente** les entités : chaque intersection entre les entités devient une entité à part entière avec son propre numéro
- **Supprime** tous les doublons, en particulier les frontières en commun

Prenons par exemple le cas d'un carré et d'un disque d'intersection non nulle :

```c++
SetFactory("OpenCASCADE");
Mesh.CharacteristicLengthMin = 0.1;
Mesh.CharacteristicLengthMax = 0.1;

Rectangle(1) = {-0.5,-0.5, 0, 1, 1};
Disk(2) = {0.75, 0, 0, 0.75, 0.75};

Mesh 2; // Maille automatiquement au lancement de GMSH
```
Après lancement nous obtenons un résultat décevant : deux maillages superposés !

{{< figure src="../fragments_before.png" title="Maillage superposé : avant `BooleanFragments`">}}

Pour remédier à cela, juste avant `Mesh 2;`, ajoutons :
```c++
BooleanFragments{ Surface{1,2}; Delete; }{}
```
Nous obtenons alors 3 entités mais le maillage est maintenant unique et connecté :

{{< figure src="../fragments_after.png" title="Maillage superposé : après `BooleanFragments`">}}


{{% alert note %}}
La place des arguments dans `BooleanFragments` n'a pas réellement d'importance. On peut donc avoir un `outil` vide par exemple.
{{% /alert %}}

## A little challenge

{{% alert exercise %}}
Essayez de reproduire la pièce ci-dessous

{{< figure src="../ecrou.png" title="Petit défi" >}}
{{% /alert %}}