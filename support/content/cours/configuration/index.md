---
title: "Configuration du code en fonction de l'environnement"
weight: 40
draft: false
description: "Comprendre comment gérer la configuration d’une application de manière propre, sécurisée et maintenable, indépendamment du code source"
summary: "Gestion de la configuration et des variables d’environnement en développement logiciel"
slug: "gestion-configuration"
tags: ["configuration", "environnement", "conception"]
series: ["Cours"]
series_order: 1

---

Lorsqu’on exécute un programme, il est souvent nécessaire de le configurer, de le paramétrer ou de lui associer des éléments spécifiques à l’environnement depuis lequel il est lancé. Par exemple, il peut s’agir de travailler avec des données stockées dans un répertoire particulier ou d’injecter des paramètres de connexion à une base de données que l’on souhaite garder confidentiels et non versionnés.

Heureusement, il est possible d’injecter dans un programme des variables externes, qui serviront à configurer son comportement tout en restant indépendantes du code source.

## Pourquoi externaliser la configuration d’un programme ?

Pour garantir la flexibilité et la sécurité d’une application, il est souvent judicieux d’externaliser certaines configurations. Voici quelques exemples courants :  

- **Éléments dépendants de l’environnement cible** : Par exemple, l’URL d’une base de données, un lien vers un jeu de données dans un datalake, le chemin d’un fichier externe, ou encore une règle métier configurable comme un seuil d’erreur.
- **Accès sécurisé à des ressources** : Les mots de passe ou clés d’authentification tel que les clés d'API, ne doivent pas être versionnés dans le code source pour éviter tout risque de fuite.  


La configuration d’une application regroupe **tout ce qui est susceptible de varier entre différents environnements** (développement, validation, production, etc.). Une bonne pratique consiste à séparer strictement la configuration du code. Tandis que le code reste inchangé à travers les déploiements, la configuration peut varier considérablement.  

Un bon test pour s’assurer de cette séparation consiste à se demander si l’application pourrait être rendue open source à tout moment, sans risquer de compromettre des identifiants ou d’autres données sensibles.

Cela peut être mis en œuvre de plusieurs façons :  
- Par la lecture de variables d’environnement.  
- Par l’injection de paramètres lors du lancement du programme.  
- Par l’ajout de fichiers de configuration situés à un emplacement connu et attendu par le programme.  

Ces trois techniques sont différentes interfaces pour **fournir des données au programme**, mais elles servent toutes le même objectif : injecter des informations externes dans le code.  


