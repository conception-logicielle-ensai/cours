---
title: "TP: Mise en place du projet"
weight: 15
draft: false
description: "Mise en place du projet des équipes constituées, application des concepts appris Git, Architecture et Packaging"
summary: "Mise en place du projet des équipes constituées, application des concepts appris Git, Architecture et Packaging"
slug: "tp-mise-en-place"
tags: ["portabilite","packaging","git"]
---

Ce TP n'a pas intention à être fini, il est plus une proposition de travail dans le temps associé

Ce TP regroupe des notions de Git, d'architecture applicative et de packaging.



> A partir d'ici nous vous préconisons de travailler en équipe.
## Partie 1 : Création du dépot projet
**Git**
- Créez un dépôt correspondant au dépôt du projet sur la forge de votre choix : github / gitlab
- Clonez ce dépôt en local
- Ajoutez des éléments permettant d'éviter de versionner les fichiers problématiques (voir le cours Git Avancé si vous ne voyez pas de quoi on parle.)

## Partie 2 : choix du mode de packaging
**Packaging**:
- Dans un dossier bien choisi (par exemple votre projet ensai), créez un environnement virtuel et activez le. (soit via `uv`, soit via d'autres moyens)
- Mettez en place une solution de packaging pour le projet, avez vous besoin de faire des requêtes http ? Installez Requests. Avez vous besoin d'accéder à des bases de données?
- Sauvegardez la configuration et documentez comment se créer un environnement de travail fonctionnel pour travailler sur votre projet

## Partie 3 - Premières décisions d'architecture
**Architecture**:
- Réflechissez au périmètre du projet que vous allez concevoir. (n'hésitez pas a nous poser des questions et a remplir le forms quand vous aurez fini)
- Distinguez des fonctionnalités de votre projet, ou réflechissez y. Mettez ces fonctionnalités dans des **tickets** ou **issues**. Vous pouvez ensuite créer une `feature-branche` par fonctionnalité.
- Réflechissez a l'organisation de vos packages : un dépot de sources python `src` ? Des sous modules par rapport aux couches applicatives ? `mvc` Ou une approche orientée fonctionnalitées `DDD`?

