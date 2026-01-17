---
title: "Outils d'analyse statique d'une base de codet"
weight: 24
draft: false
description: "Comprendre et mettre en place l’analyse statique pour améliorer la qualité et la sécurité d’une base de code"
summary: "Analyse statique, linting, formatting et SAST"
slug: "analyse-statique"
tags: ["qualimetrie", "linting", "formatting", "SAST"]
series: ["Cours"]
series_order: 6

------

> L'analyse statique du code consiste à l'analyser SANS le lancer.

## Petite définition

L’analyse du code statique, appelée aussi analyse du code source ou révision du code statique, est le processus consistant à **détecter les mauvaises habitudes de codage, les vulnérabilités potentielles et les défauts de sécurité dans le code source** d’un logiciel, **sans pour autant exécuter ce dernier**. Il s’agit d’une forme de testing en boîte blanche.

Ce type d’analyse permet à vos équipes de repérer les bugs ou vulnérabilités que les autres outils et méthodes de test, par exemple les révisions de code manuelles et les compilateurs, ne relèvent généralement pas.

---

## Linting (analyse statique du code)

![Illustration linting](https://www.eficode.com/hubfs/application%20management%20blog%20-%20How%20to%20write%20better%20code%20with%20linting,%20formatting,%20and%20analysis%20tools.png)

Dans la plupart des langages de programmation, et en particulier dans les langages **faiblement typés** et **non compilé**, il est important de mettre en place des règles pour garantir la qualité du code source.

Pour cela, on utilise des **linters** (ou outils d’analyse statique).
Ces outils analysent le code **sans l’exécuter**.

### Pourquoi utiliser un linter ?

**Pour le style**

* assurer une cohérence du code au sein d’une équipe,
* définir des règles claires et partagées,
* éviter les débats subjectifs (« je préfère écrire comme ça »).

**Pour la qualité**

* détecter des erreurs avant même de lancer le programme (variables inutilisées, imports incorrects, code mort, etc.),
* réduire les bugs simples,
* améliorer la lisibilité et la maintenabilité du code.

> En pratique, le linting permet de transformer des règles de bonne écriture en règles **automatiques**, objectives et appliquées de manière uniforme.

En Python, il existe plusieurs outils connus :

* `flake8`
* `pylint`
* `autopep8`
* `ruff`

Ces outils s’appuient en grande partie sur les **PEP** (Python Enhancement Proposals), qui définissent des règles communes pour écrire du code Python lisible et cohérent.

Les plus importantes sont :

* **PEP 7** : conventions pour le code C de Python
  [https://peps.python.org/pep-0007/](https://peps.python.org/pep-0007/)
* **PEP 8** : conventions de style pour le code Python
  [https://peps.python.org/pep-0008/](https://peps.python.org/pep-0008/)

> Les règles ne sont **pas figées** : chaque projet peut choisir celles qu’il applique ou non.

> **Fun fact** : vous utilisez déjà des linters sans le savoir. La plupart des IDE en intègrent par défaut, souvent avec une configuration minimale.

---

### L’outil `ruff`

**Ruff** est un outil open source développé par **Astral** (la même équipe que `uv`).
Il sert à la fois de **linter** et de **formatter** pour le code Python.

Il permet de **remplacer plusieurs outils classiques par un seul**.

Ruff est écrit en **Rust**, ce qui le rend beaucoup plus performant que la plupart des linters écrits en Python.

#### Utilisation du linter `ruff`

Exemples de commandes courantes :

```bash
# Ajouter ruff comme dépendance de développement
uv add --dev ruff

# Analyser le code et afficher les erreurs
uv run ruff check

# Analyser et corriger automatiquement les erreurs corrigeables
uv run ruff check --fix
```

#### Configuration de Ruff

Les règles de Ruff sont définies dans un fichier **`ruff.toml`** à la racine du projet.

Voici un exemple de **configuration simple** pour un projet Python :

```toml
# -------------------------------------------------
# Paramètres globaux
# -------------------------------------------------

# Longueur maximale d’une ligne (compatible avec Black)
line-length = 88

# Répertoires exclus de l’analyse
exclude = [
    ".venv",
    "venv",
    "__pycache__",
    "migrations",
]

# Affiche les corrections appliquées automatiquement
show-fixes = true


# -------------------------------------------------
# Linting (analyse statique)
# -------------------------------------------------

[lint]

# Règles activées
select = [
    "E",    # Erreurs de style (indentation, espaces…)
    "F",    # Erreurs logiques (imports manquants, variables inutilisées)
    "W",    # Avertissements de style
    "I",    # Organisation des imports
    "B",    # Bugs subtils courants
    "C4",   # Compréhensions inutilement complexes
    "SIM",  # Simplifications évidentes
    "UP",   # Syntaxe Python moderne
    "N",    # Conventions de nommage
    "ARG",  # Paramètres de fonctions inutilisés
]

# Règles ignorées
ignore = [
    "E501", # Longueur de ligne (gérée par le formatter)
]


# -------------------------------------------------
# Gestion des imports
# -------------------------------------------------

[lint.isort]

combine-as-imports = true
force-sort-within-sections = true
lines-after-imports = 2
```

Par défaut, Ruff ignore les fichiers listés dans le `.gitignore`.

#### Ressources utiles

* Tutoriel officiel Ruff :
  [https://docs.astral.sh/ruff/tutorial/](https://docs.astral.sh/ruff/tutorial/)

* Documentation complète des options :
  [https://docs.astral.sh/ruff/settings/](https://docs.astral.sh/ruff/settings/)

___


### Pour aller plus loin

* Panorama plus large des outils d’analyse et de linting Python :
  [https://github.com/vintasoftware/python-linters-and-code-analysis](https://github.com/vintasoftware/python-linters-and-code-analysis)

---

## Formatting (formatage du code)

![Illustration formatting](https://i.ytimg.com/vi/j1MbEYhYj_Y/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD\&rs=AOn4CLAX3DL8ZBkglQ7rk7uuAURahEw_mA)

Une fois les règles définies par le **linting**, il est utile d’automatiser leur application à l’aide d’un **formatter**.

Un formatter se charge de mettre en forme le code de manière automatique et uniforme
(indentation, retours à la ligne, guillemets, organisation générale), sans modifier son comportement.

Ces outils sont généralement intégrés aux IDE
(Visual Studio Code, PyCharm, …) et peuvent s’exécuter automatiquement à la sauvegarde d’un fichier.

### Pourquoi utiliser un formatter ?

L’objectif principal du formatage automatique est de **séparer le fond de la forme**.

Il permet de ne conserver dans l’historique Git que des changements réellement fonctionnels, en évitant les commits pollués par de simples modifications de style. Cela facilite la relecture du code ainsi que les revues de pull requests.

En Python, il existe plusieurs outils connus :

* `black`
* `yapf`
* `autopep8`
* `isort`
* `ruff`


---

### Le formatter `ruff`

Ruff ne se limite pas au linting : il peut également être utilisé comme **formatter** Python.

```bash
# Ajouter ruff comme dépendance de développement
uv add --dev ruff

# Vérifier le formatage du code (sans modifier les fichiers)
uv run ruff format --check

# Formater automatiquement le code
uv run ruff format
```

#### Configuration de Ruff

Toujours dans le fichier **`ruff.toml`** à la racine du projet.

```toml
# -------------------------------------------------
# Formatage du code
# -------------------------------------------------

[format]

# Style de guillemets imposé
quote-style = "double"

# Style d’indentation (standard Python)
indent-style = "space"

# Type de fin de ligne (compatibilité multiplateforme)
line-ending = "lf"
```

---

### Pour aller plus loin

* Liste de formatters Python :
  [https://github.com/life4/awesome-python-code-formatters](https://github.com/life4/awesome-python-code-formatters)

---

## Security testing

### SAST (Static Application Security Testing)

![Illustration SAST](/images/statique/sast.png)

Le **Static Application Security Testing (SAST)** regroupe des outils et des techniques utilisés en sécurité informatique pour identifier des **vulnérabilités potentielles directement dans le code source** d’une application.

Ces analyses sont réalisées **sans exécuter le programme**. Elles permettent de détecter des problèmes de sécurité dès la phase de développement, avant la mise en production.

Les outils de SAST peuvent notamment repérer :

* des injections (SQL, commandes, entrées utilisateur mal contrôlées),
* des failles de type XSS (*Cross-Site Scripting*),
* des erreurs de gestion des ressources ou de la mémoire,
* l’utilisation de bibliothèques contenant des failles connues.

Utilisé régulièrement, le SAST permet d’intégrer la sécurité dès la conception de l’application (*security by design*) et de réduire significativement les risques de failles.

Pour ce cours, nous utiliserons l’outil **Snyk**.
Il existe cependant de nombreux autres outils d’analyse de sécurité des applications, comme **Sonar**, utilisé notamment à l’INSEE.

### Sécurité : quelques rappels

En sécurité informatique, on distingue plusieurs grandes familles d’attaques courantes :

* **Les attaques par injection** (SQL, texte, commandes, etc.)
  Elles consistent à manipuler les entrées de l’application afin de détourner son comportement prévu. Elles représentent une part importante des failles de sécurité.

* **Le Cross-Site Scripting (XSS)**
  Il s’agit d’injecter du code JavaScript dans une application web afin qu’il soit exécuté côté utilisateur, dans un but malveillant.

* **Les failles de sécurité connues (CVE)**
  Ce sont des vulnérabilités identifiées publiquement dans des bibliothèques ou des versions de logiciels, qui peuvent être exploitées si elles ne sont pas corrigées.

* **Les attaques par déni de service (DDoS)**
  Elles visent à surcharger un service (réseau, mémoire, CPU) pour le rendre partiellement ou totalement indisponible.

* **Les attaques de type “Man in the Middle”**
  Elles consistent à intercepter les communications afin de récupérer des informations sensibles et usurper l’identité d’un utilisateur.

---

## TP — Mise en place de la qualité et de la sécurité sur votre projet

### Étape 1 — Mettre en place le linting avec Ruff

1. Ajoutez **Ruff** comme dépendance de développement au projet.
2. Créez un fichier `ruff.toml` à la racine du projet.
3. Configurez Ruff pour le linting, en utilisant la configuration vue en cours ou une configuration qui vous semble pertinente.
4. Lancez l’analyse du code avec Ruff.
5. Corrigez automatiquement les problèmes pouvant l’être.

### Étape 2 — Mettre en place le formatting avec Ruff

1. Ajoutez la section de formatage dans le fichier `ruff.toml`, en vous basant sur la configuration vue en cours ou sur une configuration cohérente avec votre projet.
2. Vérifiez que le code respecte les règles de formatage.
3. Appliquez le formatage automatique.

### Étape 3 — Sécurité statique (SAST) avec Snyk

* Créez un compte sur **Snyk** : [https://snyk.io](https://snyk.io)
* Connectez votre compte **GitHub** à Snyk.
* Dans l’interface Snyk, rendez-vous dans l’onglet :
  **Projects → GitHub**
* Sélectionnez votre dépôt :
  `{votre_user}/{nom_du_projet}`
* Lancez l’analyse de sécurité du projet.
* Consultez les vulnérabilités détectées, aussi bien dans le code que dans les dépendances.

> N’oubliez pas d’ajouter une description de ces outils dans votre `README` et de **committer l’ensemble des fichiers créés ou modifiés**.