---
title: "Aide a l'organisation du code projet"
weight: 10
draft: false
description: "Aide a la prise en main du poste linux et environnements d'execution contextuel au cours"
summary: "Aide a la prise en main du poste linux et environnements d'execution contextuel au cours"
slug: "prise-en-main-linux"
tags: ["ubuntu", "linux", "démarrage"]
---

Ce module vous présente des propositions d'organisation du code en modules cohérents.

L'architecture multi-couches permet de séparer les différentes responsabilités d'une application en plusieurs couches. On parle aussi d'architecture **n** tiers. Dans le modèle classique client-serveur, nous avons trois couches principales :

- Couche Client (ou présentation) : Interagit avec l'utilisateur, collecte ses données, et affiche les résultats.
- Couche Serveur (ou logique métier) : Traite les données envoyées par le client, applique la logique métier, et communique avec la base de données. Ici on y retrouve par exemple dans le modèle MVC, `business_object` et `services`. Pour une application web c'est la même chose.
- Couche Base de Données (ou stockage des données) : Stocke et gère les données de l'application, et répond aux requêtes envoyées par le serveur.

Il faudra adapter votre architecture en ce sens.

### Python - fastapi/flask
- Un dossier pour l'application : `app`
- Un sous dossier de app pour chaque couche applicative : `router/controller` `dao` `business_object` ...
```bash
/app
/app/main.py #(ou autre mais point d'entrée)
/app/requirements.txt #(ou poetry)
/app/README.md #(documentation de l'app avec comment la lancer)
/app/business_object/ #(modèle de données)
/app/dao/ #(accès au données)
/app/controller/ #(ou controller c'est selon)
/app/service/ #(classes et méthodes de service)
```

### Python - django

```bash
/app
/app/manage.py
/app/README.md
/app/config/ # application principale (exemple mysite)
/app/config/settings.py
/app/config/urls.py
/app/config/...
/app/requirements.txt
/app/.env/
/app/apps/monappli/ # django gère sur un même serveur plusieurs applis
/app/apps/monappli/models.py # Modèles de données (ORM)
/app/apps/monappli/dao.py # Accès aux données (Data Access Layer)
/app/apps/monappli/services.py # Logique métier
/app/apps/monappli/controllers.py   # Gestion des requêtes HTTP
/app/apps/monappli/urls.py          # Routing de l’application
/app/apps/monappli/views.py         # Vues Django classiques
/app/static/                  # Fichiers statiques (CSS, JS, images)
/app/static/css/
/app/static/js/
/app/static/images/
```

### IHM (voir cours 4-5)
- Suivre l'architecture proposée par **vite** pour les application **React** :

```bash
site/
site/package.json
site/package-lock.json
site/src/main.jsx # composant démarrant l'application
site/src/App.jsx # Composant principal de l'application.  
site/src/assets/ # Fichiers statiques comme les styles globaux, images, polices, etc.  
site/src/components/ # Composants UI réutilisables.  
site/src/pages/ #Pages principales de l'application.  
site/src/services/ # Fonctions liées aux appels API et logique métier. api, web, .. convient.
site/src/utils/ # Fonctions utilitaires globales.  
site/public/ # Contient les fichiers accessibles directement dans le navigateur (ex: `favicon.ico`).  
```

- Site statique simple : séparer html, css, js et documentation propre au site

```bash
/mon-site
/mon-site/index.html # (page principale) 
/mon-site/about.html # (autres pages du site)
/mon-site/assets/ # (ressources) 
/mon-site/assets/css/ # (fichiers CSS)
/mon-site/assets/css/styles.css 
/mon-site/assets/js/ # (scripts JavaScript) 
/mon-site/assets/images/ # (images et icônes)
/mon-site/README.md # (documentation du sous projet site)
/mon-site/package.json # (si utilisation de npm)
```