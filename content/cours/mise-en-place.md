
+++
title = "Mise en place"
description = "Section sur le contour du cours"
date = 2024-01-15T00:00:02
aliases = ["mise-en-place"]
short = true
+++

# Pré-requis

Liste des outils qui seront utilisés pendant ce cours.  
Il est demandé de les avoir pré-installés et testés (des exemples de tests sont proposés dans les blocs de code) avant le début du cours.  
En cas de question / problèmes, n'hésitez pas à venir demander de l'aide sur teams.

## Système d'exploitation

Le système d'exploitation n'est pas imposé, les mises en pratique se feront principalement en Python, qui est compatible avec les OS principaux :

- Linux
- Windows
- Mac OS

Les versions doivent être "relativement" récentes.

## Git

Tout le code que l'on va écrire sera versionné sur Git et il vous sera régulièrement demandé de récupérer / partager du code via Git.  
La première séance de cours sera dédiée à des rappels et approfondissements sur Git.  
Pour Windows, on pourra utiliser https://gitforwindows.org/.

- Pour vérifier que tout est bien installé, lancez un terminal et lancez la commande `git status`
## IDE

Bien que vous soyez fortement incités à tester et utiliser différents IDE, il est demandé d'installer [Visual Studio Code](https://code.visualstudio.com/) qui propose des plugins pour l'ensemble des langages et outils que nous allons découvrir ensemble.  
Les plugins suivants sont conseillés :

- Python `ms-python.python`
- YAML `redhat.vscode-yaml`
- GitLens — Git supercharged `eamodio.gitlens`

## Python 3

Le langage principal d'application sera `python`, il faut donc l'installer.  
Faites bien attention à installer la version 3 et non pas Python 2.  
Pour Windows, l'installation peut se faire depuis le store d'application. Le plus simple pour y accéder : taper python dans une invite de commande windows.

Pour vérifier l'installation, il faut python et pip installés :

```
python --version
pip --version
```

Les deux doivent mentionner une version 3.X de python (préférablement 3.11 ou 3.12).

A noter : sur certains systèmes, il y a cohabitation de python 2 et python 3. Dans ce cas, l'appel à python 3 et à pip se font respectivement via `python3` et `pip3`.

> Lien vers l'install : https://www.python.org/downloads/
## SSPCloud

Un des objectifs de ce cours est de vous sensibiliser aux nouvelles pratiques et technologies de développement informatique et statistique.  
Dans ce cadre, on vous met à disposition un compte sur la plateforme de datascience SSPCloud (SSP = Service statistique public) portée par la division innovation de l'Insee.  
On parle de `Datalab`, laboratoire de la donnée. C'est à dire, une plateforme vous proposant différents services allant du stockage de données à des environnements de développement et d'exécution.  
Le datalab est accessible sur [https://datalab.sspcloud.fr](https://datalab.sspcloud.fr).

Pour accéder a la plateforme, il vous faut créer un compte par l'onglet dédié : 
- Accéder au site : https://datalab.sspcloud.fr/
- Aller sur l'onglet **connexion**
- En bas de ce site vous allez pouvoir créer un compte
  
Techniquement, vous n'avez rien à installer, tout se fait via votre navigateur.

On utilisera ces environnements dans différents cours ou selon votre pc perso.