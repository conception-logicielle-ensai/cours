---
title: "Conteneurs et environnements applicatifs"
weight: 11
draft: false
summary: "Découverte de la conteneurisation, portabilité et partage d'environnement packagés"
slug: "conteneurs-environnement-applicatifs"
tags: ["docker", "conteneurs","run","conteneurs-environnement-applicatifs","packaging"]
series: ["Cours"]
series_order: 14
---


> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/conteneurs-environnement-applicatifs

Le développement et le déploiement d'applications modernes posent des défis récurrents qui ont longtemps freiné le bon fonctionnement des projets. **"It works on my machine"** est devenu une phrase emblématique exprimant la frustration face aux différences d'environnements entre le poste de travail en local et des environnements où l'on héberge le code.

**Il s'agit aujourd'hui encore, de parler de portabilité applicative et de packaging sous le prisme de la mise a disposition d'environnement que l'on peut executer sur différents environnements de manière sécurisée**

La conteneurisation répond à ce problème en proposant une approche radicalement différente : plutôt que d'adapter l'application à chaque environnement, on encapsule l'application avec toutes ses dépendances dans une **unité standardisée** et **portable**. 

L'hébergement de ces conteneur de manière isolée **by design**, permet sereinnement de les déployer dans des environnements cloud publics et privés, a partir d'une allocation fixe de ressource.

Cela permet également un travail facilité en local, puisque l'on peut mettre en place tout un écosystème fonctionnel en parallèle du code sur lequel on travaille.

> C'est ce que nous allons voir dans cette partie


## **Idée générale**

L'objectif au sein des projets est de pouvoir créer un livrable unique qui permette de lancer notre application, préalablement configurée, afin de garantir son bon fonctionnement, quelle que soit la machine utilisée.

**C'est ce qu'on appelle la portabilité applicative.**  

### Lancement classique
Jusqu'à présent, pour exécuter une application Python sur une autre machine, plusieurs étapes sont nécessaires :  
1. Récupérer tous les fichiers `.py` du projet  
2. Ajouter les fichiers de configuration  
3. Disposer d'un environnement où Python est installé, dans une version compatible  
4. Installer toutes les dépendances du projet  
5. Lancer une ligne de commande pour démarrer l’application  

C'est assez long, et il y a des risques d'erreurs.  

**Docker** permet de créer un environnement préconfiguré contenant le code de l’application, toutes les dépendances déjà installées et une version de Python précisément définie  

### Utilisation de la conteneurisation, exemple avec Docker

Docker est une plateforme de conteneurisation qui a popularisé et démocratisé l'utilisation des conteneurs Linux. 

Docker permet de **packager** une application avec **toutes ses dépendances** dans une unité standardisée appelée **conteneur**, et à fournir des **outils** simples pour créer, distribuer et exécuter ces conteneurs.

> En quelque sorte : Docker partage certaines similitudes avec les gestionnaires de paquets dans sa capacité à distribuer des logiciels de manière standardisée. 

Avec Docker, l'utilisateur n’a plus qu’à :  
1. Avoir Docker installé sur sa machine  
2. Télécharger l’image Docker de l'application  
3. Lancer cette image  

#### **Une image Docker, c’est quoi ?**  
Une **image Docker**,  c'est un énorme zip qui contient qui contient tout le nécessaire pour exécuter une application dans un environnement contrôlé. C'est une forme d’**Infrastructure as Code**, garantissant que si l’application fonctionne sur une machine, elle fonctionnera sur une autre (tant que Docker est disponible).

Elle contient donc : 
- Le système d'exploitation nécessaire au processus
- Les bibliothèques systèmes : fonction noyaux utilisées par le code, exemple pour la lecture de fichier 

> Pour aller plus loin sur les appels système et leur rôle : https://man7.org/linux/man-pages/man2/syscalls.2.html


## Conteneurs : Concepts clés

L'introduction de l'isolation des processus, mise en place en 2006, a permis de mieux gérer les interactions entre les différentes tâches d'un système. 
Cette avancée a transformé notre approche de la livraison des logiciels. Désormais, nous pouvons proposer des modules qui fonctionnent de manière isolée du reste du système qui les héberge, tout en exécutant des traitements dans des environnements contrôlés.

