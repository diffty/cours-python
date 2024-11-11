# TD5: Interface graphique

![](./img/111962.gif)

## Intro

Dans ce TD, nous allons reprendre le convertisseur d'image en ASCII art et lui créer une petite interface afin d'en faire un outil pleinement fonctionnel !


## Partie 1

### PySide

PySide est un des modules Python de création d'interface les plus utilisés (notamment en prod).

Il est basé sur la très puissante librarie C++ Qt, dont l'ensemble des fonctionnalités seront utilisable dans nos programmes Python afin de faire aussi bien des petits outils avec interface graphique, que des grosses applications complètes, et ce sans avoir à se fader un langage aussi complexe que le C++.

De plus, l'avantage majeur de cette librarie est qu'elle est **multiplateforme**, c'est-à-dire qu'un code écrit avec pourra être utilisé **indifféremment sur tous les systèmes d'exploitation**, que ce soit Windows, Linux ou macOS.


### Installer

Comme pour `pillow` dans le [TD ASCII](./td-ascii.md), ouvrez une Invite de Commande et utilisez `pip` pour [installer le module](./td-ascii.md#2--le-module-pillow) nommé `PySide6`.


### "Hello World" ou création d'une fenêtre basique

Un *Hello World* est le programme minimum que l'on peut faire pour tester un langage ou une librarie.
Ici, on va juste faire une petite fenêtre pour afficher un texte.


#### a) Importer `QApplication` et `QLabel`

👉 Importez les classes `QApplication` et `QLabel` depuis le module `PySide2.QtWidgets`.

:::{admonition} Importer certains éléments particuliers d'un module
:class: tip, dropdown
Pour importer un élément particulier se trouvant dans un module, par exemple ici, la classe `QApplication` du module `PySide6.QtWidgets`, on peut faire :

```python
import PySide6.QtWidgets
app = PySide6.QtWidgets.QApplication()
```

Mais on peut faire aussi :

```python
from PySide6.QtWidgets import QApplication
app = QApplication()
```

Cette dernière version est plus courte, et évite de devoir se retaper toute la hiérarchie de modules et sous-modules à chaque utilisation d'un élément présent dans un module/librairie.
:::


#### b) Instancier un objet `QApplication`

Créer une [[Cours Python#Objet|instance]] de la [[Cours Python#Classe|classe]] `QApplication` et la stocker dans une variable nommée `app`.

:::{admonition} Instancier une classe / Créer un objet à partir d'une classe
:class: tip, dropdown
Pour instancier une classe, la syntaxe est la même que lorsque l'on appelle une fonction : on nomme la classe et on fait suivre son nom de `()`. L'instruction retournera notre objet instancié depuis la classe, qu'on pourra donc stocker dans une variable.

```python
app = QApplication()
```
:::


#### c) Créer un widget `QLabel`

:::{admonition} Qu'est-ce qu'un Widget?
:class: info
C'est un élément composant une interface, un bouton, une case à cocher, un champ de texte, une image, un texte...
 
Chaque Widget correspond à une classe de PySide/Qt.
 
![](./img/parent-child-widgets.png)
:::

Pour créer un widget, on instancie la [[Cours Python#Classes & Objets|classe]] correspondante au widget que l'on désire afficher.
Ici, nous souhaiterons commencer par afficher un widget qui ne sert qu'à afficher du texte : `QLabel`.

Ce widget pourra afficher le texte de votre choix, en lui donnant en paramètre une chaîne de caractère avec le texte en question !

Une fois instancié, vous pouvez l'afficher en utilisant sa méthode `.show()`.

```python
label = QLabel(text="Coucou.")
label.show()
```

:::{note}
Le premier widget créé à l'extérieur d'une hiérarchie de widgets deviendra la fenêtre principale de l'application. Nous verrons en [[#PARTIE 2|partie 2]] comment créer des interface plus complexes en disposant des widgets dans une fenêtre :)
:::


#### d) Exécuter l'interface graphique

Une fois l'interface définie, on peut démarrer notre application graphique en appelant la méthode  `exec()` de notre instance de `QApplication`.

```python
app.exec()
```

:::{warning}
Cette ligne doit toujours se trouver A LA FIN du programme.
:::

:::{admonition} Que fait `exec()` ?
:class: info, dropdown

Cette méthode démarre la boucle d'événements de notre programme, une boucle infinie qui va bloquer l'exécution de la suite du programme mais qui va se charger de récupérer en permanence les évènements, intéractions avec l'utilisateur, l'interface et l'OS, changements d'états du programme, etc.
:::

![](./img/Pasted-image-20231204014511.png)


## Partie 2

### Dessin de l'interface

![](./img/td-ui-app-wireframe1.png)


### Réutilisation du code du TD4


### Évènements



## Partie 3

### Packaging en .exe


## La suite?

* File picker
* 