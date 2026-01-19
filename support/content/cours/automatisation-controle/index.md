---
title: "Automatisation des contrôles sur une base de code versionnée"
weight: 22
draft: false
description: ""
summary: ""
slug: "automatisation"
tags: ["automatisation","git hooks", "git actions"]
series: ["Cours"]
series_order: 8
---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/architecture-applicative

## Automatisation des contrôles en local


### Qu'est-ce qu'un git hooks ? 

Les hooks Git sont des scripts qui s'exécutent automatiquement dès qu'un événement particulier se produit dans un dépôt Git. Ils vous permettent de personnaliser le comportement interne de Git et de déclencher des actions personnalisables à des points clés dans le cycle de vie du développement.

### Création d'un hook

Nous souhaitons automatiser le formatage du code ainsi que la correction des avertissements issus de l’analyse statique (linter) pour les fichiers inclus dans notre prochain commit. Pour ce faire, nous allons mettre en place un hook Git chargé d’effectuer l’auto-formatage et la validation du code au moment du commit.

En supposant que vous ayez installé les packages nécessaires, sinon, tapez la commande suivante :

```bash
# Commande pour installer ruff
uv add --dev ruff
uv sync
```

Pour utiliser `ruff`, il vous suffit de taper les commandes suivantes à la racine de votre projet :

```bash
# Corriger automatiquement les problèmes simples
uv run ruff check --fix
# Appliquer le formatage automatique
uv run ruff format

# Afficher les problèmes détectés
uv run ruff check
# Vérifier que le code respecte les règles de formatage 
uv run ruff format --check
```

Pour cela, nous allons créer un fichier `pre-commit` personnalisé directement dans le dossier `.git/hooks` de votre projet.

