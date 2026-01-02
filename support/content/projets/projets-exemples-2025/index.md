---
title: "Idées de sujets proposés pour l'année 2025  "
weight: 1
draft: false
description: ""
slug: "projets-exemples-2025"
tags: ["specification-projet-notation", "about", "introduction"]
---
## Propositions des sujets

### Buzzify : l'application qui vous permet de buzzer

#### Contexte

Dans de nombreuses activités collaboratives (quiz, compétitions, brainstorming), un système de buzzer est souvent utilisé pour signaler rapidement une action, une réponse ou une idée. Ce projet vise à recréer un tel système sous forme d’une application connectée, en exploitant les technologies modernes comme les websockets pour assurer une interaction en temps réel.

#### Fonctionnalités attendues
- Créer une interface web accessible par plusieurs personnes
- Permettre a chaque joueur d'actionner le buzzer par cette interface
- Diffuser l'action buzzer sur tous les clients connectés lorsque la personne appuie

#### Fonctionnalités avancées

- Proposer une authentification pour la "room" ou a défaut générer un identifiant de session permettant de se distinguer des autres

### Bookit: l'application qui permet la reservation de services

#### Contexte

Que ce soit dans une école, une entreprise, ou un centre communautaire, la gestion des ressources comme les salles, les véhicules ou les équipements peut devenir complexe. Cette application permettrait de centraliser cette gestion, de visualiser les disponibilités des ressources, et de permettre aux utilisateurs de faire des réservations facilement.

#### Fonctionnalités attendues
- Permettre de définir 3 granularités d'utilisateurs : anonyme, consommateur, administrateur
- Les administrateurs peuvent créer des "événements" qui pourront être :
  - Des évenements pour la réservations de bus (pour les événements / soirées de l'école : ces événéments peuvent avoir une infinité de participants)
  - Des événements pour la réservation de salles (musique, ludik ...) (ces événements n'ont qu'une réservation)
  - (autres propositions de votre part possibles)
- Lors de la création d'un événement, l'application notifiera, soit au travers d'un bot twitter, soit au travers d'un envoi de mail groupé a différentes personnes (la liste des utilisateurs) la disponibilité de l'événement.
- Les utilisateurs pourront via l'interface développer, s'inscrire aux événements.

#### Fonctionnalités avancées
- Intégration avec google agenda
- Qualité de l'interface utilisateur

### MétéoPredict: l'application qui prédit la température 

#### Contexte
L'objectif de cette application est de prédire la météo d'un jour spécifique (par exemple, température, précipitations, humidité) en s'appuyant sur les données météorologiques historiques. Cette application utilisera des outils statistiques pour réaliser cela et s'appuiera sur des données **météo france**

#### Fonctionnalités attendues
- Permettre de prédire la température qu'il fera le lendemain a partir d'un aggrégat de données récupérées sur les années précédentes. (voir les fichiers météo france)
- Proposer un tableau comparant la prédiction a des valeurs connues (même jour l'année précédente, valeur réelle (a récupérer via API) du jour précédent). Effectuer une interface "jolie" adaptée.
- Déterminer la géolocalisation de l'utilisateur a la requête.
- On utilisera d'abord les données depuis https://meteo.data.gouv.fr/ , on pourra passer en appel API de manière avancée.
#### Fonctionnalités avancées
- Choix d'un algorithme pour la prédiction : (régression linéaire (par défaut), Arima, Réseau de neurones récurrents, random forest... ) 
- Multiplication des jeux de données

### Autres projets : votre projet

Vous pouvez évidemment proposer un sujet, sous réserve de validation. Pour maximiser les chances, il faut que votre projet utilise une ressource externe.

Exemples : 
- Application pour évaluation de la qualité des aliments type **yuka** avec openfood facts
- Application pour le partage de données de localisation type **poop map**
- Application pour le parcours d'annonces ou la reservation : **leboncoin**, comparateur **skyscanner**..
- Application pour vos jeux : récupération de statistiques sur les autres joueurs etc..
- Plugins pour la reservation : **courses a pied**, **événements**
