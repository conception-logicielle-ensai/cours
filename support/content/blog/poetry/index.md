---
title: "Poetry : prise en main et intêret de l'outil"
weight: 13
draft: true
description: "Présentation de poetry, autre outil pour la gestion des dépendances d'un projet python"
summary: "Présentation de poetry, autre outil pour la gestion des dépendances d'un projet python"
slug: "prise-en-main-poetry"
tags: ["portabilite","packaging","poetry","cut"]
---
![](/images/gestionnaire-de-package/competing-standard.webp)

Cet article présente des notions sur poetry qui sont sorties du cours en 2026 (cours 2025).

Désormais, il est plutôt d'usage d'utiliser **UV** plutôt que **poetry**, donc le cours s'attelera plutôt sur UV. Mais poetry est encore dans quelques projets donc cela peut avoir encore du sens d'avoir accès a cette documentation.

## Parlons de Poetry

Projet récent, répondant a des besoins d'usage non couverts/ mal couverts par pip.

> Code source: https://github.com/python-poetry/poetry

Poetry intègre :
- les notions de versionning des fichiers via un lockfile `poetry.lock`
- un mécanisme de résolution de dépendances qui garantit la compatibilité entre toutes les versions installées.
- crée un lien entre packages et environnement virtuel où l'on execute, ce qui permet d'installer un environnement directement.

=> `poetry install` et `poetry shell`


### Installation 

Pour ce qui est du packaging, poetry est un facilitateur dans la mise en place de la configuration pour le déploiement de package. Tout est dans un fichier qui est le même que le fichier de versionning du projet, mais également tout s'effectue d'un seul coup et avec un seul outil.

Pour démarrer il faut l'installer
```sh
pip install poetry
```

et pour démarrer une config pour poetry : 
```sh
poetry init
```

Ensuite pour ajouter des package a un projet poetry :
```sh
poetry add requests
```

> (ici requests)

Poetry fonctionne avec un fichier `pyproject.toml` qui est bien plus complexe que le simple fichier `requirements.txt`. C'est plus propre et cela vous permet d'administrer en fonction des versions de python les installation et les environnements virtuels a construire dans le projet. 

Cela permet également une gestion des dépendances Dev dans le projet: Des paquets dév sont utiles en local (test, lint) mais pas pour le fonctionnement propre du code.

La configuration s'appuie sur un dépôt de sources python type : `./src/...`