> Voici un lien vers un tutoriel qui explique [comment créer un git hook](https://www.atlassian.com/fr/git/tutorials/git-hooks).

---

### TP création d'un hook `pre-commit`

Pour ce TP, nous allons nous concentrer sur l'utilisation de la ligne de commande.

#####  0. **Observer le dossier `.git`**

Nous allons parcourir le dossier `.git`, qui contient des informations cruciales sur la gestion des versions de notre projet. Pour cela, nous utiliserons les commandes suivantes :

```bash
# Se déplacer dans le répertoire du projet
cd chemin/vers/ton/projet
# Afficher le contenu du dossier .git
ls -la .git
# Entrer dans le dossier .git
cd .git
# Voir le contenu du dossier .git
ls
# Accéder au dossier objects
cd objects
# Afficher le contenu du dossier objects
ls
# Revenir au dossier .git
cd ..
# Revenir au dossier parent du projet
cd ..
```


> - **`ls`** : Affiche la liste des fichiers et des répertoires.
> - **`-l`** : Affiche les détails sous forme de liste (permissions, propriétaire, taille, etc.).
> - **`-a`** : Affiche tous les fichiers, y compris ceux qui commencent par un point (`.`), qui sont normalement cachés.

> **`cd`** : Permet de changer de dossier.

**Sous-dossiers importants dans `.git`**

Voici quelques exemples de sous-dossiers essentiels que vous pouvez trouver dans `.git` :

- **`objects`** : Contient les objets Git (commits, arbres, blobs).
- **`refs`** : Contient les références aux branches et aux tags.
- **`config`** : Fichier de configuration du dépôt Git.


##### 1. **Créer ou modifier le fichier `pre-commit`**
   Accédez au dossier `.git/hooks` de votre projet et créez un fichier nommé `pre-commit` :

   ```bash
   touch .git/hooks/pre-commit
   ```

   > `touch` est utilisée pour créer des fichiers vides

#####  2. **Ajouter du contenu au fichier `pre-commit`**
   Ouvrez ce fichier dans un éditeur de texte :

   ```bash
   nano .git/hooks/pre-commit
   ```

   > `nano` est un éditeur de texte en ligne de commande.

   Ajoutez le script suivant :

   ```bash
    #!/bin/bash
    set -e

    echo "=== Hook pre-commit ==="

    # Tous les fichiers du staging (sauf suppressions)
    STAGED_FILES=$(git diff --cached --name-only --diff-filter=d || true)

    if [ -z "$STAGED_FILES" ]; then
        echo "Aucun fichier à analyser."
        exit 0
    fi

    echo "Fichiers analysés :"
    echo "$STAGED_FILES"
    echo

    # ---------------------------------------------------
    # Étape 1 — Formatage automatique
    # ---------------------------------------------------
    echo "[1/2] Formatage automatique (ruff format)"
    if uv run ruff format $STAGED_FILES; then
        echo "Formatage terminé avec succès."
    else
        echo "Erreur lors du formatage."
        echo "Le commit est bloqué."
        exit 1
    fi
    echo

    # ---------------------------------------------------
    # Étape 2 — Corrections automatiques
    # ---------------------------------------------------
    echo "[2/2] Corrections automatiques (ruff check --fix)"
    if uv run ruff check --fix $STAGED_FILES; then
        echo "Corrections automatiques appliquées."
    else
        echo "Certaines règles Ruff ne peuvent pas être corrigées automatiquement."
        echo "Des erreurs Ruff nécessitent une correction manuelle."
        echo "Le commit est bloqué."
        exit 1
    fi
    echo


    # ---------------------------------------------------
    # Mise à jour du staging
    # ---------------------------------------------------
    echo "Mise à jour du staging..."
    git add $STAGED_FILES

    echo "Toutes les vérifications Ruff sont passées."
    echo "Commit autorisé."
    exit 0
   ```
   > `$(...)` est utilisé pour exécuter une commande et insérer son résultat dans une autre commande ou une variable.
   
   > `echo` est utilisée pour afficher du texte ou des variables dans le terminal. 

##### 3. **Rendre le fichier exécutable**
   Rendez le fichier `pre-commit` exécutable avec la commande suivante :

   ```bash
   chmod +x .git/hooks/pre-commit
   ```

   > `chmod` (abréviation de Change Mode) est utilisée en ligne de commande pour modifier les permissions d'accès à des fichiers ou des répertoires

##### 4. **Tester le hook**
   Essayez de committer un fichier dans votre projet pour voir si le hook est exécuté :

   ```bash
   git add .
   git commit -m "Test du hook pre-commit"
   ```

   Si des fichiers sont modifiés par `ruff`, le hook les ajoutera automatiquement au staging et réexécutera les étapes.

---

### Points importants

Si un fichier ne respecte pas les règles, `ruff` bloquera le commit. Vous devrez corriger les erreurs avant de réessayer.


## Automatisation des contrôles sur des dépots hébergés

Les Forges Logicielles : Gitlab, Github (etc ...) proposent une infrastructure de **runners** et **executeurs** qui permettent d'éxecuter des traitements lors d'événements liés a la mise a jour, publication ou autre de votre code.

L'un des principaux usages de ces fonctionnalité, c'est de faire tourner des vérifications statiques et dynamiques de votre code. Ainsi, vous n'avez plus a penser a vos contrôles, ils s'executent a chaque nouveau commit par exemple et donc cela vous permet de voir apparaître les bugs, les régression et d'avoir des arguments objectifs pour la validation ou le rejet d'ajout du code d'un collègue. 

> Cela permet également l'hébergement de pages web construites (comme ce site web par exemple 🔥).

### Github Actions, une introduction

![](/images/automatisation-controle/ghactions.webp)

GitHub Actions est un outil d'intégration et de déploiement continu (CI/CD) natif à GitHub, qui permet d’automatiser des workflows directement dans vos dépôts. Il repose sur trois concepts principaux :

- les Workflows : Définissent une suite d'actions à exécuter. Ils sont configurés via des fichiers YAML placés dans le dossier `.github/workflows/` du projet sur github.

- les Jobs : Chaque workflow est composé de jobs qui s’exécutent dans des environnements distincts.

- les Steps : Un job est lui-même décomposé en une séquence d'étapes, comprenant des actions prédéfinies ou personnalisées.

> Les traitements sont executés sur des machines fournies par github, vous pouvez voir la typologie des machines ici : https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners#standard-github-hosted-runners-for--private-repositories 

> **Dans le TP d'aujourd'hui et dans vos projets on privilégiera d'utiliser une machine qui est un `ubuntu-22.04` comme votre environnement personnel.**


Le format des workflow est en **YAML**, c'est un format de fichier comme le **JSON**, le **XML** ou le **CSV**. On le retrouve spécifiquement pour la configuration de fichier d'intégration et de configuration car il est assez *léger* a la lecture : il repose comme le python sur de l'indentation.

Exemple avec un yaml "de base".

```yaml
name: Simple Python Workflow

on: [push]

jobs:
  simple-check:
    runs-on: ubuntu-22.04

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Run a simple Python command
        run: python --version
```

Le principe est le suivant : 
- D'une machine nue on peut executer des commandes pour lui installer ou bien lancer des commandes a partir de ce qui est déjà installé sur la machine. 

Avec en plus : des jobs déjà construits par d'autres personnes c'est ce que l'on voit dans `actions/checkout@v4` `actions/setup-python@v5`

> On peut voir le code source ici pour le `actions/setup-python par exemple ici : https://github.com/actions/setup-python => https://github.com/actions/setup-python/releases/tag/v5.3.0  => https://github.com/actions/setup-python/blob/0b93645e9fea7318ecaed2b359559ac225c90a2b/src/setup-python.ts (oui c'est peu compréhensible de but en blanc, mais lors de problèmes c'est pratique de pouvoir voir du code, pour comprendre #opensource)

### Que souhaite-t-on automatiser ?

- La création et configuration des environnements

- Exécution automatique des tests

- Analyse de la qualité du code (lint)

- Validation de la qualimétrie via des outils et bots

> [!TIP]+ Nos préconisations
> 👉 Nous préconisons la mise en place systématique des tests et du lint sur vos projets, de façon transverse, en complément des githooks locaux, afin de garantir une qualité de code constante dès les premières étapes du développement.


### Scripts exemples fonctionnels

Pour une application qui utilise UV nous avons un script exemple ici :

```yaml
name: CI Python avec uv

on:
  push:
    branches: [ '**' ]
  pull_request:
    branches: [ main, develop ]

jobs:
  format:
    name: Format check avec Ruff
    runs-on: ubuntu-22.04
    
    steps:
    - name: Récupérer le code
      uses: actions/checkout@v4
    
    - name: Installer uv
      uses: astral-sh/setup-uv@v4
      with:
        version: "latest"
    
    - name: Configurer Python
      run: uv python install
    
    - name: Installer les dépendances
      run: uv sync --dev
 
    - name: Vérification format avec Ruff
      run: uv run ruff format --check

  lint:
    name: Linting avec Ruff
    runs-on: ubuntu-22.04
    
    steps:
    - name: Récupérer le code
      uses: actions/checkout@v4
    
    - name: Installer uv
      uses: astral-sh/setup-uv@v4
      with:
        version: "latest"
    
    - name: Configurer Python
      run: uv python install
    
    - name: Installer les dépendances
      run: uv sync --dev
    
    - name: Vérification lint avec Ruff
      run: uv run ruff check .

  test:
    name: Tests avec pytest
    runs-on: ubuntu-22.04
    
    steps:
    - name: Récupérer le code
      uses: actions/checkout@v4
    
    - name: Installer uv
      uses: astral-sh/setup-uv@v4
      with:
        version: "latest"
    
    - name: Configurer Python
      run: uv python install
    
    - name: Installer les dépendances
      run: uv sync --dev
    
    - name: Lancer les tests avec pytest
      run: uv run pytest

```

Ici : 

- 3 jobs : un Job `format`, un Job `Lint`, un Job `test`

Qui s'executent sur toutes les branches : 
```yaml
on:
  push:
    branches: [ '**' ]
  pull_request:
    branches: [ main, develop ]
```

Les 3 jobs s'appuient sur des actions déjà construites: 
- `actions/checkout@v4` : récupère le code en l'état git
- `astral-sh/setup-uv@v4` : installe uv dans l'environnement

Puis des scripts de `test` ou d'analyse via `ruff`

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
Ces 2 parties sont plutôt rapides à mettre en place et valent quelques précieux points. Nous vous conseillons de les mettre en place au plus vite (genre aujourd'hui) au sein de l'équipe et au niveau du dépôt de code.
{{</alert>}}
