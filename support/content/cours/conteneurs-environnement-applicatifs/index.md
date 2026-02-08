---
title: "Conteneurs et environnements applicatifs"
weight: 11
draft: false
summary: "Découverte de la conteneurisation, portabilité et partage d'environnement packagés"
slug: "conteneurs-environnement-applicatifs"
tags: ["docker", "conteneurs","run","portabilite","packaging"]
series: ["Cours"]
series_order: 14
---


### **Idée générale** 

L'objectif est de pouvoir créer un livrable unique qui permette de lancer notre application, préalablement configurée, afin de garantir son bon fonctionnement, quelle que soit la machine utilisée.
**C'est ce qu'on appelle la portabilité applicative.**  

Jusqu'à présent, pour exécuter une application Python sur une autre machine, plusieurs étapes sont nécessaires :  
1. Récupérer tous les fichiers `.py` du projet  
2. Ajouter les fichiers de configuration  
3. Disposer d'un environnement où Python est installé, dans une version compatible  
4. Installer toutes les dépendances du projet  
5. Lancer une ligne de commande pour démarrer l’application  

C'est assez long, et il y a des risques d'erreurs.  

**Docker** permet de créer un environnement préconfiguré contenant le code de l’application, toutes les dépendances déjà installées et une version de Python précisément définie  

Avec Docker, l'utilisateur n’a plus qu’à :  
1. Avoir Docker installé sur sa machine  
2. Télécharger l’image Docker du projet  
3. Lancer cette image  

**Une image Docker, c’est quoi ?**  
Une **image Docker**,  c'est un énorme zip qui contient qui contient tout le nécessaire pour exécuter une application dans un environnement contrôlé. C'est une forme d’**Infrastructure as Code**, garantissant que si l’application fonctionne sur une machine, elle fonctionnera sur une autre (tant que Docker est disponible).  

> **Ressources :** Les fichiers présentés sur cette slide sont disponibles sur le dépôt GitHub :  
>[🔗 Cours 6 - Exemples](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-6)  

## Conteneurs : Concepts clés

L'introduction de l'isolation des processus, mise en place en 2006, a permis de mieux gérer les interactions entre les différentes tâches d'un système. 
Cette avancée a transformé notre approche de la livraison des logiciels. Désormais, nous pouvons proposer des modules qui fonctionnent de manière isolée du reste du système qui les héberge, tout en exécutant des traitements dans des environnements contrôlés.

![conteneur-runtime](/images/portabilite/cgroupsnamespace.png)
  
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


## **Image Docker**

![conteneur-image](/images/portabilite/docker-layers.jpg)

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

![autre](/images/portabilite/dockerimage.webp)

> Pour information, on manipule des images que l'on peut partager sur des registres d'image. 
> Le plus connu et le plus classique (équivalent a Pypi pour docker) est dockerhub: https://hub.docker.com/.

<div class="alert alert-info">
  <strong>Pour aller plus loin :</strong> <br/> 
  <a href="https://www.ionos.fr/digitalguide/serveur/know-how/les-images-docker/" target="_blank">Les images Docker</a>
</div>


## Dockerfile

La construction des images se fait en couches (layers) : imagine chaque image comme une couche d'un gâteau, où certaines couches peuvent être considérées comme "parents" et d'autres comme "enfants". Cela signifie qu'une image peut être basée sur une autre, créant ainsi une hiérarchie. Par exemple, une image parent peut contenir des configurations ou des bibliothèques de base, tandis que les images enfants ajoutent des fonctionnalités spécifiques.

Pour définir **la recette** menant à un environnement à partir d'une installation Linux de base, similaire à un poste sans aucune configuration, Docker permet de créer un fichier appelé `Dockerfile`.

Voici un exemple de Dockerfile pour un environnement Python basé sur Ubuntu 22.04 :