![conteneur-runtime](/images/conteneurs-environnement-applicatifs/cgroupsnamespace.png)
  
Le terme **container runtime** désigne le logiciel responsable de la gestion des conteneurs, permettant d’exécuter des applications de manière isolée sur un même système d’exploitation.  

1. **Kernel :**  
Les conteneurs partagent le même **système d'exploitation** (OS) mais hébergent des **processus indépendants**.
Chaque conteneur fonctionne comme une entité autonome, mais ils utilisent tous le même noyau de l'hôte.  

2. **Namespaces :**  
Introduits en 2002, ils permettent d’isoler les processus en créant des environnements virtuels distincts. 
Par exemple, un conteneur peut avoir son propre espace réseau, son système de fichiers et ses processus, limitant ainsi l'interaction avec d'autres conteneurs. 

3. **Cgroups (Control Groups) :**  
Introduits en 2006, ils permettent de **restreindre l'accès à certaines ressources** (CPU, mémoire, I/O, etc.) pour chaque conteneur. 
Cela garantit que les conteneurs ne consomment pas plus de ressources que prévu, évitant ainsi que des applications malveillantes ou défaillantes n'affectent les autres.  

**Évolution de l'hébergement des applications**  
- **Avant :** Chaque application nécessitait une machine physique dédiée. Cela était coûteux et inefficace.  
- **Ensuite :** L’introduction des machines virtuelles a permis d'héberger plusieurs applications sur une même machine, mais cela nécessitait beaucoup de ressources pour chaque VM, notamment en termes de CPU et de mémoire.  
- **Maintenant :** Avec les conteneurs, il est possible d'installer un moteur d'isolation sur une seule machine pour lancer plusieurs applications indépendamment, tout en utilisant efficacement les ressources disponibles.  

> Remarque: Les conteneurs reposent sur le noyau de leur hôte, pour ce mécanisme d'isolation des processus et donc pour certaines fonctions, ils n'ont pas de noyau propre.


## Docker : **Image Docker**

![conteneur-image](/images/conteneurs-environnement-applicatifs/docker-layers.jpg)

> Un peu plus concrètement cette fois

Une image Docker est un paquet autonome qui contient tout le nécessaire pour exécuter une application. Elle sert de modèle pour créer des conteneurs, qui sont des instances en cours d’exécution de cette image. 

Une image Docker comprend :

- Le code source de l'application
- Les dépendances requises
- Une version spécifique de Python (ou d'un autre environnement)
- Le système de fichiers et les configurations nécessaires

On peut considérer l’image Docker comme un **modèle figé** qui facilite le déploiement d'un environnement de travail prêt à l'emploi, sans configuration manuelle.

Pour illustrer cette notion, on peut faire une analogie avec la cuisine : 

- **Une image Docker** est comme une recette de cuisine, définissant les ingrédients et les étapes à suivre.
- **Un conteneur** représente le plat préparé à partir de cette recette, c'est-à-dire l'exécution concrète de l'image.

Une image sert de modèle ou de plan pour créer un environnement d'exécution. Cela signifie qu'elle contient tout ce dont un logiciel a besoin pour fonctionner correctement, comme le système d'exploitation, les bibliothèques et les dépendances. Quand on utilise cette image, on peut facilement déployer un environnement de travail qui est prêt à l'emploi, sans avoir à configurer chaque élément manuellement.

![autre](/images/conteneurs-environnement-applicatifs/dockerimage.webp)

> Pour information, on manipule des images que l'on peut partager sur des registres d'image. 
> Le plus connu et le plus classique (équivalent a Pypi pour docker) est dockerhub: https://hub.docker.com/.

<div class="alert alert-info">
  <strong>Pour aller plus loin :</strong> <br/> 
  <a href="https://www.ionos.fr/digitalguide/serveur/know-how/les-images-docker/" target="_blank">Les images Docker</a>
</div>


## Docker: Définition d'une image, le Dockerfile


![dockerfile](/images/conteneurs-environnement-applicatifs/dockerfile.png)


La construction des images se fait en couches (layers) : imaginez chaque image comme une couche d'un gâteau, où certaines couches peuvent être considérées comme "parents" et d'autres comme "enfants". 

Cela signifie qu'une image peut être basée sur une autre, créant ainsi une hiérarchie. Par exemple, une image parent peut contenir des configurations ou des bibliothèques de base, tandis que les images enfants ajoutent des fonctionnalités spécifiques.

Pour définir **la recette** menant à un environnement à partir d'une installation Linux de base, similaire à un poste sans aucune configuration, Docker permet de créer un fichier appelé `Dockerfile`.

Le Dockerfile définit les étapes de construction du livrable **image Docker**, ce livrable doit contenir tout le nécessaire pour lancer une application et également définir la commande au lancement de l'image.

**Cela revient donc à automatiser le lancement de votre application dans un script, avec la partie installation d'un système depuis un état (ici on part par exemple d'un ubuntu vierge)**

