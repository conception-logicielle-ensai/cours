---
title: "Déploiement avec Kubernetes"
weight: 10
draft: false
summary: "Déploiement d'une application sur le cloud via Kubernetess"
slug: "kubernetes"
tags: ["deploiement", "kubernetes", "cloud"]
series: ["Cours"]
series_order: 9
---

<img src="https://kubernetes.io/images/kubernetes-open-graph.png" />

## Contexte

Cette section présente les enjeux et les principes fondamentaux du déploiement applicatif, à travers un cas concret d’hébergement dans le cloud reposant sur **Kubernetes**.

Les exemples s’appuient sur :

* une interface utilisateur (frontend) accessible à l’adresse suivante :
  [https://frontend-conception-logicielle.kub.sspcloud.fr/](https://frontend-conception-logicielle.kub.sspcloud.fr/)

* une API (backend) accessible à l’adresse suivante :
  [https://backend-conception-logicielle.kub.sspcloud.fr](https://backend-conception-logicielle.kub.sspcloud.fr)

> Le code source complet de l’application est disponible dans le dépôt GitHub du projet **`archi-exemple`** :
> [https://github.com/conception-logicielle-ensai/archi-exemple](https://github.com/conception-logicielle-ensai/archi-exemple)


## Préambule : hébergement traditionnel d’une application

<img src="https://cense.fr/wp-content/uploads/2020/04/hebergement-serveur--768x656.jpg" />

Avant d’aborder ce que permet Kubernetes, il est nécessaire de rappeler ce que signifie, plus généralement, **héberger une application ou un site web**.

### Environnement d’exécution

Toute application nécessite un **environnement d’exécution** : il s’agit du support matériel et logiciel sur lequel elle peut être lancée.

Cela comprend notamment :
* l’installation des dépendances (langages, bibliothèques, frameworks) ;
* la configuration des services nécessaires ;
* la gestion des fichiers applicatifs.

*Cet environnement peut prendre différentes formes, par exemple :*
* *un ordinateur personnel sur lequel Python est installé ;*
* *un serveur physique sur lequel est installé un moteur Docker, permettant d’exécuter des applications sous forme de conteneurs.*

### Routage et exposition de l’application

Une fois l’application en cours d’exécution dans son environnement, il faut la rendre **accessible depuis l’extérieur**, c’est-à-dire via Internet ou un réseau.

Pour cela, il est nécessaire de comprendre comment fonctionne l’accès à une machine à partir d’une URL, par exemple :

* [https://google.com](https://google.com)

Des **serveurs DNS** (Domain Name System) assurent la **correspondance entre les noms de domaine et des adresses IP**, ces dernières étant difficilement mémorisables pour un utilisateur.

Les adresses IP pointent vers ce que l’on appelle, selon les cas, un **serveur**, un **load balancer**, un **cluster** ou une **machine virtuelle**. En simplifiant, il s’agit d’une adresse publique permettant d’atteindre votre service.

Enfin, la machine ciblée doit autoriser les connexions sur certains ports réseau : par défaut, ceux-ci sont fermés et doivent être explicitement ouverts.


Quand vous tapez une URL :

```
https://frontend-conception-logicielle.kub.sspcloud.fr
```

le déroulement est :

1. votre ordinateur → demande :
   « Quelle est l’IP de ce nom ? »
2. ➜ envoyée au **DNS récursif configuré localement**
3. ce DNS contacte successivement :
   * les serveurs racine ;
   * les serveurs du TLD `.fr` ;
   * les serveurs autoritaires de `sspcloud.fr` ;
4. il récupère l’IP finale ;
5. il vous la renvoie ;
6. votre navigateur peut ouvrir la connexion réseau.

**Comment savoir quel DNS votre machine utilise ?**

Sur macOS / Linux :

```bash
scutil --dns
```


### En résumé

Une fois l’application installée, elle doit être accessible aux utilisateurs via le réseau.

Cela implique :
* la configuration d’un serveur applicatif (par exemple Uvicorn pour une API Python) ou d’un serveur web statique (dans ce cours, nous utiliserons Vite pour simplifier) ;
* l’attribution d’une adresse IP publique ;
* l’ouverture des ports nécessaires afin d’autoriser la communication avec les clients.

Ces réglages relèvent de l’environnement d’exécution et de sa configuration réseau ; on parle parfois, par extension, de **pare-feu applicatif**.

> D’autres notions importantes interviennent également à ce niveau, telles que la scalabilité, le routage avancé ou la haute disponibilité.


## Préambule : différentes formes d’hébergement

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*I3cw3M43PVg3JFYv_gRh2g.png" />

Historiquement, on distingue plusieurs grandes formes d’hébergement applicatif :

* **Bare metal** : exécution directe sur un serveur physique, sans couche de virtualisation. Cette approche est performante, mais souvent plus complexe à administrer.

* **Machine virtuelle (VM)** : utilisation d’un hyperviseur (par exemple VMware ou KVM) pour exécuter plusieurs systèmes isolés sur une même machine physique.

* **Conteneur** : exécution légère et isolée d’applications via des outils comme Docker ou Kubernetes, partageant le noyau du système d’exploitation hôte.

### Idée générale

L’objectif, au fil du temps, a été de réduire le nombre de machines physiques nécessaires et d’optimiser l’utilisation des ressources, en faisant cohabiter plusieurs traitements sur une même infrastructure grâce à des mécanismes d’isolation des processus.

> **Tous ces modes d’hébergement restent pertinents selon le contexte** : contraintes de performance, coûts, sécurité, simplicité d’exploitation ou exigences réglementaires.


## Architecture de Kubernetes : un standard pour l’hébergement cloud

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*HHRp0HENvfAu2hXT8Gto9g.png" />

Kubernetes est une plateforme d’**orchestration de conteneurs** conçue pour administrer un ensemble de serveurs hébergeant des applications sous forme de conteneurs.

Il fournit une couche d’abstraction permettant de gérer automatiquement le déploiement, l’exécution et l’exploitation d’applications distribuées à l’échelle d’un cluster.

Kubernetes permet notamment :
* de lancer automatiquement des conteneurs sur différentes machines selon les besoins ;
* de conserver les données grâce à des espaces de stockage persistants ;
* de faire communiquer les applications entre elles à l’intérieur du cluster, même si elles sont isolées dans des conteneurs ;
* de gérer le placement des applications, leur redémarrage en cas de problème et leur mise à jour sans interruption visible pour les utilisateurs.


L’idée fondamentale est la suivante :
1. partir du concept de processus isolés exécutés sur une machine (via les conteneurs) ;
2. administrer plusieurs serveurs réunis au sein d’un même cluster ;
3. centraliser la gestion des demandes au niveau du plan de contrôle (*control plane*) et répartir l’exécution sur les nœuds de travail (*worker nodes*) ;
4. surveiller en continu l’état du cluster afin de garantir que l’état réel corresponde à l’état désiré (nombre de réplicas, services actifs, version des applications, etc.).


## Utilisation d’un cluster Kubernetes

![configmap](/images/kube/configmap-gcp.png)

Pour utiliser Kubernetes, on ne se connecte pas directement aux machines.
On communique avec un composant central appelé **API Server**.
L’API Server est une **interface qui reçoit des demandes** (par exemple : « lance cette application », « expose ce service », « ajoute cette configuration ») sous forme de fichiers décrivant ce que l’on veut obtenir dans le cluster.

Ces fichiers sont écrits dans un format lisible par l’humain : **YAML** (basé sur JSON).

Dans ce cours, nous utiliserons principalement l’outil en ligne de commande **`kubectl`**, qui sert d’intermédiaire entre nous et l’API Kubernetes.

> Remarque : les outils en ligne de commande (CLI) sont très importants dans l’ingénierie logicielle. On les retrouve partout : administration de bases de données, gestion d’API, déploiement d’applications, etc.


### Décrire une application : exemple simple avec un Pod

Voici un exemple de fichier YAML décrivant un **Pod** (l’unité de base d’exécution dans Kubernetes) :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
  - name: hello-world-container
    image: ragatzino/backend-demo-conception-logicielle:latest
    ports:
    - containerPort: 8000
```

> Ce fichier décrit ce que Kubernetes doit lancer : un conteneur, à partir d’une image donnée, avec un port ouvert.

Quand on envoie ce fichier au cluster, Kubernetes démarre automatiquement le processus sur l’un des serveurs disponibles.


### Déployer via l’API Kubernetes

Kubernetes permet d’installer des applications **à la demande** grâce à son API.

On ne déploie pas directement des programmes, mais des **ressources Kubernetes**, par exemple :

* des **Deployments**: pour gérer des applications;
* des **Services** ou **Ingress**: pour exposer des applications sur le réseau;
* des **ConfigMaps** : pour stocker de la configuration.

Comme toute API bien constituée, elle nécessite une authentification et n'est pas forcément accessible partout.

### Exemples d'implémentations

Kubernetes est un **projet open source**, ce qui explique qu’il existe de nombreuses plateformes qui l’utilisent.

***Plateformes cloud grand public**
* **GKE (Google Cloud Platform)** : offre Kubernetes chez Google (avec souvent du crédit gratuit au départ)
* **EKS** : Kubernetes chez Amazon Web Services
* **AKS** : Kubernetes chez Microsoft Azure

**Clouds institutionnels (France)**
* **SSPCloud** : Cloud administré par la division innovation à l'INSEE
* **Nubo** : cloud interministériel (DGFiP notamment)

### Kubectl : Utilitaire CLI pour dialoguer avec l'API server d'un cluster Kubernetes

`kubectl` est la commande qui permet de discuter avec l’API Server du cluster.
Il permet de gérer les ressources Kubernetes (Pods, Services, Deployments, etc.), déployer des applications et diagnostiquer des problèmes.

L’installation est disponible ici :
[https://kubernetes.io/docs/reference/kubectl/](https://kubernetes.io/docs/reference/kubectl/)

> Dans certains environnements (comme SSPCloud), tout est déjà installé et configuré.


#### Commandes de base à connaître

Pour afficher les ressources existantes d’un certain type ainsi que leur état :

```bash
kubectl get <ressources>
```
où `<ressources>` peut être par exemple `pods`, `services`, `deployments` ou `configmaps`.

Pour supprimer une ressource précise :

```bash
kubectl delete <ressource> <nom-de-la-ressource>
```
`<nom-de-la-ressource>` correspond simplement au nom attribué à cette ressource dans le cluster.

Pour obtenir des informations détaillées sur une ressource :

```bash
kubectl describe <ressource> <nom-de-la-ressource>
```

Cette commande est très utile pour comprendre le comportement d’un Pod ou diagnostiquer un problème de déploiement.

**Appliquer une configuration depuis un fichier**

Pour envoyer un fichier YAML au cluster :

```bash
kubectl apply -f <nom_du_fichier.yaml>
```

Pour supprimer ce qui est décrit dans ce fichier :

```bash
kubectl apply -f <nom_du_fichier.yaml>
```
**Exemple : un Deployment simple**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend
          image: ragatzino/backend-demo-conception-logicielle:latest
          resources:
            limits:
              memory: "200Mi"
              cpu: "300m"
          envFrom:
            - configMapRef:
                name: configuration-backend
          ports:
            - containerPort: 8000
```

> Ce fichier demande à Kubernetes de maintenir une application backend en fonctionnement, avec des limites de ressources et de la configuration externe.
















---

## Ressources d'un cluster

![ressources d'un cluster](/images/kube/Kubernetes_Resources.png)

Dans cette partie on va présenter quelques ressources que vous pourrez utiliser sur le cluster, bien d'autres existent répondant chacunes a des besoins spécifiques : 

- Gestion de la resilience et de l'ordonnancement: **deployment**, **statefulSet**, **cronjob**
- Configuration applicatives : **configmap** **secret**
- Routage réseau: **ingress**, **service**
Plus loin :
- Droits d'accès à vos ressources : **role**, **rolebinding**, **serviceaccount**
- Persistence des données: **persistentvolume**, **persistentvolumeclaim**

> Remarque, il en existe une myriade d'autres

**Quel est notre objectif?**

Déployer une application sur le cloud, accessible via une url fixe, et avec une configuration via variable d'environnement

**Comment y arriver?**
Nous allons nous limiter pour les usages du cours a 4 ressources: 
- Deployment : Lancer des processus conteneurisés **pod**, les répliquer, définir une stratégie de mise a jour
- Service : Permettre l'accès a des processus a l'intérieur du cluster, ou leur donner une adresse IP
- Ingress : Mettre en place une entrée DNS, et un routage vers un service en HTTP depuis l'extérieur du cluster.
- Configmap: Permettre la création de fichiers de configuration et l'injection de variables d'environnements a l'intérieur de nos conteneurs.

> On va détailler cela dans cette partie.

### Pod : la brique unitaire contenant un/des conteneur(s)

![pod](/images/kube/pod.jpg)

Un pod est le plus petit concept Kubernetes et correspond (en gros) à un ensemble de conteneurs (souvent un seul) qui fonctionnent ensemble et se partagent les ressources notamment le réseau. Pour en savoir plus sur les concepts kubernetes, ça commence ici : https://kubernetes.io/docs/concepts/.

Dans notre cas on lancera un seul conteneur par "Pod", donc cela sera équivalent a une instance de votre application <=> un `docker run`.

**Problème:** si il plante, le processus n'est pas relancé donc en général on utilise pas la ressource Pod telle qu'elle.

**Solution**: Il faut **orchester** ces traitements, et en fonction de leur enjeu, leur appliquer une règle

**Exemples**
- Un traitement qui s'execute de manière asynchrone et one shot (on appelle ça un batch), doit fonctionner mais ne doit pas être executé en boucle
- Un traitement qui offre un service a des utilisateurs en exposant des ressources (API, UI), doit a priori être toujours en ligne, et donc on doit veiller a ce qu'ils soient relancés si ils plantent.

### Deployment : Orchestration des pods - redémarrer, mettre a jour..

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*E6OVAUw16eBwPID3.png"/>

Un Deployment est un objet Kubernetes permettant de gérer et mettre à jour un ensemble de Pods de manière déclarative (contrat de la ressource).

**Objectifs**
- Il assure la haute disponibilité et l'auto-réparation des pods. 
- Il permet le scaling et les rollbacks.

### Configmap: externaliser la configuration, introduire des variables d'environnement dans les conteneurs

Un ConfigMap stocke des données de configuration sous forme de clés-valeurs et permet aux pods de les utiliser.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configuration-application
data:
  VARIABLE_ENVIRONNEMENT1: variable_environnement
  DATABASE_URL: "postgres://user:password@db:5432/mydb"
```
Il sont ensuite importable en tant que variables d'environnement dans les conteneur ainsi : 


```yaml
containers:
  - name: mon-container
    image: mon-image:v1
    envFrom:
      - configMapRef:
          name: mon-configmap
```

### Service : Exposition intra cluster et load balancing

![service](/images/kube/service.gif)

Un Service expose un ensemble de Pods et assure la communication entre eux ou avec l'extérieur.


```yaml
apiVersion: v1
kind: Service
metadata:
  name: application
spec:
  selector:
    app: application # doit correspondre a une valeur définie dans le deployment.yaml
    #     metadata:
    #        labels:
    #          app: application
  ports:
    # PORT QUE VOUS VOULEZ, 80 correspond au port standard http, doit correspondre côté ingress.
    - port: 80
      # PORT DU CONTENEUR, ET DONC DE VOTRE SERVEUR
      targetPort: 5173

```

### Ingress : exposition HTTP et ingress
![ingress](/images/kube/ingress.png)

Un Ingress permet d'exposer des Services HTTP/S en définissant des règles de routage.

Exemple :
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: application
  labels:
    name: application
spec:
  rules:
    - host: hello.kube.sspcloud.fr
    # CETTE URL est unique, donc si quelqu'un l'a déjà prise vous ne pourrez pas la prendre
    # Changer le host vers une url qui fonctionne, une url du type *.kub.sspcloud.fr exemple mon-application.kub.sspcloud.fr
      http:
        paths:
          - pathType: Prefix
            path: "/"
            backend:
              service:
                name: application
                port:
                  number: 80
```

> Remarque : L’Ingress nécessite un Ingress Controller (ex: Nginx Ingress Controller) pour fonctionner. C'est le cas sur le sspcloud.



## TP : Mise en place de vos applications sur les plateformes kuberentes du SSPCloud

> Ce TP nécessite la mise en place d'un livrable au format docker et le déploiement sur un registre publique (type dockerhub) pour l'héberger par la suite.

<img src="https://www.sspcloud.fr/assets/heroHeader-C7FzBYtH.png" />

Le sspcloud propose un api server ouvert au sein du cluster kubernetes SSPCloud à ces utilisateurs.
Ce service est un service a faible support, seulement pour des usages de poc, pas de données sensibles, pas de garantie de service, toutefois cela convient tout a fait pour les expérimentations et l'hébergement de projets informatiques ENSAI.

Pour la mise en place, il nous faut un service qui peut accéder au cluster et donc qui est hébergé sur le cluster (puisque l'api n'est accessible que depuis le cluster).

Le SSPCloud met a disposition des services qui vous permettent d'instancier des applications sur le cluster, notamment des services de type "plateforme de développement" : 
- VScode
- RStudio

### **1 - Mise en place d'un service dans le cluster**
Rendez vous sur le SSPCloud pour déployer un service `VSCode` en choisissant `admin` dans l'onglet `Kubernetes`. 

- [Lien direct](https://datalab.sspcloud.fr/launcher/inseefrlab-helm-charts-datascience/vscode?autoLaunch=false&kubernetes.role=%C2%ABadmin%C2%BB). 

Ce choix `admin` donne à votre service `VSCode` les permissions nécessaires au déploiement d'une application dans le cluster kubernetes du sspcloud.

### **2 - Interaction avec le cluster kubernetes**

Une fois le `VSCode` lancé, ouvrez le et ouvrez y un terminal. On utilise l'utilitaire `kubectl`. 

Lancez la commande suivante:

```
kubectl get pods
```

> Vous constatez, de manière méta que vous disposez d'une ressource de type pod, qui est le vscode dans lequel vous vous situez.

### **3 - Récupération de contrats de base**

On a prémaché le travail pour vous, on a mis des contrats approchant ce que vous pouvez vouloir mettre en place sur le dépôt suivant [https://github.com/conception-logicielle-ensai/exemples-cours.git](https://github.com/conception-logicielle-ensai/exemples-cours.git) :

- Faites ce que vous faites le mieux depuis le vscode : `git clone https://github.com/conception-logicielle-ensai/exemples-cours.git`

Dans la partie : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-6/kubernetes/app](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-6/kubernetes/app)

Observez le contenu du sous dossier `exemples-cours/cours-6/kubernetes/app` : il y a différents fichiers ̀`yaml` de configuration de ressources pour kubernetes.

Mettez en place la configuration en amont du déploiement (le déploiement le référence) : 

```
kubectl apply -f exemples-cours/cours-6/kubernetes/app/configmap.yaml
```

Appliquez le deployment:

```
kubectl apply -f exemples-cours/cours-6/kubernetes/app/deployment.yaml
```

et vérifiez vos `Pod`

```
kubectl get pods
```

### **4 - Exposition sur internet**

Modifiez le contrat, ingress pour mettre une url qui vous est propre, puis appliquez les différents fichiers 

Service
```
kubectl apply -f exemples-cours/cours-6/kubernetes/app/service.yaml
```

```
kubectl apply -f exemples-cours/cours-6/kubernetes/app/ingress.yaml
```


### **5 - Application d'une configuration et redémarrage**

On souhaite maintenant changer la configuration pour afficher une autre information sur la page d'accueil du swagger:

Si l'on regarde le configmap, on peut y configurer des variables d'environnement pour notre application :



Notre application reçoit la variable d'environnement : `APP_TITLE` pour changer a l'affichage du swagger le titre de l'application au niveau de sa documentation.
Réalisez une modification de ce champ dans le fichier configmap et relancez votre application.

**pour appliquer le changement**:
```
kubectl apply -f exemples-cours/cours-6/kubernetes/app/configmap.yaml
```

Mais il vous faut relancer, car les variables d'environnement ne se chargent qu'au démarrage des applicatifs.

**pour relancer**

Soit vous supprimez tout refaites tout : 
- `kubectl delete -f exemples-cours/cours-6/kubernetes/app/` 
- puis `kubectl apply -f exemples-cours/cours-6/kubernetes/app/`
Soit vous supprimez le pod : 
- `kubectl get pods` 
- puis en notant lepodtrouvésvpchangezmoi le nom du pod `kubectl delete pod lepodtrouvésvpchangezmoi`



### **6 - Passage a votre appli**

Modifier l'image, et éventuellement les ports d'écoute du déployment et du service pour changer d'application et déployer l'application que vous avez packagé précédemment dans ce TP.
