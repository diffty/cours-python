# TD4: Convertisseur d'image en ASCII 

## Intro

Aujourd'hui on va faire de l'ASCII Art à partir d'une image !


## PARTIE 1

### 1 · Choisir une image

De préférence :
* Pas trop grande
* Avec un fort contraste

Si le contraste n'est pas assez fort ou que la taille est un peu trop importante, n'hésitez pas à l'accentuer sur Photoshop


### 2 · Le module `pillow`

Afin d'ouvrir l'image, la décoder et lire chacun de ses pixel, nous allons, pous nous aider, utiliser le module Python `pillow` (autrefois appelé {abbr}`PIL (Python Imaging Library)`).

Il s'agit d'un module très populaire permettant de créer et manipuler des images de tout format, les ouvrir, redimensionner, cropper, convertir, leur appliquer des filtres... Et bien sûr les disséquer voire altérer pixel par pixel.

:::{note}
C'est ce même module que nous avons utilisé dans le [TD précédent](./td-glitch.md) afin de vérifier que notre image était bien lisible !
:::

:::{admonition} RAPPEL: Installer un module via `pip`
:class: tip, dropdown
Ouvrez une invite de commande Windows (ou un Terminal dans VSCode), tapez la commande suivante et validez avec {kbd}`ENTRÉE`.
```python
pip install pillow
```
:::


### 3 · Import de `pillow` (aka. `PIL`)

Maintenant que l'on est assuré que `pillow` se trouve sur notre machine, nous allons pouvoir ouvrir notre image ! Mais tout d'abord, nous allons l'importer dans notre script.

:::{admonition} Importer depuis un module externe
:class: tip, dropdown

Afin d'utiliser la puissance de PIL dans notre script, nous devrons importer l'objet `Image`, contenu dans le module `PIL` (qui est le nom historique du projet sur lequel se base `pillow`, probablement conservé pour des raisons de compatibilité)

```python
from PIL import Image
```

Cette ligne permet d'aller "piocher" dans le module `PIL`, et d'y sortir l'objet `Image` afin de le mettre à disposition du code de notre script, comme si nous l'avions déclaré et écrit nous-même dans notre propre `.py`.
:::


### 4 · Ouverture de l'image

Nous allons enfin pouvoir ouvrir notre image, en utilisant la [méthode](./cours.md#les-méthodes) `.open()` de l'[objet](./cours.md#classes--objets) `Image`.

:::{note}
Cette méthode s'apparente énormément à la fonction `open()`, intégrée à Python, et que nous connaissons plutôt bien puisque nous l'avons utilisé lors des précédents TP.

Cependant, ces deux outils n'ont rien à voir entre eux et ne partage que leur nom, leur fonctionnement leur étant propre ; par exemple, pour `Image.open()`, seul le chemin vers l'image à ouvrir sera nécessaire, **pas besoin** de spécifier un mode d'ouverture (les `'r'`, `'w'`, `'rb'`, `'wb'`...).
:::

:::{admonition} Ouvrir une image avec `pillow`
:class: tip, dropdown

Pour ouvrir une image, on utilise la ligne suivante :

```python
im = Image.open("C:/chemin/vers/votre/image.png")
```

Cette ligne créera un [objet](./cours.md#classes--objets) de type [`Image`](https://pillow.readthedocs.io/en/stable/reference/Image.html), et le stockera dans la variable `mon_image`.

C'est cet objet qui nous permettra de manipuler notre image !
:::


### 5 · Récupération des dimensions de l'image

En premier lieu, nous allons pouvoir récupérer les dimensions de notre image à présent ouverte, à savoir sa largeur et sa hauteur en pixels.

Pour cela, nous pourrons utiliser la liste stockée dans l'attribut [`.size`](https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.size) intégré à notre objet `mon_image`, et où `pillow` a stocké, juste après décodage, les dimensions de l'image qu'il a pu réussir à déchiffrer sous forme d'une liste à 2 cases, contenant respectivement la largeur et la hauteur de l'image en pixels.

:::{admonition} Lecture des dimensions de l'image
:class: tip, dropdown

```python
largeur = mon_image.size[0]
hauteur = mon_image.size[1]
```
:::

:::{admonition} **🚨 ATTENTION 🚨**
:class: warning

`.size` est un [**attribut**](./cours.md#les-attributs) de `mon_image` et non une [**méthode**](./cours.md#les-méthodes) !

Le fonctionnement d'un **attribut** s'apparente à 100% à une **variable**, mais qui serait intégrée à un objet. Ne lui mettez donc pas de `()` après son nom, sinon Python considèrera que vous essaierez d'appeler une **méthode** ! (qui, elles, s'apparentent à une fonction, mais intégrée à un objet, tout comme `im.getpixel()` que nous verrons juste après)
:::


### 6 · Créer 2 boucles imbriquées pour parcourir l'image

Il s'agira ici de créer 2 boucles **imbriquées**, qui permettront de parcourir l'ensemble des pixels de l'image, du coin supérieur gauche de l'image à son coin inférieur droit.

Pour cela, nous utiliserons une première boucle, qui se **répétera autant de fois qu'il y a de pixels en hauteur de l'image**. Et ce afin de la parcourir **ligne de pixels par ligne de pixels**. Nous utiliserons donc une boucle de type `for y in range(...)`, en utilisant la **variable contenant la hauteur de l'image** récupérée en [(5)](#5--récupération-des-dimensions-de-limage) comme paramètre du `range(  )`.

Ensuite, la seconde boucle, de nouveau de type `for x in range(...)`, sera à écrire **à l'intérieur** de la première (d'où l'**imbrication**), et elle se répétera autant de fois qu'il y a de pixels en largeur de l'image. Cette seconde boucle permettra ainsi de parcourir notre image de **gauche à droite**, et ce pour chaque ligne.

Ainsi, nous obtiendrons un bout de code qui répètera un bloc de code s'exécutant pour chaque pixel, de gauche à droite, de chaque ligne, de haut en bas.

![](img/td4-boucle-imbrique.gif)


### 7 · Lire le contenu de chaque pixel de l'image

Dans le bloc de code de cette double boucle imbriquée, nous obtenons 2 variables utilisables : `x` et `y` ! Ces deux variables correspondent aux coordonnées de chaque pixel de notre image, parcouru par la double boucle.

Nous allons pouvoir utiliser ces coordonnées pour récupérer la couleur (enfin, ici le niveau de gris) chaque pixel que nous allons convertir en ASCII.

Pour cela, nous utiliserons la méthode `.getpixel()`, qui :
* prend en unique **paramètre** une liste à 2 cases correspondant à chacune des coordonnées du pixel : `[x, y]`,
* **retourne** un nombre entier de `0` à `255` correspondant à l'"**intensité lumineuse**" du pixel.

Nous récupèrerons cette valeur d'intensité retournée que nous stockerons dans une variable `intensite_pixel`.

:::{admonition} Récupérer la valeur d'un pixel
:class: tip, dropdown
```python
intensite_pixel = mon_image.getpixel([x, y])
```
:::

### 8 · Créer un tableau servant de palette


### 9 · Trouver le caractère correspondant à l'intensité du pixel de l'image


### 10 · Ecrire ce caractère dans un fichier texte


### 11 · Ecrire un caractère de retour à la ligne à chaque changement de ligne lors de la lecture de l'image


### 12 · Ajoutez un redimensionnement de l'image d'entrée AVANT son parcours et sa conversion en ASCII


