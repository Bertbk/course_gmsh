+++
title = "Graphical Interface"

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
  name = "Graphical Interface"
  weight = 20

+++

Rappel : GUI = Graphical User Interface (= l'interface graphique).

## Description générale

Pour accéder aux options : `Tools->Options` (ou `shift + ctrl + N`). Ce menu est décomposé en catégories :

- `General`
- `Geometry`
- `Mesh`
- `Solver`
- `Post-Pro`
- Autant de catégories que de solutions actuellement lues : `View [0]`, `View [1]`, ...

Dans GMSH, une `View` est une visualisation d'une solution : vous pouvez affichez autant de solutions sur une fenêtre GMSH que vous le souhaitez.

## Caméra

La caméra se manipule comme pour la visualisation de maillage :

- Bouton gauche de la souris : rotation de la caméra
- Bouton droit de la souris : translation de la caméra
- Molette : zoom/dézoom

Ne pas hésiter à utiliser les boutons `X`, `Y` et `Z` en bas à gauche pour remettre la caméra sur un axe prédéfini

{{< figure src="../gui_camera.png" title="Camera">}}


{{% alert tips %}}
En appuyant sur la touche `r` alors GMSH recharge le fichier (reset).
{{% /alert %}}



## Options `General`

Les options `General` sont divisées en plusieurs onglets, notamment :

### `General` (oui, encore)

Parmi les options que  pratiques pour nos tutoriels :

- `Use dark interface` : la fenêtre GMSH devient alors moins éblouissante.
- `Show bounding box` : Affiche les coins (et les coordonnées) de la boite entourant votre géométrie (bounding box)

{{% alert tips %}}
Pour sauvegarder les options pour les instances futures : `File->Save options as default`.
{{% /alert %}}

### `Axes`

Vous pouvez ici supprimer le petit axe en bas à droite (pour enregistrer de belles figures, par exemple).

### `Color`

Vous pouvez jouer avec la position de la source de lumière (artificielle), en haut à droite.
Nous pouvons enlever l'effet de lumière, mais cela se situe dans le menu dédié à chaque `View`.


## Options `Geometry`

Citons dans l'onglet `Visibility` la possibilité d'afficher les Normales des surfaces ainsi que la possiblité d'afficher (ou non) les surfaces et les volumes.


## Options `Mesh`

Parmi toutes les options, citons dans l'onglet `Visibility` la possibilité d'afficher :

- Les éléments segments, qui **ne sont pas affichés par défaut** et qui peut entraîner parfois une certaine confusion
- Les faces des éléments : pour les rendre opaques (visiblité)
- Les Normales et les Tangentes (en bas) :

{{< figure src="../gui_normals.jpg" title="Affichage des normales">}}


## Options spécifiques à une `View` (Post-Processing)

{{% alert warning %}}
Pour ce menu, il faut visualiser une solution éléments finis obtenues, par exemple, avec [GetDP](http://getdp.info) ou [FreeFem++](https://freefem.org)
{{% /alert %}}

La catégorie qui nous intéresse  est `View[0]`. Celle-ci comporte plusieurs onglets que nous détaillons :

- `General`
- `Axes`
- `Visibility`
- `Transfo`
- `Aspect`
- `Color`
- `Map`

Il est possible de sauvegarder les options de visualisations d'une `View` donnée. Cela permet, en rouvrant le fichier avec GMSH, de conserver les différentes options choisies (transfo, etc.). Pour cela : `File->Save Model Option`. Un fichier `.opt` sera sauvegardé à côté de votre fichier `.pos`.

### `General`

Parmi les options qui vous peuvent vous intéresser ici :

- `Interval type` : surtout **ne choisissez jamais** "numeric values" sinon GMSH va probablement planter (affichage de chaque valeur en chaque noeud !). Par contre, `isovalues` ou `filled isovalues` sont intéressantes. Accessible depuis le menu rapide (double clic).
- `Range mode`
  - `Per Time Step` : l'échelle change selon que l'on observe la partie réelle ou imaginaire. Accessible depuis le menu rapide (double clic).
  - `Custom` : Permet d'effectuer une "coupe" dans les valeurs en limitant l'affichage des valeurs comprises dans un certain intervalle.
  - `Step` : Dans GMSH, les `Step` sont des pas de temps, mais c'est aussi dedans que sont rangées les parties réelles (`Step 0`) et imaginaire (`Step 1`).

### Axes

Si vous souhaitez afficher des axes : libre à vous de tester.

### Visibility

-  L'option `Draw element outlines` permet de superposer (ou non) le maillage.
- `Time Display` : choisissez `Harmonic Data`, de sorte à afficher "real part" et "imaginary part" après le nom de la `View`.


### Transfo

Nous pouvons ici :

- Transformer (translation, rotation, ...) la solution : il faut changer la matrice (rotation) et le vecteur (translation).
- `Normal Raise` : Permet d'afficher en 3D une solution 2D.  Accessible depuis le menu rapide (double clic). Exemple dans l'image ci-dessous :

{{< figure src="../gui_normalraise.png" title="Exemple avec NormalRaise">}}


### Color

Si vous n'aimez pas l'effet de lumière, vous pouvez le désactiver (`Enable lighting`).


### Color Map

Pour modifier la "color map" : soit à la main (chaud), soit avec des ensembles prédéfinis que vous pouvez sélectionner en tapant 0,1,2,3...,9.


## Quelques plugins

Ces plugins sont accessibles dans `Tools->Plugins`. Ils permettent de modifier et d'effectuer quelques opérations simples sur les solutions obtenues. Attention, les plugins ne modifient pas les fichiers, pour cela il faudra eregistrer la (nouvelle) solution sur disque.

### ModulusPhase}

Permet de calculer le module d'une `View`. Attention, la `View` est écrasée.

Il n'est a priori pas la peine de modifier les options par défaut, sauf éventuellement le numéro de la `View` (-1 signifiant la `View` en cours).

### ModifyComponents

Permet d'effectuer une opération mathématique sur les composants de la solution. Comme nous travaillons en scalaire, il n'y a qu'une composante. L'aide associée permet de bien comprendre.

## Supprimer une `View`, la recharger, ...

- Pour rendre une `View` visible, il faut cocher la case à gauche de son nom.
- Pour supprimer une `View`, il suffit de cliquer sur la flèche à droite de son nom et de choisir `Remove`. Nous pouvons aussi toutes les supprimer ou juste celles visibles.
- Pour recharger une `View` (*i.e.*: relire sur disque), soit le raccourci avec la touche `r` soit la flèche à droite de son nom et `Reload`.



## Exportation en image (png, jpg, ...)

Pour exporter une image sous GMSH, il suffit de faire `Files->Export`. Soit vous sélectionner dans le menu le format désiré, soit en tapant le nom du fichier de sortie et son extension, GMSH trouvera automatiquement le bon format (png, jpg, ...).
