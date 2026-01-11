---
title: "Création d'un package"
weight: 16
draft: true
description: "Création d'un package avec pip, poetry et uv"
summary: "Création d'un package avec pip, poetry et uv"
slug: "creation-package-poetry"
tags: ["portabilite","packaging","poetry", "legacy"]
---

## Création d'un package classique 

Pour créer un package sans utilitaire dédié, il est nécessaire d'installer `buildtools` et `twine`.

Cela peut être assez lourd et vous pouvez suivre un tutoriel de ce type :  https://packaging.python.org/en/latest/tutorials/packaging-projects/


**Ce qui est attendu:**

A la racine du package se trouve :

- un fichier pyproject.toml, qui contient des métadonnées sur la version, le nom de l'application, etc..
  => Il s'agit d'un fichier python qui permet ensuite de construire un "livrable" au sens de python, prêt a être envoyé.
- Un fichier LICENSE pour préciser le mode d'usage et de partage du package
- un fichier README pour décrire l'usage

Puis il faut, dans cet ordre :

- construire le livrable (avec un outil comme `buildtools`)
- envoyer le livrable (avec un outil comme `twine`)


## Création d'un package avec UV

Suivre la doc ici : https://docs.astral.sh/uv/guides/package/

## Creation d'un package avec poetry

Suivre la doc ici : https://www.geeksforgeeks.org/python/how-to-build-and-publish-python-packages-with-poetry/

