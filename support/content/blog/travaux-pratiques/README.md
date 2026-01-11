---
title: "TP2: Mise en place du projet"
weight: 14
draft: false
description: ""
summary: "Gestion de packages : éléments supprimés ressources tierces"
slug: "poetry"
tags: ["portabilite","packaging","git"]
---

## Partie 1 : Portabilité, Architecture
Ce TP regroupe des notions de Git, d'architecture applicative et de packaging.


- Si vous ne l'avez pas fait, installez le package `helloensai` avec pip et lancez le via une commande python.
- Consultez les projets exemples : [1](https://github.com/conception-logicielle-ensai/archi-exemple) , [2](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/gestionnaire-de-package/publish), [3](https://github.com/conception-logicielle-ensai/predicat). Répondez aux questions suivantes : est ce que le projet détaille comment arriver sur le projet? quel est le gestionnaire de paquet privilégié pour le projet ?



**Git**
- Créez un dépôt correspondant au dépôt du projet sur la forge de votre choix : github / gitlab
- Clonez ce dépôt en local
- Ajoutez des éléments permettant d'éviter de versionner les fichiers problématiques (voir le cours Git Avancé si vous ne voyez pas de quoi on parle.)

**Packaging**:
- Dans un dossier bien choisi (par exemple votre projet ensai), créez un environnement virtuel et activez le. (soit via `uv`, soit via d'autres moyens)
- Mettez en place une solution de packaging pour le projet, avez vous besoin de faire des requêtes http ? Installez Requests. Avez vous besoin d'accéder à des bases de données?
- Sauvegardez la configuration et documentez comment se créer un environnement de travail fonctionnel pour travailler sur votre projet

**Architecture**:
- Réflechissez au périmètre du projet que vous allez concevoir. (n'hésitez pas a nous poser des questions et a remplir le forms quand vous aurez fini)
- Distinguez des fonctionnalités de votre projet, ou réflechissez y. Mettez ces fonctionnalités dans des **tickets** ou **issues**. Vous pouvez ensuite créer une `feature-branche` par fonctionnalité.
- Réflechissez a l'organisation de vos packages : un dépot de sources python `src` ? Des sous modules par rapport aux couches applicatives ? `mvc` Ou une approche orientée fonctionnalitées `DDD`?

