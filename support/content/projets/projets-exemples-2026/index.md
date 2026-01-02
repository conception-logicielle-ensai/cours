---
title: "Idées de sujets proposés pour l'année 2026"
weight: 1
draft: false
description: ""
slug: "projets-exemples-2026"
tags: ["projet"]

---
## Propositions des sujets

Les sujets sont proposés ci dessous. Vous devez choisir un projet dans une groupe de 2 à 4 personnes (si possible 3 svp), les propositions personnelles sont valorisées et il faudra réaliser un envoi de votre groupe + sujet validé avant le 19 janvier 17h ici : 

[Lien du google forms](https://docs.google.com/forms/d/e/1FAIpQLSfe9r67gWk1Q9UxizWX96fpOTzzQUAkNXw56OoLoyBiEpNNFQ/viewform?pli=1)

**L'idée générale est de vous proposer des sujets, vous êtes ensuite libres de prioriser les développement et n'êtes pas obligés de suivre la trame précisée**

**Par contre il faudra indiquer quelque part dans votre dépôt le nom du projet, les fonctionnalités prévues, les fonctionnalités réalisées**


## Spécifications des projets

La spécification des projets se distingue en fonctionnalités attendues et fonctionnalités avancées.

L'idée est de penser les logiciels comme des produits évolutifs : 
- Il faut pouvoir atteindre un socle de **fonctionnalités attendues** qui correspond a un produit qui rend le service
- Puis une fois ces fonctionnalités atteintes (on peut éventuellement en enlever si c'est légitime), on peut améliorer le service en investissant sur les **fonctionnalités avancées**.

### RSSpresso: Aggrégateur de flux RSS

#### Contexte
Le projet consiste à concevoir et développer une application d’agrégation de flux RSS.

> Un flux RSS (Really Simple Syndication) est un format standardisé de diffusion de contenus utilisé par de nombreux sites web pour partager automatiquement leurs mises à jour (articles, actualités, billets de blog, podcasts, etc.).

L’application devra être capable de :

- Collecter automatiquement des contenus provenant de plusieurs sources RSS

- Centraliser ces contenus dans une base de données

- Les rendre consultables via une interface

- Préparer le terrain pour des traitements avancés (analyse de texte, extraction d’informations, etc.)

#### Fonctionnalités attendues

**Epic: Gestion des flux RSS**
- Possibilité de configurer une liste de flux RSS
- Récupération périodique des flux
- Parsing des flux RSS

Exemples de flux : 

- https://www.lemonde.fr/rss/une.xml
- https://www.francetvinfo.fr/titres.rss
- https://www.sciencedaily.com/rss/computers_math/computer_science.xml

**Epic: Accès au service**
- Mise en place d'une interface accessible via navigateur et mise en place l'authentification par mot de passe d'un administrateur à la plateforme

**Epic: Construire une base de données personnalisée**
- Création d'un modèle de données adapté aux articles récupérés : source, titre, contenu, date, lien vers la source
- Détection des doublons et contraintes pour éviter doublons.

**Epic: Consultation d'articles récupérés**
- Liste chronologique des articles agrégés
- Filtrage par source
- Recherche simple par mots-clés

#### Fonctionnalités avancées

**Epic: Effectuer des résumés des articles via accès aux sources**
- Webscraping des sources
- Traitement soit NLP, soit IA générative sur les données récupérées pour résumer / récupérer les informations principales 

**Epic: Aggréger des informations de différentes sources**
- Regroupement d'articles similaires et labelisation
- Détection de tendances (exemple : RSS sécurité, détection d'une attaque en aggrégeant les sources fiable)

**Epic: Améliorer les interfaces**
- Créer une plateforme pour plusieurs utilisateurs
- Permettre de définir des champs particuliers a suivre (pour chaque utilisateur)

**Epic: Créer une API qui donne les données aggrégées**
- Récupération des données des articles, par date, contenu, categorie => mise a disposition via un endpoint simple


### Autres projets : votre projet

Vous pouvez évidemment proposer un sujet, sous réserve de validation. Pour maximiser les chances, il faut que votre projet utilise une ressource externe.

**⚠️⚠️ Il faudra faire valider vos projets par le corps enseignant ⚠️⚠️**

<a href="mailto:antoine.brunetti@insee.fr,oriane.foussard@insee.fr
?subject=Projet%20conception%20logicielle%202026
&body=Bonjour%2C%0A%0AJe%20me%20permets%20de%20vous%20contacter%20au%20sujet%20du%20projet%20de%20conception%20logicielle%202026.%0A%0ACordialement%2C">
Envoyer un mail a antoine.brunetti@insee.fr et oriane.foussard@insee.fr
</a>



Exemples : 
- Application pour évaluation de la qualité des aliments type **yuka** avec openfood facts
- Application pour le partage de données de localisation type **poop map**
- Application pour le parcours d'annonces ou la reservation : **leboncoin**, comparateur **skyscanner**..
- Application pour vos jeux : récupération de statistiques sur les autres joueurs etc..
- Plugins pour la reservation : **courses a pied**, **événements**
