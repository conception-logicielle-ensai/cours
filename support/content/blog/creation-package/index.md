---
title: "Création d'un package"
weight: 15
draft: false
description: "Gestion de packages : éléments supprimés ressources tierces"
summary: "Gestion de packages : éléments supprimés ressources tierces"
slug: "creation-package-poetry"
tags: ["portabilite","packaging","poetry", "legacy"]
---

Les gestionnaires de paquets permettent a la fois d'utiliser des paquets existants, mais vous le devinez bien, il est possible d'en créer vous même.


## Création d'un package avec UV

## Creation d'un package avec poetry


Les dépots de packets peuvent être :

- privés - c'est le cas dans les entreprises en général
- publics :
  - c'est le cas du dépôt pypi https://pypi.org/ , vers lequel pointe par défaut une installation de pip.
  - mais également du dépôt de test : https://test.pypi.org/

L'idée de la création d'un package est de créer une brique réutilisable de composants fonctionnels.

Cela peut être par exemple la réutilisation de classes entre différents projets ou la sauvegarde d'un sous ensemble de fonctions utiles que vous aimez utiliser sur les différents projets sur lesquels vous travaillez.

Un package en python a cette forme :

```
projet
├── LICENSE
├── src/
│   └── package
│       ├── __init__.py
│       └── t.py
├──  tests/
├── README.md
└── pyproject.toml
```

A la racine du package se trouve :

- un fichier pyproject.toml, qui contient des métadonnées sur la version, le nom de l'application, etc..
  => Il s'agit d'un fichier python qui permet ensuite de construire un "livrable" au sens de python, prêt a être envoyé.
- Un fichier LICENSE pour préciser le mode d'usage et de partage du package
- un fichier README pour décrire l'usage

Puis il faut, dans cet ordre :

- construire le livrable (avec un outil comme `buildtools`)
- envoyer le livrable (avec un outil comme `twine`)


<div class="alert alert-info">
  <strong>Pour aller plus loin</strong> <br/> 
Comment packager un projet ? : <br/><a href="https://packaging.python.org/en/latest/tutorials/packaging-projects/">https://packaging.python.org/en/latest/tutorials/packaging-projects/</a>
</div>


## Travaux Pratiques

L'objectif de ce TP est de manipuler les éléments de packaging.
En effet, pour assurer la portabilité applicative, on sera amené a **canoniser** l'environnement d'exécution de nos applicatifs.

**1 ) Modules python**
- Si vous ne l'avez pas fait, installez le package `helloensai` avec pip et lancez le via une commande python.
- Consultez les projets exemples : [1](https://github.com/conception-logicielle-ensai/archi-exemple) , [2](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-3/gestionnaire-de-package/poetry-publish), [3](https://github.com/conception-logicielle-ensai/predicat). Répondez aux questions suivantes : est ce que le projet détaille comment arriver sur le projet? quel est le gestionnaire de paquet privilégié pour le projet ?

**2 ) Portabilité applicative**
- Dans un dossier bien choisi (par exemple votre projet ensai), créez un environnement virtuel et activez le.
- Veillez a éviter que le venv soit non versionné.
- Réfléchissez aux dépendances éventuelles pour former un fichier `requirements.txt` ou un fichier `pyproject.toml`

