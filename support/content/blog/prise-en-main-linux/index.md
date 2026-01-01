---
title: "Prise en main du poste linux"
weight: 2
draft: false
description: ""
slug: "prise-en-main-linux"
tags: ["git", "avance"]
---


![](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2021/11/what-is-ubuntu.png)

Cette section a pour objectif de vous accompagner dans la **prise en main du poste Linux** qui vous a été fourni lors de la rentrée du **06/01/2025**.

Elle constitue une base de travail pour configurer un **environnement de développement fonctionnel, cohérent et proche des standards professionnels**, que nous utiliserons tout au long du cours et des travaux pratiques.

---

## Pourquoi utiliser Linux ?

Linux occupe une place centrale dans le monde de l’informatique moderne, en particulier dans les domaines suivants :
- hébergement de sites web et d’applications,
- infrastructures cloud,
- serveurs,
- conteneurisation et DevOps,
- développement logiciel open source.

La grande majorité des plateformes d’hébergement et des serveurs dans le monde reposent sur des systèmes de type **Unix/Linux**.

| Operating System | Market Share |
|------------------|--------------|
| **Unix / Linux** | **79,4 %**   |
| **Windows**     | 20,6 %       |

> Source : Wikipédia – Usage share of operating systems

Lors des dernières séances du cours, vous serez amenés à **déployer et héberger vos projets** sur des infrastructures cloud. Être à l’aise avec Linux est donc un prérequis essentiel.

---

## Environnement de travail attendu

Pour suivre ce cours dans de bonnes conditions, votre poste doit disposer des outils suivants :

- **Visual Studio Code** : éditeur de code moderne et extensible
- **Python 3** et **pip** : langage principal du cours et gestionnaire de paquets
- **Git** : gestionnaire de versions
- **Docker** : conteneurisation et déploiement

Sur Linux, l’installation et la gestion de ces outils se font majoritairement via :
- le **terminal**,
- des **interfaces en ligne de commande (CLI)**.

L’objectif est de vous familiariser rapidement avec ces outils, très utilisés dans le monde professionnel.

---

## Vérification des outils déjà installés

Avant d’installer quoi que ce soit, il est recommandé de vérifier si les outils sont déjà présents sur votre machine.

### Vérifier Python 3
```bash
dpkg -l | grep python3
```

### Vérifier Visual Studio Code
```bash
dpkg -l | grep vscode
```

### Vérifier les paquets Python installés
```bash
python3 -m pip list
```

Si une commande ne renvoie rien ou provoque une erreur, cela signifie généralement que l’outil n’est pas installé.

---

## Distribution utilisée : Ubuntu

Pour les besoins du cours et des TP, nous utilisons la distribution **Ubuntu**, basée sur Debian.

Ubuntu dispose de plusieurs **gestionnaires de paquets**, dont les plus courants sont :

- **apt** : gestionnaire de paquets Debian (principal)
- **snap** : gestionnaire de paquets universels maintenu par Ubuntu

Un gestionnaire de paquets permet :
- d’installer des logiciels facilement,
- de gérer les dépendances automatiquement,
- de maintenir le système à jour.

Avant toute installation avec `apt`, il est recommandé de mettre à jour l’index des paquets :

```bash
sudo apt update
```

Cette commande synchronise la liste des logiciels disponibles depuis les dépôts configurés sur votre système.

---

## Installation des outils nécessaires (TP1)

Si aucun des outils requis n’est installé sur votre poste, vous pouvez exécuter le script suivant **dans un terminal**.

> ⚠️ Si certains outils sont déjà présents, vous pouvez n’exécuter que les lignes nécessaires.

```bash
sudo apt update

echo "Installation de Python 3, pip et Git"
sudo apt install -y python3-pip git-all python3-virtualenv

# Installation de Visual Studio Code via Snap
sudo snap install --classic code
```

### Détails des paquets installés

- `python3-pip` : gestionnaire de paquets Python
- `git-all` : installation complète de Git
- `python3-virtualenv` : création d’environnements virtuels Python
- `code` : Visual Studio Code

---

## Bonnes pratiques dès le début

✔ Utilisez le terminal régulièrement
✔ Lisez attentivement les messages d’erreur
✔ N’installez pas de paquets Python avec `sudo pip`
✔ Privilégiez les environnements virtuels
✔ Versionnez votre code avec Git dès le début

---

À partir de cette base, nous pourrons travailler efficacement sur les TP, les projets et les déploiements cloud.