**Ce qui reste le plus préconisé, c'est l'utilisation de variable d'environnement dans l'applicatif (cf https://12factor.net)**

Toutefois, les différentes méthodes d'injection existent et sont plus ou moins maintenues selon le language.



## Configuration par fichier de configuration externe

Dans de nombreux langages, il existe des formats spécifiques pour gérer la configuration via des fichiers.  

En Python, les fichiers de configuration sont couramment utilisés, souvent dans des formats standardisés, parmi lesquels le format **TOML** (Tom’s Obvious, Minimal Language) et le format **INI**.  

#### Exemple de fichier INI

```ini
# config.ini à la racine du projet
[section_name]
key1 = value1
key2 = value2

[another_section]
keyA = valueA
keyB = valueB

# Exemple avec des paramètres par défaut
[DEFAULT]
ServerAliveInterval = 45
Compression = yes
CompressionLevel = 9
ForwardX11 = yes

[forge.example]
User = hg

[topsecret.server.example]
Port = 50022
ForwardX11 = no
```

Dans la bibliothèque standard de Python, on trouve le module **`configparser`**, qui permet de gérer des fichiers de configuration au format **INI**. Ces fichiers sont habituellement nommés avec une extension descriptive comme `.ini`, et ce module permet également de les lire facilement :  

```python
import configparser

# Création d'un objet ConfigParser
config = configparser.ConfigParser()

# Lecture du fichier config.ini
config.read('config.ini')

# Exemple d’accès aux données
server_interval = config['DEFAULT']['ServerAliveInterval']
user = config['forge.example']['User']
print(server_interval)  # Affiche : 45
print(user)             # Affiche : hg
```

<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> <br/>Feuilleter la documentation <a href="https://docs.python.org/3/library/configparser.html">https://docs.python.org/3/library/configparser.html</a>
</div>


> **Note** :  
> Ce type de configuration est présent dans pratiquement tous les langages, bien que chaque langage ait ses conventions. Par exemple :  
> - En Java : `application.properties` ou `application.yml`  
> - En Node.js : `config.json`  
> - En Python : `.ini` (ou plus récemment, `TOML`)  


## Configuration par arguments en ligne de commande

L'utilisation de la ligne de commande est une méthode historique pour gérer la configuration des programmes. Elle permet de créer des variables de CLI (command-line interface), facilitant ainsi l'exécution de programmes directement via le terminal.

De nombreux programmes informatiques, qu'ils soient conçus pour effectuer des tâches rapidement (comme des scripts ou des outils en ligne de commande) ou pour fonctionner en arrière-plan en tant que serveurs (comme les serveurs web ou les bases de données), peuvent recevoir des paramètres (variables) directement via la ligne de commande lorsque vous les exécutez. En Python, il est possible de récupérer ces paramètres à l'aide du module `sys`.

Prenons, par exemple, le fichier `main.py` que nous avons construit précédemment. En ajoutant le code suivant :

```python
import sys
import logging

arguments = sys.argv
logging.basicConfig(filename=arguments[1], encoding='utf-8', level=logging.DEBUG)
```

et en lançant la commande :  
`python main.py toto.log`  

nous pouvons écrire les logs dans le fichier `toto.log`.

Cependant, un inconvénient de cette approche est qu'avec un nombre accru d'arguments, il devient plus facile de commettre des erreurs d'ordonnancement ou d'oublier un paramètre.

<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> <br/>Clique est une bibliothèque pour Python qui facilite la création d'interfaces en ligne de command <a href="https://click.palletsprojects.com/en/stable/">https://click.palletsprojects.com/en/stable/</a>
</div>

> À noter que cette méthode ne sera pas approfondie ici, car nous nous concentrerons principalement sur la configuration par variables d'environnement.


## Configuration par variable d'environnement

Les variables d’environnement sont des variables disponibles dynamiquement pour votre programme pendant l’exécution. Contrairement à des valeurs codées en dur, elles sont adaptables à l'environnement dans lequel l'application s'exécute. Cela permet une plus grande flexibilité et facilite les éventuelles modifications de configuration.

L'injection par variable d'environnement est une solution optimale, notamment pour des personnes extérieures au projet. Cette approche permet de surcharger facilement la configuration en renseignant des paramètres sous la forme :

```
CLE=valeur
```

> **Remarque :** Les noms des variables d'environnement suivent la notation `UPPER_SNAKE_CASE`.

En Python, la gestion des variables d'environnement peut être facilitée avec la librairie `python-dotenv`. Celle-ci permet de lire un fichier **.env** et d'exporter son contenu dans l'environnement d'exécution.

#### Exemple d'un fichier .env
```
# Configuration par défaut
CHEMIN_FICHIER_LOG=
ENVIRONNEMENT=local
```
Voici comment importer les variables depuis un fichier .env :

```python
from dotenv import load_dotenv

# Charge toutes les variables d'environnement depuis le fichier .env
load_dotenv()
```

Une fois chargées, les variables sont accessibles via la librairie `os` :

```python
import os

chemin_fichier_log = os.getenv("CHEMIN_FICHIER_LOG")
environnement = os.getenv("ENVIRONNEMENT")
```

Il est également possible de définir plusieurs fichiers pour organiser les configurations et éviter de versionner des données sensibles. Par exemple, vous pouvez charger un fichier local supplémentaire :

```python
from dotenv import load_dotenv
import os

# Charge le fichier principal
load_dotenv()

# Charge un fichier local si présent
local_env_path = ".env.local"
if os.path.exists(local_env_path):
    load_dotenv(dotenv_path=local_env_path, override=True)
```

<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> <br/>Variables d'environnement :  <a href="https://kinsta.com/knowledgebase/what-is-an-environment-variable/">https://kinsta.com/knowledgebase/what-is-an-environment-variable/</a>
</div>


## Travaux Pratiques

**OBJECTIF DU TP** : Créez un fichier `main.py` qui, au lancement de l'application, affiche dans les logs toutes les valeurs des variables d'environnement, à l'exception des mots de passe.  

1. Créez un fichier `.env.local` contenant :  
   - Une fausse URL de base de données  
   - Un faux mot de passe de base de données
   - La version actuelle de votre application (par défaut, utilisez `0.1` si vous n'avez pas encore commencé votre projet).  

2. Créez un fichier `.env.template` qui liste toutes les variables présentes dans le fichier `.env.local`. Ce fichier servira de modèle pour permettre aux développeurs de créer leur propre fichier `.env` localement.  

3. Ajoutez le fichier `.env.local` au `.gitignore` pour éviter qu'il soit versionné.  

4. Dans le fichier `main.py` : Implémentez une méthode qui charge les variables d'environnement globales ainsi que celles du fichier local (si un fichier `.env.local` est présent).  

5. Ajoutez une méthode qui affiche toutes les variables d'environnement (globales et locales), en masquant les valeurs des variables dont le nom contient les mots-clés suivants :  
   - `password`, `pwd`, `jeton`, `token`, `secret`, `key`, `cle`, `mdp`, ou `motdepasse`.  
   Pour ces variables, affichez des `****` à la place de leur valeur.  

6. Créez une nouvelle version de votre code (en veillant à ce que le fichier `.env.local` ne soit pas inclus dans la version). Envoyez ensuite cette version à votre dépôt distant.