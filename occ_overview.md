+++
title = "Un moteur CAO plus performant"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math=false
weight = 30
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/gmsh", repo_branch = "master", submodule_dir="content/course/gmsh/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_gmsh"
  submodule_dir = "content/course/gmsh/"
  

# Add menu entry to sidebar.
[menu.gmsh]
  parent = "occ"
  name = "A powerful CAD engine"
  weight = 10

+++

## Pourquoi faire ?

Plutôt que d'utiliser le moteur de CAO interne à GMSH, nous utilisons dorénavant le moteur [OpenCascade](https://www.opencascade.com/). Les avantages sont les suivants :

- Géométries "prêtes à l'emploi" (cf. [La documentation \(cherchez "Cylinder" par ex.\)](http://gmsh.info/doc/texinfo/gmsh.html)) :
  - 2D : rectangle, disque, ...
  - 3D : pavé, sphère, tore, cône, cylindre, ...
- Gestion des Splines
- Outils Booléens classiques de CAO entre plusieurs entités comme :
  - Union
  - Différence
  - Intersection
  - Fragmentation

Le principal désavantage est que nous *ne connaissons plus a priori* les numéros de toutes les entités puisque c'est OpenCascade qui s'en charge. En particulier :

- Pour un maillage anisotropique (finesse de maille différente par région): il faut rechercher les points *a posteriori* pour leur assigner une taille de maille.
- Il est parfois nécessaire de faire des aller-retour avec l'interface graphique pour retrouver les numéros des entités intermédiaires (lignes, surfaces, ...)

Néanmoins, nous conseillons **fortement** d'utiliser ce moteur de CAO plutôt que celui de base.


## Un exemple

De nombreux exemples utilisant ces fonctionnaliés sont [disponibles dans les sources de GMSH](https://gitlab.onelab.info/gmsh/gmsh/tree/master/demos/boolean).
Nous en implémentons ici [un en particulier](https://gitlab.onelab.info/gmsh/gmsh/raw/master/demos/boolean/boolean.geo), qui reproduit [l'illustration de la page dédiée à la CAO dans Wikipédia](http://en.wikipedia.org/wiki/Constructive_solid_geometry) :


{{< figure src="../Csg_tree.png" title="Constructive solid Geometry (crédit: Wikipédia)" width="500" >}}

```cpp
SetFactory("OpenCASCADE");

// from http://en.wikipedia.org/wiki/Constructive_solid_geometry

Mesh.Algorithm = 6;
Mesh.CharacteristicLengthMin = 0.4;
Mesh.CharacteristicLengthMax = 0.4;

R = DefineNumber[ 1.4 , Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Box dimension" ];
Rs = DefineNumber[ R*.7 , Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Cylinder radius" ];
Rt = DefineNumber[ R*1.25, Min 0.1, Max 2, Step 0.01,
  Name "Parameters/Sphere radius" ];

Box(1) = {-R,-R,-R, 2*R,2*R,2*R};

Sphere(2) = {0,0,0,Rt};

BooleanIntersection(3) = { Volume{1}; Delete; }{ Volume{2}; Delete; };

Cylinder(4) = {-2*R,0,0, 4*R,0,0, Rs};
Cylinder(5) = {0,-2*R,0, 0,4*R,0, Rs};
Cylinder(6) = {0,0,-2*R, 0,0,4*R, Rs};

BooleanUnion(7) = { Volume{4}; Delete; }{ Volume{5,6}; Delete; };
BooleanDifference(8) = { Volume{3}; Delete; }{ Volume{7}; Delete; };
```

{{% alert exercise %}}
Copiez/Collez le code dans un fichier `.geo`. Maillez en 2D pour voir apparaitre la géométrie voulue.
{{% /alert %}}