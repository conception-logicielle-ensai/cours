---
title: "Automatisation des contrôles sur une base de code versionnée"
weight: 22
draft: false
description: ""
summary: ""
slug: "automatisation"
tags: ["automatisation","git hooks", "git actions"]
series: ["Cours"]
series_order: 2
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

    # Récupération des fichiers Python présents dans le staging
    STAGED_FILES=$(git diff --cached --name-only --diff-filter=d | grep '\.py$' || true)

    if [[ -z "$STAGED_FILES" ]]; then
        echo "Aucun fichier Python à vérifier."
        exit 0
    fi

    echo "Vérifications Ruff sur les fichiers ajoutés au staging..."

    # 1. Formatage
    echo "Formatage avec Ruff..."
    uv run ruff format $STAGED_FILES

    # 2. Corrections automatiques de lint
    echo "Corrections automatiques avec Ruff..."
    uv run ruff check --fix $STAGED_FILES

    # 3. Vérification finale bloquante
    echo "Vérification finale avec Ruff..."
    uv run ruff check $STAGED_FILES
    RUFF_STATUS=$?

    if [[ $RUFF_STATUS -ne 0 ]]; then
        echo "Erreur : des problèmes Ruff non corrigeables automatiquement subsistent."
        exit 1
    fi

    # Ré-ajout au staging
    echo "Ajout des fichiers modifiés au staging..."
    git add $STAGED_FILES

    echo "Toutes les vérifications sont passées. Commit autorisé."
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