```Dockerfile
# On part d'un environnement Ubuntu 22.04
FROM ubuntu:22.04
# On installe Python et pip
RUN apt-get update && apt-get install -y python3-pip
# On copie les fichiers de notre projet dans l'environnement
COPY . .
# On installe les dépendances à partir du fichier requirements.txt
RUN pip install -r requirements.txt
# On définit la commande à exécuter pour lancer notre application
ENTRYPOINT ["python", "main.py"]
```

Le `Dockerfile` est un fichier texte avec une syntaxe spécifique qui permet de construire des images Docker. Il décrit l'ensemble des opérations à effectuer sur l'environnement de build, incluant la personnalisation de l'environnement de base (avec la commande **FROM**) et les paramètres de lancement. Par défaut, il est courant de définir l'**ENTRYPOINT** dans le Dockerfile, ce qui détermine la commande à exécuter lors du démarrage du conteneur.

> Remarque : Dans le monde des conteneurs, tout est un processus, et chaque processus doit être conçu sans état interne. Cela signifie que les conteneurs ne doivent pas stocker d'informations qui pourraient être perdues lorsque le conteneur est arrêté ou redémarré. Par conséquent, il est recommandé de concevoir des applications conteneurisées de manière à ce qu'elles puissent fonctionner sans dépendre d'un état persistant.

<div class="alert alert-info">
  <strong>Pour aller plus loin :</strong> <br/> 
  <a href="https://docs.docker.com/reference/dockerfile/" target="_blank">Liste des instruction possible dans un Dockerfile</a>
</div>


## Quelques commandes

![dockerfile](/images/portabilite/dockerfile.png)

### Lancement d'une image 
Pour exécuter une image Docker, utilisez la commande suivante :

```
docker run <image> <options>
```

**Exemples :**

Pour exécuter l'image `hello-world` :

```
docker run hello-world
```

Pour lancer une instance de PostgreSQL en arrière-plan avec une base de données locale, vous pouvez utiliser :

```
docker run -d postgres -p 5432:5432
```

Dans cet exemple, l'option `-d` permet de lancer le conteneur en arrière-plan, tandis que `-p 5432:5432` rend PostgreSQL accessible sur `localhost:5432`.

### Création d'une image à partir d'un Dockerfile

Pour construire une image Docker à partir d'un Dockerfile, utilisez la commande suivante :

```
docker build -t <nomdelimage:versiondelimage> <cheminversledossiercontenantdockerfile>
```

**Exemple :**

Pour créer une image nommée `openfoodapi` avec la version `1.0.0`, exécutez :

```
docker build -t openfoodapi:1.0.0 .
```

Ici, le point `.` indique que le Dockerfile se trouve à la racine du répertoire courant.

## Déploiement

Pour déployer une image sur un registre d'images, comme DockerHub, vous devez suivre ces étapes :

- Avoir un compte valide sur DockerHub.
- Vous connecter en ligne de commande à DockerHub via l'outil Docker.
- Ajouter un tag à l'image pour spécifier le chemin vers le registre de destination.

Pour plus d'informations, consultez [la documentation de Docker](https://docs.docker.com/get-started/introduction/build-and-push-first-image/).

## Travaux Pratiques 

**L'objectif de ce TP est de créer une image Docker pour un projet fonctionnel, tel que votre propre projet.**

À partir d'un projet fonctionnel, vous pouvez définir une image Docker pour l'exécuter dans un environnement isolé :

1. Installez Docker en suivant les instructions disponibles [ici](/docs/prise-en-main-linux/).
2. Vérifiez que l'installation a réussi en exécutant la commande suivante : `docker run hello-world`.
3. Créez un fichier Dockerfile et essayez de construire localement l'image Python en utilisant la commande : `docker build -t monimage .`.
4. Taguez votre image et publiez-la en suivant le tutoriel mentionné précédemment.
5. Mettez en place l'automatisation via GitHub Actions en consultant cette ressource : https://github.com/marketplace/actions/build-and-push-docker-images.