### Exemples de Dockerfile

<div class="dockerfile-multi-builder">
  <style>
    .dockerfile-multi-builder {
      font-family: monospace;
      width: 680px;
      background: #020617;
      color: #e5e7eb;
      border: 1px solid #334155;
      border-radius: 10px;
    }
    .df-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 16px;
      border-bottom: 1px solid #1e293b;
    }
    .df-header span {
      font-weight: bold;
      color: #38bdf8;
    }
    .select-df {
      background: #020617;
      color: #e5e7eb;
      border: 1px solid #334155;
      border-radius: 6px;
      padding: 4px 8px;
    }
    .df-content {
      padding: 16px;
      min-height: 220px;
      white-space: pre-wrap;
      line-height: 1.5;
    }
    .df-controls {
      display: flex;
      justify-content: space-between;
      padding: 12px;
      border-top: 1px solid #1e293b;
    }
    .df-button {
      background: #020617;
      color: #e5e7eb;
      border: 1px solid #334155;
      border-radius: 6px;
      padding: 6px 12px;
      cursor: pointer;
    }
    .df-button:disabled {
      opacity: 0.4;
      cursor: not-allowed;
    }
    .final {
      color: #22c55e;
    }
    /* --- Dockerfile syntax highlighting --- */
.df-keyword {
  color: #38bdf8;
  font-weight: bold;
}

.df-comment {
  color: #64748b;
  font-style: italic;
}

.df-string {
  color: #22c55e;
}

.df-flag {
  color: #facc15;
}

.df-command {
  color: #e879f9;
}

  </style>

  <div class="df-header">
    <span id="df-title">Étape 1</span>
    <select id="variant" class="select-df">
          <option value="uv">Python – UV</option>
      <option value="pip" selected>Python – PIP</option>
      <option value="npm">Node – NPM</option>
    </select>
  </div>

  <div class="df-content" id="df-content"></div>

  <div class="df-controls">
    <button class="df-button" id="prev">← Précédent</button>
    <button class="df-button" id="next">Suivant →</button>
  </div>

  <script>
    (function () {
      const data = {
        pip: [
          `# Étape 1
FROM python:3.13.12-trixie
# on part d'une image debian avec python3.13 d'installé`,
          `# Étape 2
FROM python:3.13.12-trixie
COPY . . # on copie les fichiers de notre projet
# on met les fichiers qui se trouvent dans le repertoire (donc notre projet) dans l'image`,
          `# Étape 3
FROM python:3.13.12-trixie
COPY . .
RUN pip install -r requirements.txt
# on installe les dépendances`,
          `# FICHIER FINAL
FROM python:3.13.12-trixie
COPY . .
RUN pip install -r requirements.txt
ENTRYPOINT ["python", "main.py"]
# on choisit la commande lancée au démarrage`
        ],

        uv: [
          `# Étape 1
FROM astral/uv:python3.13-trixie # on part d'une image qui a UV, Python3.13 et un OS Debian`,
          `# Étape 2
FROM astral/uv:python3.13-trixie
COPY . . 
# on met nos fichiers dans l'image`,
          `# Étape 3
FROM astral/uv:python3.13-trixie
COPY . . 
RUN uv sync 
# on télécharge les dépendances dans l'environnement `,
          `# FICHIER FINAL
FROM astral/uv:python3.13-trixie
COPY . . 
RUN uv sync 
ENTRYPOINT ["uv", "run","main.py"] 
# on lance le code avec la commande uv run`
        ],

        npm: [
          `# Étape 1
FROM node:lts-trixie 
# on prend une image debian (cousin ubuntu) avec npm`,
          `# Étape 2
FROM node:lts-trixie
WORKDIR /app 
# on met un repertoire comme repertoire de base de notre application dans l'image`,
          `# Étape 3
FROM node:lts-trixie
WORKDIR /app
COPY . . 
# on copie tout le code source dans l'image`,
          `# Étape 4
FROM node:lts-trixie
WORKDIR /app
COPY . . 
RUN npm install 
# on installe toutes les dépendances`,
          `# FICHIER FINAL
FROM node:lts-trixie
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "run", "dev","--", "--host"] 
# on lance le serveur vite avec l'option --host pour rendre le service accessible par l'exterieur du conteneur`
        ]
      };

      let variant = "pip";
      let step = 0;

      const title = document.getElementById("df-title");
      const content = document.getElementById("df-content");
      const prev = document.getElementById("prev");
      const next = document.getElementById("next");
      const select = document.getElementById("variant");

      function render() {
        const steps = data[variant];
        const isFinal = step === steps.length - 1;

        title.textContent = isFinal ? "FICHIER FINAL" : `Étape ${step + 1}`;
        title.className = isFinal ? "final" : "";
        content.textContent = steps[step];

        prev.disabled = step === 0;
        next.disabled = isFinal;
      }

      prev.onclick = () => {
        if (step > 0) step--;
        render();
      };

      next.onclick = () => {
        if (step < data[variant].length - 1) step++;
        render();
      };

      select.onchange = (e) => {
        variant = e.target.value;
        step = 0;
        render();
      };

      render();
    })();
  </script>
