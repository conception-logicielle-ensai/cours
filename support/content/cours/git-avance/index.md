---
title: "Git Avancé"
weight: 40
draft: false
description: "Consolider l'usage de Git dans un cadre de projet et de conception logicielle (quand, quoi, comment, pourquoi)"
summary: "Notions avancées de Git pour le travail en projet"
slug: "git-avance"
tags: ["git", "avance"]
series: ["Cours"]
series_order: 1

---

## Choix du client utilisé pendant le cours

Comme vous allez le voir, **Git est un système de contrôle de version**.

Il repose sur des protocoles standardisés (comme HTTP, SSH ou son propre protocole Git) pour assurer la communication entre dépôts locaux et distants. Cette standardisation permet à Git d’être compatible avec de nombreux outils et interfaces, qu’il s’agisse de la ligne de commande, d’environnements de développement intégrés (IDE), ou d’applications graphiques comme GitHub Desktop, Git Kraken, ou les plugins Visual Studio Code.

---

Tout au long de ce cours, vous utiliserez exclusivement le terminal pour mieux comprendre les commandes Git et leur fonctionnement en profondeur.  
Une fois que vous maîtrisez un concept et les commandes associées, vous êtes libres de passer à l’outil graphique de votre choix, comme [Git Extensions](https://gitextensions.github.io/), le [plugin Git pour Visual Studio Code](https://code.visualstudio.com/docs/editor/versioncontrol), ou [Git Kraken](https://www.gitkraken.com/).  

--- 

Pour ce chapitre, vous pouvez pratiquer dans l'environnement de votre choix, à condition d'avoir `git` installé et accessible via un terminal.  

Nous vous encourageons vivement, tout au long de ce chapitre et des suivants, à explorer différents environnements pour découvrir les diverses possibilités, ainsi que les similitudes et les différences.  

Voici quelques exemples d'environnements que vous pouvez utiliser :  

- Votre machine personnelle avec Git installé  
- Tout environnement du [SSPCloud](https://datalab.sspcloud.fr/), comme Cloudshell, VSCode, ou RStudio  

> **Feuille de commandes utiles pour Git** : [https://education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)  

> Pour accéder à un récapitulatif rapide des commandes Git les plus courantes, vous pouvez utiliser la commande suivante dans un terminal : 
> ```
> git --help
> ```  


## Pourquoi utiliser git ?
![ ](/images/git/versionning_problem.png)

Vous avez besoin de Git chaque fois :

 - vous collaborez avec d'autres
 - vous souhaitez garder une trace de votre travail au fil du temps
 - Vous désirez récupérer du code et y contribuer
 - Vous écrivez du code

---

Git est un système de contrôle de version décentralisé (Distributed Version Control System - DVCS) qui aide les développeurs à gérer leurs projets logiciels. 
Il permet de conserver toutes les versions du code, ce qui est utile lors de collaborations, car il simplifie la gestion des contributions de chacun et prévient les conflits. 
Git garde également un historique complet des modifications, essentiel pour suivre l'évolution d'un projet.
Grâce à des demandes de tirage (pull requests) et à des branches, il facilite l'intégration des modifications de manière organisée. 
Les développeurs peuvent revenir à des versions antérieures pour comprendre des fonctionnalités ou des problèmes, tout en synchronisant leur travail local avec les environnements distants. 

> Documentation officiel, À propos de la gestion de version : https://git-scm.com/book/fr/v2/D%c3%a9marrage-rapide-%c3%80-propos-de-la-gestion-de-version

## Utilisation de git en local

### Initialiser un projet Git

Pour commencer un projet Git, il suffit d'exécuter la commande suivante :

```
git init
```

Cette commande crée un sous-dossier nommé `.git` dans le répertoire courant. Ce dossier contient tous les fichiers nécessaires au suivi des modifications, y compris l'historique des versions, les références et les configurations du dépôt.

Lorsque l'on récupère un dépôt depuis Internet, on obtient également le dossier `.git` dans notre répertoire local. **La présence de ce dossier indique à Git que nous sommes dans un dépôt Git**.

> Notez que le dossier `.git` est masqué dans votre explorateur de fichiers (comme tous les dossiers dont le nom commence par un point). Cependant, il est possible de le parcourir et de le modifier, comme nous le verrons dans la section [🔄 Automatisation des contrôles sur une base de code versionnée](/docs/automatisation).

> Consultez la documentation officielle : [Git Init](https://git-scm.com/docs/git-init).


### Gestion des versions

#### Création d'une version

Dans un dépôt Git, vous avez la possibilité de créer de nouvelles versions de votre projet en spécifiant les fichiers que vous souhaitez inclure dans cette version. Ce processus est souvent appelé "staging".

**Objectif du Staging**

L'objectif principal de cette fonctionnalité est de vous permettre de travailler localement sans vous préoccuper du versionnage. Lorsque vous êtes prêt à créer une nouvelle version, vous pouvez regrouper vos fichiers de manière logique et cohérente.

*Exemple :*

Imaginons que vous développiez une application de gestion de bibliothèque, avec deux fonctionnalités principales :

1. Ajout de nouveaux livres à la base de données.
2. Suppression de livres de la base de données.

Après avoir apporté des modifications à votre code, il est préférable de créer une version pour chaque ajout fonctionnel. Par conséquent :

- D'abord, vous enregistrez les changements liés à l'ajout de livres.
- Ensuite, vous traitez les modifications apportées à la suppression de livres.

**Ajout de Fichiers au Staging**

Pour préparer les fichiers en vue de la prochaine version, vous utilisez la commande `git add`. Par exemple :

```bash
git add nom_du_fichier
```

Ou, pour ajouter tous les fichiers modifiés dans le répertoire courant :

```bash
git add .
```

Git utilise ce qu'on appelle un **index**, qui sert de transition entre votre répertoire de travail (working directory) et la version finale (commit).

Pour vérifier l'état de vos fichiers, vous pouvez utiliser la commande :

```bash
git status
```

**Qu'est-ce qu'un Commit ?**

Un commit représente un ensemble de modifications (ajouts, modifications, suppressions) sauvegardées dans votre dépôt Git.

Lorsque tous les fichiers ajoutés sont prêts et constituent un ensemble cohérent pour une nouvelle version, vous pouvez créer un commit avec la commande :

```bash
git commit [options]
```

*Exemple :*

```bash
git commit -m "Ajout de la fonctionnalité d'ajout de livres"
```

Un commit contient, en plus des modifications apportées, plusieurs métadonnées, notamment la date, l'auteur (nom et adresse e-mail) et un message descriptif. 

> Ces métadonnées sont déclaratives, ce qui signifie que vous pouvez anti-dater ou post-dater un commit si vous le souhaitez.

> Consultez la documentation officielle : [Git Commit](https://git-scm.com/docs/git-commit).


![ ](/images/git/explained-git-staging-area.png)

#### Naviguer dans les versions


Git offre plusieurs commandes pour gérer l'historique des versions et naviguer entre les différentes modifications.

**Annuler des Modifications**

La commande `git restore` est utilisée pour annuler les modifications **dans votre répertoire de travail** ou pour restaurer des fichiers à partir d'un commit précédent.

*Exemple :*

Si vous avez modifié un fichier, mais que vous souhaitez annuler ces modifications avant de faire un commit, vous pouvez utiliser :

```bash
git restore nom_du_fichier
```

Pour restaurer un fichier à partir d'un commit spécifique, vous pouvez indiquer l'identifiant du commit :

```bash
git restore --source <commit_id> nom_du_fichier
```

Cette commande est utile lorsque vous avez besoin de revenir à une version antérieure d'un fichier sans affecter les autres modifications dans votre branche actuelle.

> Consultez la documentation officielle : [Git Restore](https://git-scm.com/docs/git-restore).

---

La commande `git reset` est utilisée pour annuler des modifications **dans un dépôt Git**. Elle peut modifier l'historique des commits, l'index (la zone de staging) et le répertoire de travail, selon les options que tu utilises avec cette commande.

**Types de `git reset`**

1. **`git reset --soft <commit>`** :  
   Déplace le HEAD de la branche courante vers le commit spécifié, en gardant les modifications dans l'index.

   ```bash
   git reset --soft HEAD~1
   ```

2. **`git reset --mixed <commit>`** (par défaut) :  
   Déplace le HEAD vers le commit spécifié, mais retire les modifications de l'index tout en les gardant dans le répertoire de travail.

   ```bash
   git reset HEAD~1
   ```

3. **`git reset --hard <commit>`** :  
   Déplace le HEAD vers le commit spécifié et réinitialise l'index et le répertoire de travail pour correspondre à ce commit, entraînant la perte de toutes les modifications non engagées.

   ```bash
   git reset --hard HEAD~1
   ```

> Consultez la documentation officielle : [Git Reset](https://git-scm.com/docs/git-reset).
---


**Explorer l'Historique des Commits**

La commande `git log` vous permet d'afficher l'historique des commits dans votre dépôt. Cela vous aide à comprendre les changements qui ont été apportés au fil du temps.

![ ](/images/git/gitlog_schema.png)

*Exemple :*

Pour afficher l'historique des commits, vous pouvez simplement exécuter :

```bash
git log
```

Cela affichera une liste des commits avec leurs identifiants, auteurs, dates et messages associés. Si vous souhaitez afficher l'historique de manière plus concise, vous pouvez utiliser des options comme `--oneline` :

```bash
git log --oneline
```

Cela montre chaque commit sur une seule ligne, ce qui peut être plus lisible pour une vue d'ensemble rapide.

> Consultez la documentation officielle : [Git Log](https://git-scm.com/docs/git-log).


## GitFlow : Branches et Fonctionnalités

L'objectif de GitFlow est de déterminer un mode de fonctionnement en projet qui permet de découper le travail en différents degrés de maturité :

- **Version stable de l'application** : La dernière version livrée, qui est fonctionnelle et prête à être utilisée.
- **Version en cours de développement** : Une version qui peut fonctionner, mais qui nécessite probablement des ajustements avant l'intégration finale, ou qui attend l'achèvement de certaines fonctionnalités.
- **Versions temporaires** : Branches destinées à des fonctionnalités en cours de développement, comme la correction de bogues ou l'ajout de nouveaux endpoints.

Pour gérer ces différentes versions, il est essentiel de comprendre le concept de branches dans Git.

#### Branches

Une branche dans Git représente une ligne du temps indépendante, partant d'un commit ou d'une version précise. Elle permet de travailler sur des tâches spécifiques sans affecter le reste du projet et facilite le travail en équipe sur des fonctionnalités distinctes.

**Commandes pour gérer les branches :**

Pour créer une nouvelle branche :

```bash
# Créer une branche appelée nouvelle-branche
git branch nouvelle-branche

# Supprimer la branche nouvelle-branche en local
git branch -d nouvelle-branche

# Lister toutes les branches disponibles
git branch

```

Pour se déplacer sur une branche existante :

```bash
# Si nouvelle-branche existe
git switch nouvelle-branche

# Créer une branche nouvelle-branche et s'y déplacer
git switch -c nouvelle-branche
```

> Consultez la documentation officielle : [Git Branch](https://git-scm.com/docs/git-branch)

> Consultez la documentation officielle : [Git Switch](https://git-scm.com/docs/git-switch)

#### Intégration des Changements

Pour intégrer des modifications issues d'une autre branche dans la branche courante, vous pouvez utiliser la commande `git merge`.

**Exemple :**

Pour intégrer les changements de la branche `nouvelle-branche` dans la banche `branche-stable` :

```bash
git switch branch_stable
git merge nouvelle_branche
```

Cela fusionnera les modifications de `nouvelle-branche` dans la branche `branche-stable`.

> Pour en savoir plus sur la gestion des conflits lors d'une fusion, consultez [cette documentation sur les conflits Git](https://opensource.com/article/20/4/git-merge-conflict).

> Consultez la documentation officielle : [Git Merge](https://git-scm.com/docs/git-merge)


---

La commande  `git rebase` est une autre manière d'intégrer des changements. Au lieu de créer un commit de fusion, elle déplace la base de la branche actuelle pour inclure les commits d'une autre branche. Cela rend l'historique plus linéaire.

![ ](/images/git/git_merge_rebase.webp)

**Exemple :**

Pour intégrer les changements de la branche `nouvelle-branche`  dans la banche `branche-stable`:

```bash
git switch nouvelle_branche
git rebase branch_stable
git switch branch_stable
git merge nouvelle_branche
```
> Consultez la documentation officielle : [Git Rebase](https://git-scm.com/docs/git-rebase)


#### Branches de Fonctionnalités et Branches Stables

Dans le cadre de GitFlow, on distingue plusieurs types de branches :

##### Branches Stables

- **master/main** : La branche de référence, contenant la version la plus stable du projet.
- **release-1, release-2, ...** : Branches qui intègrent des changements depuis le développement.
- **develop** : Branche contenant le travail en cours, sanctuarisée et stabilisée.

##### Branches de Fonctionnalités

- **Hotfix** : Souvent nommées `hotfix-**`, ces branches permettent d'apporter des modifications urgentes et critiques sur les versions stables, par exemple, pour corriger des bogues de sécurité.
  
- **Feature** : Souvent nommées `topic-**`, ces branches sont utilisées pour le développement de nouvelles fonctionnalités.

![ ](/images/git/gitflow.webp)

##### Workflow Classique

Le workflow classique suit ces étapes :

1. Un développement est effectué sur une branche de fonctionnalité.
2. La branche est soumise à une revue par l'équipe du projet.
3. Une fois approuvé, les changements sont intégrés à la dernière version de la branche `develop`.

> Pour approfondir vos connaissances sur les workflows classiques de travail avec Git, vous pouvez consulter [ce petit guide](https://gist.github.com/blackfalcon/8428401).

## Utilisation avec un dépot distant

### Git, gitlab, github ...

![ ](/images/git/gitlab-github-remote.png)


Avec la réussite de Git, des outils appelées Forges Logicielles sont apparues. Elles permettent de proposer l'hébergement du code source d'application publiques et privées de manière gratuite et offrent d'autres services de gestion ainsi que des interfaces clients appréciables.

On distingue 2 catégories de forges : 

- les forges "As A Service" qui sont mises à disposition par des entreprises tierces et dont les données sont hébergées chez ces fournisseurs
- les forges "On premise" qui sont hébergées directement dans les organisations qui les utilisent.

Exemples de forges "As A Service" :

- [Github.com](https://github.com) est un des leaders du marché, hébergeant une grande partie du code open source et des grands projets ouverts. Github a été racheté par Microsoft en 2018 pour 7.5 milliards de dollars. Le code source de github n'est pas public.
- [Gitlab.com](https://gitlab.com) est un concurrent très actif. Le code qui sous-tend gitlab.com est en très grande partie libre : https://gitlab.com/gitlab-org/gitlab
 
    Important : `gitlab.com` est une installation particulière du logiciel gitlab sur les serveurs de l'entreprise gitlab. Vous rencontrerez, dans votre carrière, d'autres installations du logiciel gitlab sur d'autres serveurs (cf "On premise"). Attention donc à ne pas confondre le service `gitlab.com`, le logiciel gitlab et les différentes installations de gitlab que vous rencontrerez.

- [Bitbucket.org](https://bitbucket.org/) : moins utilisé, il appartient à Atlassian (connu pour son outil de gestion de projet / ticket `Jira`)

Exemples de forges "On premise" :

- [Gitlab sspcloud](https://git.lab.sspcloud.fr)
- [Gitlab INSEE](https://gitlab.insee.fr) : accessible uniquement depuis le réseau INSEE
- [FramaGIT](https://framagit.org/explore/projects)

De nos jours, la plupart des forges "on premise" sont des installations du logiciel gitlab mais il existe des alternatives. Citons par exemple https://gogs.io/ et https://fusionforge.org/

### Dépôts centraux

Les dépôt centraux se présentent comme des dépôt locaux hébergés sur des serveurs distants.

Il s'agit là de versions "canoniques" qui peuvent être récupérées par des développeurs habilités, ou par tout le monde. On parle alors de dépôt public (et non pas forcément d'opensource).

On peut intéragir avec eux a l'aide de commandes dédiées qui permettent de mettre à jour un dépôt, local ou distant, par rapport à un autre.


#### Cloner un Dépôt

La commande `git clone` est utilisée pour créer une copie locale d'un dépôt distant. Cela vous permet de commencer à travailler sur un projet existant sans avoir à le créer manuellement.

**Exemple :**

Pour cloner un dépôt, exécutez la commande suivante :

```bash
git clone <url_du_dépôt>
```

Cela créera un nouveau dossier avec le nom du dépôt et téléchargera tout le contenu du dépôt distant, y compris l'historique des commits.

> Vous récupérer le code source ainsi que le dossier `.git`

> Consultez la documentation officielle : [Git Clone](https://git-scm.com/docs/git-clone).

> Pour aller plus loin,
[Authentification : ssh/https](https://gist.github.com/Ragatzino/791caa39f7522dc3001ba3b24372507c). Cela vous permez de notament récupéré des projet privé dont vous avez des droits ou propager vos modifications sans vous authentifier à chaque fois.

#### Gérer les Dépôts Distants

La commande `git remote` permet de gérer les connexions aux dépôts distants. Vous pouvez l'utiliser pour ajouter, modifier ou supprimer des références à des dépôts distants.

**Exemple :**

```bash
# Ajouter un nouveau dépôt distant :
git remote add <nom> <url_du_dépôt>
# Lister les dépôts distants configurés
git remote -v
```

>  `<nom>` est souvent `origin`, qui est la convention pour le dépôt distant principal.

> Consultez la documentation officielle : [Git Remote](https://git-scm.com/docs/git-remote).

#### Récupérer les Modifications

La commande `git pull` est utilisée pour récupérer et intégrer les modifications d'un dépôt distant dans votre branche locale.

**Exemple :**

Pour récupérer les modifications de la branche principale d'un dépôt distant :

```bash
git pull origin main
```

Cela mettra à jour votre branche locale avec les derniers changements du dépôt distant.

> Consultez la documentation officielle : [Git Pull](https://git-scm.com/docs/git-pull).

> Pour aller plus loin, [Récuperer des changements sans les intégrer, à l'aide de la commande git fetch](https://www.atlassian.com/git/tutorials/syncing/git-fetch)

#### Envoyer des Modifications

La commande `git push` est utilisée pour envoyer vos modifications locales vers un dépôt distant (déclaré avec la commande remote). Cela permet de partager vos travaux avec d'autres développeurs et de mettre à jour le dépôt central.

**Exemple :**


```bash
# Envoyer vos changements (commits) à votre origin
git push
# Envoyer votre code a votre origin et déclarer une branche origin/nouvelle-branch
git push --set-upstream origin nouvelle-branch
# Envoyer votre code (commits) a votre origin en pushant la branche main
git push -u origin main
```

Cela mettra à jour le dépôt distant avec les commits que vous avez effectués localement.

> Consultez la documentation officielle : [Git Push](https://git-scm.com/docs/git-push).

![ ](/images/git/gitcommande_resume.png)

### L'écosystème Git : Issues, Pull Requests et Forks

Git s'inscrit dans un écosystème étendu grâce à des plateformes comme GitHub et GitLab. Ces outils ont enrichi Git avec des fonctionnalités supplémentaires pour une organisation collaborative plus structurée à l'échelle macro-projet. Voici un aperçu des concepts clés : **issues**, **pull requests** et **forks**.

---

#### Glossaire des Concepts

##### **Issues** : Gestion des Problèmes et des Demandes
Une *issue* est un ticket utilisé pour signaler un bug, proposer une nouvelle fonctionnalité, ou discuter d’une amélioration. Les issues servent de point de départ pour organiser le travail collaboratif. Elles incluent souvent des descriptions détaillées, des captures d’écran, et des discussions entre contributeurs.

- **Cas d'usage** :
  - Reporter un bug trouvé dans le projet.
  - Suggérer une nouvelle fonctionnalité.
  - Suivre les tâches à accomplir dans un projet.
  
- **Outils associés** : GitHub, GitLab, Jira.  
  Exemple d’issue sur GitHub : [Créer une Issue GitHub](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues).

---

##### **Pull Requests (ou Merge Requests)** : Proposition de Fusion de Code
Une *pull request* est une demande formelle pour intégrer les modifications effectuées sur une branche vers une autre (souvent vers la branche principale). Elle est accompagnée d’un processus de revue de code où d’autres développeurs vérifient, commentent, et demandent des modifications avant la fusion finale.

- **Étapes typiques** :
  1. Créer une branche dédiée pour votre travail.
  2. Faire des commits pour sauvegarder vos modifications.
  3. Soumettre une pull request pour demander la fusion de cette branche.
  4. Collaborer via des commentaires ou des suggestions dans la pull request.
  5. Fusionner la branche une fois approuvée.

- **Cas d'usage** :
  - Ajouter une fonctionnalité.
  - Corriger un bug signalé via une issue.
  - Améliorer la documentation.

- **Outils associés** : 
  - GitHub : [Créer une Pull Request](https://docs.github.com/en/pull-requests).
  - GitLab : [Créer une Merge Request](https://docs.gitlab.com/ee/user/project/merge_requests/).

---

##### **Forks** : Copier un Dépôt Git pour le Personnaliser
Un *fork* est une copie indépendante d’un dépôt Git, généralement utilisée pour travailler sur un projet sans modifier l’original. Il permet à des développeurs externes de contribuer à des projets où ils n'ont pas les droits d'écriture ou de récupérer un projet pour en créer une version différente.

- **Cas d'usage** :
  - **Contribution à un projet open-source** :
    1. Forker un projet public.
    2. Travailler sur la copie dans votre espace personnel.
    3. Soumettre une pull request pour proposer vos modifications.
  - **Sauvegarde et évolution d'un projet** :
    - Lorsqu’un projet open-source est abandonné ou passe en privé, un fork permet à une communauté ou à une personne de continuer son développement.

- **Outils associés** :
  - [Forks sur GitHub](https://docs.github.com/en/get-started/quickstart/fork-a-repo).
  - [Forks sur GitLab](https://docs.gitlab.com/ee/user/project/repository/forking_workflow.html).

--- 

## Qu'est-ce qu'il est utile de versionner ?  

Lorsqu'on commence à versionner un projet avec Git, une question essentielle se pose : **quels fichiers doit-on inclure dans le contrôle de version ?** Voici quelques explications pour guider vos choix et éviter les erreurs courantes.  

---

### Comment fonctionne Git ?  

Git fonctionne en sauvegardant des différences (deltas) entre les versions successives des fichiers. Cela signifie qu'il ne stocke pas une copie complète de chaque version, mais uniquement les modifications apportées. Ce mécanisme est particulièrement efficace pour les fichiers texte, mais il peut rencontrer des limitations avec d'autres types de fichiers.  

---
### Type de fichiers et versionning

#### Les fichiers texte  

Pour les fichiers texte (comme les fichiers `.md`, `.txt`, `.py`, etc.), Git excelle dans la gestion du versionnage.  

- **Pourquoi ?**  
  - Les différences entre versions se limitent souvent à quelques lignes modifiées, ce qui rend le stockage très léger.
  - Même pour des dépôts volumineux, l'empreinte mémoire reste faible, car seules les modifications (en kilooctets) sont sauvegardées.  

- **Exemple de fichiers à versionner :**  
  - Code source.  
  - Documentation en texte brut ou formatée (Markdown, LaTeX, etc.).  

---

#### Les fichiers binaires et compressés  

Pour des fichiers comme ceux de la suite Office ou LibreOffice, qui sont des fichiers compressés, le mécanisme différentiel de Git est moins efficace.  

- **Pourquoi ?**  
  - Ces fichiers ne permettent pas de détecter les modifications ligne par ligne, car toute modification peut entraîner un changement global du fichier.  
  - Chaque nouvelle version de ces fichiers est sauvegardée en entier, ce qui alourdit rapidement le dépôt.  

- **Conseils :**  
  - Si possible, privilégiez des formats texte équivalents (par exemple, `.csv` au lieu d'Excel pour les données).  
  - Évitez de versionner des fichiers binaires volumineux, sauf si c'est absolument nécessaire.  

---

#### Les fichiers générés automatiquement  

Les fichiers générés automatiquement par des outils ou lors de la compilation (comme les fichiers de log ou les dossiers `target` en Java) n'ont pas besoin d'être versionnés.  

- **Pourquoi ?**  
  - Ces fichiers peuvent être recréés à tout moment à partir des sources ou des outils de compilation.  
  - Versionner ces fichiers encombre inutilement le dépôt et crée des conflits inutiles.  

- **Exemple de fichiers à ignorer :**  
  - Dossiers de build (`target/`, `dist/`).  
  - Fichiers de log (`*.log`).  
  - Fichiers temporaires ou de sauvegarde (`*.tmp`, `*.bak`).  

---

#### Les fichiers contenant des données sensibles  

**Attention :** Une fois qu'un fichier est versionné, il reste accessible dans l'historique du dépôt, même s'il est supprimé dans des commits ultérieurs.  

- **Exemples de fichiers à ne pas versionner :**  
  - Mots de passe ou clés d’API.  
  - Informations personnelles ou confidentielles.  

- **Bonne pratique :**  
  - Stockez les secrets dans des fichiers spécifiques (comme `.env`) qui ne sont pas versionnés.  
  - Ajoutez ces fichiers à votre `.gitignore` pour éviter les erreurs.  

> Pour supprimer définitivement un fichier sensible de l’historique Git : [git-filter-repo](https://github.com/newren/git-filter-repo) ou [BFG Repo Cleaner](https://rtyley.github.io/bfg-repo-cleaner/).  

---

### Mettre en place des règles de versionnage avec `.gitignore`  

![](/images/git/gitignore.png)
Git permet de contrôler les fichiers inclus ou exclus du versionnage grâce à un fichier spécial nommé `.gitignore`, placé à la racine du dépôt.  

#### **Fonctionnement du fichier `.gitignore`**  
- Chaque ligne contient une expression qui correspond à *un ou plusieurs fichiers ou dossiers à ignorer*.  
- Exemple de contenu :  
  ```bash
  # Ignorer tous les fichiers temporaires
  *.tmp

  # Ignorer le dossier build
  /build/

  # Ignorer les fichiers de configuration locaux
  .env
  ```

- **Outil utile :**  
  Pour générer un fichier `.gitignore` adapté à votre technologie ou langage, utilisez [gitignore.io](https://www.toptal.com/developers/gitignore).  

> Consultez la documentation officielle pour plus d'informations : [Git Ignore](https://git-scm.com/docs/gitignore).  

---



## Travaux Pratiques

1. **Créer un compte GitHub** :  
   Si vous n'avez pas encore de compte, suivez [ce guide pour en créer un](https://docs.github.com/fr/get-started/start-your-journey/creating-an-account-on-github), puis connectez-vous.

2. **Forker le repository du TP Git** :  
   Rendez-vous sur le [repository du TP Git](https://github.com/conception-logicielle-ensai/tp_git) et forkez-le dans votre espace personnel (namespace). Un lien dans le cours explique comment réaliser un fork sur GitHub.

3. **Cloner votre fork en local** :  
   Récupérez une copie de votre fork sur votre machine locale.

4. **Lister les commits** :  
   Affichez les commits présents dans la branche `main` de votre repository.

5. **Modifier le README** :  
   Ajoutez une mention indiquant qu'il s'agit de votre fork du projet, directement via l'interface de GitHub.

6. **Ajouter un fichier `.gitignore`** :  
   Créez un fichier `.gitignore` dans votre repository. N'oubliez pas de synchroniser les modifications apportées au README avec votre dépôt local.

7. **Créer une branche pour une nouvelle fonctionnalité** :  
   Créez une nouvelle branche dédiée à l'ajout d'une fonctionnalité permettant de supprimer un livre.

8. **Implémenter la fonctionnalité** :  
   Suppression d'un livre de la base de données à partir de son identifiant

9. **Créer une nouvelle version** :  
   Validez vos modifications en créant un nouveau commit.

10. **Synchroniser avec le dépôt distant** :  
    Poussez vos modifications sur votre fork distant.

11. **Créer une nouvelle Pull Request**
    Via l'interface de GitHub créer une nouvelle Pull Request pour visualiser vos modifications.

12. **Fusionner votre branche avec `main`** :  
    Intégrez les changements de votre branche dans la branche `main`.

13. **(Bonus)** :  
    Envoyez votre code sur le GitLab du SSP Lab : [GitLab SSP Cloud](https://git.lab.sspcloud.fr/).
