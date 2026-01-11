---
title: "Idées de sujets proposés pour l'année 2026"
weight: 1
draft: false
description: "Sujets proposés aux élèves pour la réalisation des projets de conception logicielle 2026"
summary: "Sujets proposés aux élèves pour la réalisation des projets de conception logicielle 2026"
slug: "projets-exemples-2026"
tags: ["projet"]

---

## Propositions des sujets

Les sujets sont proposés ci dessous. Vous devez choisir un projet dans une groupe de 2 à 4 personnes (si possible 3 svp), les propositions personnelles sont valorisées et il faudra réaliser un envoi de votre groupe + sujet validé avant le 19 janvier 2026 17h ici : 

[https://forms.gle/i9H8tbQU78utnTZA9](https://forms.gle/i9H8tbQU78utnTZA9)


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
- https://conception-logicielle.abrunetti.fr/index.xml

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


### Wift : Plateforme collaborative de gestion et de recommandation de cadeaux

#### Contexte

Le projet consiste à concevoir et développer une application web permettant d’aider les utilisateurs à choisir, organiser et offrir des cadeaux de manière collaborative.

L’objectif est de répondre à plusieurs problématiques courantes :

- Trouver une idée de cadeau pertinente pour un proche
- Éviter les doublons lorsque plusieurs personnes participent à un même événement
- Gérer des cadeaux communs
- Respecter un budget et une date d’échéance

L’application devra permettre aux utilisateurs de créer des listes de cadeaux, d’y ajouter des idées issues de sites marchands, et de permettre à d’autres utilisateurs de participer à l’achat de ces cadeaux.

#### Fonctionnalités attendues

**Epic : Gestion des comptes utilisateurs**

- Création de compte utilisateur (inscription)
- Authentification par identifiant et mot de passe
- Gestion de profil utilisateur (informations de base)
- Accès sécurisé aux fonctionnalités de l’application

**Epic : Gestion des listes de cadeaux**

- Création de listes de cadeaux personnalisées
  Exemples :
  - Anniversaire
  - Noël
  - Saint-Valentin
  - Fête des mères
- Définition d’une date d’échéance pour chaque liste
- Gestion de la visibilité des listes (accès aux amis uniquement)

**Epic : Ajout et gestion des idées de cadeaux**

- Ajout d’un cadeau à une liste via un lien URL
- Analyse automatique de la page fournie (web scraping)
- Extraction et stockage a minima des informations suivantes :
  - Nom de l’objet
  - Prix
  - Image principale
  - Lien vers la page source
- Limitation volontaire du périmètre aux sites marchands pris en charge
  (exemples : Amazon, Fnac, Zara, etc.)
- Gestion de l’état du cadeau : disponible / réservé

**Epic : Participation aux cadeaux**

- Consultation des listes de cadeaux des amis
- Visualisation des idées de cadeaux avec estimation de prix
- Possibilité de réserver un cadeau
- Verrouillage du cadeau réservé pour éviter les doublons
  (le propriétaire de la liste ne voit pas les réservations)
- Après la date d’échéance :
  - Le propriétaire de la liste peut consulter les cadeaux reçus
  - Visualisation de l’identité des participants

#### Fonctionnalités avancées

**Epic : Optimisation du prix des cadeaux**

- Recherche automatique d’offres alternatives pour un même produit
- Comparaison des prix entre plusieurs sites marchands
- Proposition d’options moins chères ou équivalentes

**Epic : Gestion des cadeaux communs**

- Possibilité de participer à un cadeau à plusieurs
- Définition d’un montant cible
- Suivi des contributions individuelles
- Notifications aux participants (avancement, échéance proche, objectif atteint)

**Epic : Notifications et communication**

- Notifications de rappel avant une date d’échéance
- Notifications lors de la réservation ou participation à un cadeau commun
- Alertes en cas de baisse de prix ou d’indisponibilité d’un produit


### Outfision : Application de gestion de dressing et de recommandation de tenues


#### Contexte

Le projet consiste à concevoir et développer une application web permettant aux utilisateurs de gérer numériquement leur garde-robe et d’obtenir des recommandations de tenues adaptées à leur usage réel.

L’application vise à répondre à plusieurs problématiques courantes :

- Visualiser l’ensemble de ses vêtements et accessoires sans devoir sortir physiquement son dressing
- Identifier les vêtements peu ou jamais portés
- Composer des tenues adaptées sans effort de réflexion
- Optimiser l’usage de sa garde-robe existante

L’application devra permettre l’ajout de vêtements à partir d’images, leur catégorisation via des tags, et proposer des recommandations de tenues selon différents critères (saison, événement, météo, etc.).

#### Fonctionnalités attendues

**Epic : Gestion des comptes utilisateurs**

- Création de compte utilisateur (inscription)
- Authentification par identifiant et mot de passe
- Gestion de profil utilisateur (informations de base)
- Accès sécurisé aux fonctionnalités de l’application

**Epic : Gestion de la garde-robe**

- Ajout d’un vêtement à la garde-robe à partir d’une image
- Association d’un vêtement à plusieurs tags de classification :
  - Saison (été, hiver, mi-saison, etc.)
  - Type de vêtement (robe, pantalon, t-shirt, veste, chaussures, accessoires, etc.)
  - Couleur
  - Type d’événement (professionnel, quotidien, plage, soirée, mariage, etc.)
- Possibilité de modifier ou supprimer un vêtement existant

**Epic : Visualisation de la garde-robe**

- Consultation de l’ensemble de la garde-robe
- Consultation par catégories (type, saison, couleur, événement)
- Visualisation détaillée d’un vêtement (image, tags, historique d’utilisation)

**Epic : Recommandation de tenues**

- Recommandation automatique de tenues en fonction :
  - de la saison
  - du type d’événement
- Composition de tenues cohérentes à partir des tags
  (ex. : robe d’été ou combinaison t-shirt + pantalon)
- Possibilité d’afficher plusieurs propositions alternatives

**Epic : Suivi de l’utilisation des vêtements**

- Enregistrement de l’utilisation d’un vêtement
- Visualisation du nombre de fois où un vêtement a été porté
- Analyse sur une période définie par l’utilisateur
  (ex. : dernier mois, dernière année, période personnalisée)
- Mise en évidence des vêtements rarement ou jamais utilisés


#### Fonctionnalités avancées

**Epic : Recommandation des vêtements à vendre**

- Identification automatique des vêtements peu utilisés
- Définition d’un seuil de non-utilisation (paramétrable)
- Proposition de vêtements à revendre ou à donner
- Préparation à une éventuelle intégration avec des plateformes de revente

**Epic : Planification des tenues**

- Planification de tenues dans un calendrier
- Association d’une tenue à une date donnée
- Recommandation automatique de tenue pour une journée donnée
- Prise en compte de la météo :
  - Température
  - Pluie
  - Conditions climatiques


### Autres projets : votre projet

Vous pouvez évidemment proposer un sujet, sous réserve de validation. Pour maximiser les chances, il faut que votre projet utilise une ressource externe.

**⚠️⚠️ Il faudra faire valider vos projets par le corps enseignant ⚠️⚠️**

<a href="mailto:antoine.brunetti@insee.fr,oriane.foussard@insee.fr
?subject=Projet%20conception%20logicielle%202026
&body=Bonjour%2C%0A%0AJe%20me%20permets%20de%20vous%20contacter%20au%20sujet%20du%20projet%20de%20conception%20logicielle%202026.%0A%0ACordialement%2C">
Envoyer un mail a antoine.brunetti@insee.fr et oriane.foussard@insee.fr
</a>

Pour appuyer la création de vos projets vous avez un ensemble d'API a disposition disponibles ici : 


Exemples : 
- Application pour évaluation de la qualité des aliments type **yuka** avec openfood facts
- Application pour le partage de données de localisation type **poop map**
- Application pour le parcours d'annonces ou la reservation : **leboncoin**, comparateur **skyscanner**..
- Application pour vos jeux : récupération de statistiques sur les autres joueurs etc..
- Plugins pour la reservation : **courses a pied**, **événements**

### Aide: Utilisation d'API au service de votre projet

Une myriade d'API existent avec ou sans authentification : https://github.com/public-apis/public-apis

#### Exemple pour un projet Cinéma - utilisation de https://imdbapi.dev/.
Vous pourrez retrouver les films et des métadonnées

```sh
curl -X 'GET' \
  'https://api.imdbapi.dev/search/titles?query=rubber&limit=1' \
  -H 'accept: application/json'
```

Un **swagger** est disponible ici : https://imdbapi.dev/