</div>



Le `Dockerfile` est un fichier texte avec une syntaxe spécifique qui permet de construire des images Docker. Il décrit l'ensemble des opérations à effectuer sur l'environnement de build, incluant la personnalisation de l'environnement de base (avec la commande **FROM**) et les paramètres de lancement. Par défaut, il est courant de définir l'**ENTRYPOINT** dans le Dockerfile, ce qui détermine la commande à exécuter lors du démarrage du conteneur.

🧠 Les keywords ici employés sont les suivants : 

| Keyword     | Build | Runtime | Usage principal        |
|------------|-------|---------|------------------------|
| FROM       | ✅    | ❌      | Image de base          |
| RUN        | ✅    | ❌      | On lance des commandes |
| COPY       | ✅    | ❌      | On met des fichiers dans l'image  |
| WORKDIR   | ✅    | ❌      | Définir le contexte (chemin par défaut / dossier par défaut)   |
| ENTRYPOINT| ❌    | ✅      | Commande executée par l'image sans options        |
| CMD        | ❌    | ✅      | Commande par défaut avec args par défaut, remplaçable  |


> Remarque : Dans le monde des conteneurs, tout est un processus, et chaque processus doit être conçu sans état interne. Cela signifie que les conteneurs ne doivent pas stocker d'informations qui pourraient être perdues lorsque le conteneur est arrêté ou redémarré. Par conséquent, il est recommandé de concevoir des applications conteneurisées de manière à ce qu'elles puissent fonctionner sans dépendre d'un état persistant.

<div class="alert alert-info">
  <strong>Pour aller plus loin :</strong> <br/> 
  <a href="https://docs.docker.com/reference/dockerfile/" target="_blank">Liste des instruction possible dans un Dockerfile</a>
</div>

### Construire un Dockerfile

Pour construire un Dockerfile, vous devez arriver a théoriser toutes les étapes qui mènent d'un environnement vierge (ubuntu) ou préconfiguré (environnement [python](https://hub.docker.com/_/python/tags) ou [uv](https://hub.docker.com/r/astral/uv/tags))

- Vous aurez à mettre en place les dépendances : python  bien installé, librairies dans l'environnement ou dans le venv
- Vous aurez nécessairement a mettre vos fichiers dans l'image via **COPY**.
- Vous devrez définir une commande de démarrage cohérente par rapport a l'emplacement des fichiers dans l'image


## Quelques commandes

![dockercommands](/images/conteneurs-environnement-applicatifs/docker-commands.png)

> Image src:  https://kinsta.com/blog/docker-commands/


**Remarque Importante**: la CLI *docker* s'execute en administrateur (root), il faudra donc précéder vos commandes de **sudo** ou vous mettre en administrateur **sudo su** puis **exit** quand vous aurez fini.


### Lancement d'une image 
Pour exécuter une image Docker, utilisez la commande suivante :

```
sudo docker run <image> <options>
```

**Exemples :**

Pour exécuter l'image `hello-world` :

```
sudo docker run hello-world
```

Pour lancer une instance de PostgreSQL en arrière-plan avec une base de données locale, vous pouvez utiliser :

```
sudo docker run -d postgres -p 5432:5432
```

Dans cet exemple, l'option `-d` permet de lancer le conteneur en arrière-plan, tandis que `-p 5432:5432` rend PostgreSQL accessible sur `localhost:5432`.

### Suivi des images qui tournent actuellement
```
sudo docker ps
```

> Liste les processus docker a partir du daemon qui est sur votre machine
### Suppression des images

```
sudo docker kill $PID
```

> En remplaçant $PID par l'id du conteneur

### Interaction avec un conteneur

```
sudo docker logs $PID
```
> Voir les logs du traitement

```
sudo docker exec $PID $COMMANDE
```
> Executer une commande sur le conteneur

```
sudo docker exec -it $PID bash
```

> Aller dans le conteneur pour "débuguer"

### Création d'une image à partir d'un Dockerfile

Pour construire une image Docker à partir d'un Dockerfile, utilisez la commande suivante :

```
sudo docker build -t <nomdelimage:versiondelimage> <cheminversledossiercontenantdockerfile>
```

**Exemple :**

Pour créer une image nommée `openfoodapi` avec la version `1.0.0`, exécutez :

```
sudo docker build -t openfoodapi:1.0.0 .
```

Ici, le point `.` indique que le Dockerfile se trouve à la racine du répertoire courant.

## Docker push : Déploiement d'une image pour la partager

Pour déployer une image sur un registre d'images, comme DockerHub, vous devez suivre ces étapes :

- Avoir un compte valide sur DockerHub.
- Vous connecter en ligne de commande à DockerHub via l'outil Docker.
- Ajouter un tag à l'image pour spécifier le chemin vers le registre de destination.

**Concrètement**

1. S'authentifier sur dockerhub (ou un autre registre)
Utilisez la commande docker login pour vous authentifier auprès de DockerHub :
```sh
sudo docker login
```

2. Préparation de l'image, en la taguant avec le nom cible

> Avant de pousser une image, vous devez la taguer avec le bon format : nom_utilisateur/nom_image:tag. Le tag permet de spécifier une version (par exemple latest, v1.0, prod).

La dernière par défaut :
```sh
sudo docker tag mon-application:latest mon_utilisateur/mon-application:latest
```

> latest est un nom canonique qui correspond a la **dernière version** de l'image sur le dépôt

Ou sinon avec une version fixée:
```sh
sudo docker tag mon-application:latest mon_utilisateur/mon-application:v1.0
```

3. Puis envoi de l'image, si vous êtes bien authentifiés

```sh
sudo docker push mon_utilisateur/mon-application:latest
```
Pour plus d'informations, consultez [la documentation de Docker](https://docs.docker.com/get-started/introduction/build-and-push-first-image/).

4. Maintenant elle devient utilisable par d'autres personnes
```sh
sudo docker run mon_utilisateur/mon-application:latest
```

## Automatisation via CI/CD et GitHub Actions

![reproductible](/images/conteneurs-environnement-applicatifs/cicd.webp)


Cette opération est en général très fastidieuse et a faible valeur ajoutée pour vous, donc on a envie de l'automatiser.
De plus on aime mettre en place des versions de manière régulière et cohérentes par rapport à un dépôt de code, ce qui nous permet de repartir très facilement d'une version fonctionnelle de l'application dans l'histoire du dépôt Git.

C'est courant a partir d'un dépôt Git de mettre en place une routine de déploiement automatique vers Dockerhub ou d'autres registres, pour rendre notre application utilisable par tous les environnements (postes et serveurs cloud).

> Rappel: la présentation de Github Actions était dans la partie de cours sur l'automatisation [ici](/cours/automatisation/)

Des actions sont déjà faites et prêtes a l'emploi pour construire une image à partir d'un dépôt. Elles permettent de paramétrer des jobs automatiques qui vont effectuer les Docker build et push au sein de machines virtuelles (**QeMu**)


Nous vous proposons dans le cadre de ce cours de s'intéresser à celle ci :
- https://github.com/marketplace/actions/build-and-push-docker-images#git-context

Voici une proposition de configuration pour vos projets :

**REMARQUE IMPORTANTE: IL FAUDRA CHANGER LE CHEMIN VERS LE DOCKERFILE ET LE NOM DE L'APPLI (backend)**

```yaml
name: deploiement-dockerhub-main

on:
  push:
    branches:
      - main  

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v6
        # voir : https://github.com/marketplace/actions/build-and-push-docker-images#customizing
        with:
          context: "{{defaultContext}}" # Répertoire de contexte (racine du repo par défaut)
          file: ./Dockerfile  # Chemin vers le Dockerfile
          push: true
          tags: ${{ vars.DOCKERHUB_USERNAME }}/${{ vars.BACKEND_APPLICATION }}:latest
```

Et pour publier quand vous faites des  tags git
```yaml
name: deploiement-dockerhub-tags

on:
  push:
    tags:
      - '*'  # se déclenche quand vous faiinitiation-docker-kube-postgrestes des tags via git (soit dans l'interface soit en pushant le tag)
jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: "{{defaultContext}}" # Répertoire de contexte (racine du repo par défaut)
          file: ./Dockerfile
          push: true
          tags: ${{ vars.DOCKERHUB_USERNAME }}/${{ vars.BACKEND_APPLICATION }}:${{ github.ref_name }}

```

### Configuration préalable : générer un jeton et l'apposer sur un projet

On va devoir définir des secrets sur notre projet github pour déposer la clé de déploiement vers Dockerhub.

- Donc sur Dockerhub:
1) aller dans les paramètres du compte `Account settings` 
2) aller dans le volet `Personal access tokens`
3) Generate new token en READ & WRITE 
4) vous obtenez un password qui ressemble à `dckr_pat_xxxxxxxxxxxxx` et vous avez un rappel de votre login `user`

- Sur Github:
1) Allez sur votre projet
2) Allez dans les `Settings`
3) Allez dans l'onglet `Secrets and variables`

- Dans secrets : Ajoutez un secret au name : `DOCKERHUB_TOKEN` et dans `secret` mettre le jeton
- Dans variables, ajoutez `DOCKERHUB_USERNAME` avec en value votre identité sur dockerhub
 


## Environnement local reproductible : vers Docker Compose

![reproductible](/images/conteneurs-environnement-applicatifs/docker_compose_logo.jpeg)

### Docker pour consolider l'environnement de travail


Docker permet de lancer différents services en customisant leurs variables d'environnement, leur accès aux données via des volumes, et leur exposition en local via des ports. Cette approche présente plusieurs avantages pour le développement en local.

L'un des cas d'usage les plus courants est de travailler avec des bases de données sans avoir à les installer directement sur sa machine. Par exemple, plutôt que d'installer PostgreSQL, MySQL ou MongoDB en local avec toutes les configurations que cela implique, on peut simplement lancer un conteneur Docker. 

On peut ensuite s'y connecter avec des outils comme DBeaver, pgAdmin ou tout autre client de base de données, exactement comme si la base était installée localement.Cette approche évite les conflits de versions entre projets et garantit que chaque membre de l'équipe travaille avec exactement la même configuration de services. De plus, nettoyer son environnement devient aussi simple que supprimer un conteneur.

```sh
docker run --name postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=postgres \
  -p 5432:5432 \
  -v postgres_data:/var/lib/postgresql/data \
  -d postgres:18
```
Options principales :

- `-e` pour **environnement** : définit les variables d'environnement du conteneur
- `-p` pour le **tunnel réseau** entre le port de votre machine et du conteneur : `MON_PORT:SON_PORT`
- `-v` pour l'utilisation d'un répertoire pour stocker ses fichiers (partager du disque avec le conteneur)
- `-d` pour le lancer en tâche de fond (detached mode)

Autre exemple avec une base de données mongodb:
```sh
docker run --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=mongo \
  -e MONGO_INITDB_ROOT_PASSWORD=mongo \
  -p 27017:27017 \
  -v mongo_data:/data/db \
  -d mongo:8
```


**Exemple pratique : connexion à PostgreSQL depuis DBeaver**

Installation
```
sudo snap install dbeaver-ce --classic
```

Une fois le conteneur PostgreSQL lancé avec la commande ci-dessus, vous pouvez vous y connecter depuis DBeaver avec les paramètres suivants :
- **Host :** localhost
- **Port :** 5432
- **Database :** postgres
- **User :** postgres
- **Password :** postgres

Le conteneur se comporte exactement comme une installation locale, mais reste complètement isolé dans son environnement Docker.

### Docker compose

*Docker Compose* permet de définir et orchestrer plusieurs conteneurs ensemble. Au lieu de lancer manuellement chaque service (backend, frontend, base de données), vous décrivez tout dans un fichier docker-compose.yml et lancez l'ensemble d'une seule commande.

Cela a surtout du sens dans des projets qui commencent à avoir plus de 1 module et avec des dépendances tierces (base de données..).

Docker Compose fonctionne avec un fichier dédié décrit en `yml`, il peut par exemple être placé a la racine du projet :
```
mon-projet/
├── backend/
│   ├── Dockerfile
│   └── ...
├── frontend/
│   ├── Dockerfile
│   └── ...
└── docker-compose.yml
```


#### Exemple de fichier docker-compose.yml
Docker-compose reprend des notions de Docker dans des fichiers de configuration yaml, passons par un exemple: 

```yaml
services:
  db:
    image: postgres:18
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    volumes:
      - postgres_data:/var/lib/postgresql
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    environment:
      DATABASE_URL: postgresql://postgres:postgres@postgres:5432/postgres
    ports:
      - "8000:8000"
    depends_on:
      - db
    volumes:
      - ./backend:/app

  frontend:
    build: ./frontend
    ports:
      - "5173:5173"
    depends_on:
      - backend
    volumes:
      - ./frontend:/app

volumes:
  postgres_data:
```

> C'est l'exemple du projet [archi-exemple]()
Ce ne sera pas le coeur du cours, mais si vous souhaitez configurer un environnement de travail en local propre pour les développeurs, c'est une très bonne initiative.

#### Aide à l'installation et utilisation

**Installation**

Suivre les quelques lignes ici : https://docs.docker.com/compose/install/linux/#install-using-the-repository

**Utilisation** : quelques commandes

```sh
sudo docker compose up
```

> Lire un fichier docker-compose.yml et l'appliquer (lancer les conteneurs et les mettres en relation)

Vous pouvez l'appliquer partiellement, exemple **travail sur l'UI** :
```sh
sudo docker compose up db backend
```
Observer les erreurs et messages dans les logs des services installés : 
```sh
sudo docker compose logs -f
```

Arrêter les conteneurs :

```sh
sudo docker compose down
```


## Travaux Pratiques : Mise en place d'une image Docker pour votre application

**L'objectif de ce TP est de créer une image Docker pour votre propre projet.**

À partir d'un projet fonctionnel, vous pouvez définir une image Docker pour l'exécuter dans un environnement isolé :

1. Créez un compte sur dockerhub: https://hub.docker.com/
2. Nous vous avons installé linux selon ce mode opératoire : https://www.docker.com/get-started/, allez y jeter un oeil.
3. Vérifiez que l'installation a réussi en exécutant la commande suivante : `sudo docker run hello-world`.
4. Consultez les exemples pour vous faire une idée des Dockerfile que vous pourriez construire [Lien vers les exemples](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/conteneurs-environnement-applicatifs)
5. Créez un fichier Dockerfile et essayez de construire localement l'image Python en utilisant la commande : `sudo docker build -t monimage .`.
6. Taguez votre image et publiez-la en suivant le tutoriel mentionné précédemment.
7. Mettez en place l'automatisation via GitHub Actions en consultant cette 
ressource : https://github.com/marketplace/actions/build-and-push-docker-images.

8. (bonus) Mettez en place une configuration docker compose sur votre projet pour faciliter le travail en local des membres de l'équipe