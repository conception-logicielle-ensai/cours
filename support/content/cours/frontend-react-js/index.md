---
title: "Frontend, JavaScript et React"
weight: 18
draft: false
summary: "Présentation des données destinée aux utilisateurs : le frontend avec le framework React"
slug: "frontend-react-js"
tags: ["frontend", "javascript", "react", "vite"]
series: ["Cours"]
series_order: 9
---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : 
https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/frontend-react-js


## Architecture applicative

<img src="https://www.getwidget.dev/blog/wp-content/uploads/2024/10/How-to-Connect-Frontend-and-Backend-Applications.png" />

Lorsqu’on développe une application, il est essentiel d’organiser le code afin qu’il soit lisible, évolutif et simple à maintenir. 
Cette organisation repose sur une **architecture applicative**, c’est-à-dire **la manière dont les différentes parties du système sont découpées et communiquent entre elles**.

Deux modèles sont particulièrement répandus dans le développement web.

### Architecture MVC (Modèle – Vue – Contrôleur)

L’architecture **MVC** est une organisation logique du code en trois responsabilités distinctes :

* **Modèle** : gère les données, leur validation et leur persistance (base de données, fichiers…).
* **Vue** : se charge de l’affichage et de la présentation à l’utilisateur.
* **Contrôleur** : reçoit les actions de l’utilisateur, orchestre le traitement et décide quelles données doivent être affichées.

Cette architecture est historiquement très utilisée dans les applications **monolithiques**, notamment lorsque le rendu des pages est effectué côté serveur (**Server Side Rendering – SSR**). Dans ce cas, le backend génère directement les vues HTML envoyées au navigateur.

### Architecture frontend / backend

Dans la majorité des applications web modernes, on adopte une séparation plus marquée entre :

* l’interface utilisateur,
* la logique métier et les données.

On parle alors d’architecture **frontend / backend**, qui s’inscrit pleinement dans le modèle **client / serveur** évoqué précédemment.

L’application est divisée en deux grandes parties :

#### Le frontend

Le **frontend** correspond à la partie **exécutée côté client** (généralement dans le navigateur).
Il regroupe :

* l’interface graphique (boutons, formulaires, tableaux, graphiques, animations…),
* la gestion des interactions utilisateur,
* l’affichage dynamique des données.

Son objectif principal est d’offrir une expérience utilisateur fluide, claire et réactive.

#### Le backend

Le **backend** correspond à la partie **exécutée côté serveur**.
Il est responsable de :

* la logique métier,
* la validation des données,
* l’accès aux bases de données,
* les calculs et traitements,
* la sécurité et l’authentification.

Dans les applications modernes, ces deux parties communiquent au moyen d’**API**, lesquelles définissent les points d’accès disponibles, le format des échanges et les règles de sécurité.

<img src="/images/react/frontend-vs-bancend.webp"/>


### Comment fonctionnent frontend et backend ensemble ?

Dans une application typique, le frontend et le backend interagissent selon une séquence bien définie :

1. l’utilisateur effectue une action sur l’interface, *par exemple en soumettant un formulaire* ;
2. le frontend envoie une requête HTTP vers une API exposée par le backend, *comme une requête POST destinée à enregistrer des données* ;
3. le backend traite la demande, applique les vérifications nécessaires et stocke les informations en base ;
4. une réponse est renvoyée au frontend, indiquant un succès, un échec ou contenant des données mises à jour ;
5. le frontend met à jour l’affichage en fonction de la réponse reçue.

Ce mécanisme repose le plus souvent sur des opérations dites **CRUD** — Créer, Lire, Mettre à jour et Supprimer — qui permettent au frontend d’interagir avec les données par l’intermédiaire du backend.

### Votre rôle en tant que futurs statisticiens

Dans un projet, les équipes se répartissent généralement les tâches :  
- Les développeurs **frontend** conçoivent et optimisent l'interface utilisateur.  
- Les développeurs **backend** s'occupent des traitements de données et de la mise en place des API. 

En tant que statisticiens, vous serez souvent amenés à travailler sur le **backend**, en manipulant les données.

> [!TIP]+ Pour aller plus loin
> [The BFF Pattern (Backend for Frontend): An Introduction](https://blog.bitsrc.io/bff-pattern-backend-for-frontend-an-introduction-e4fa965128bf)


## Introduction à React  

### Frameworks frontend

Pour faciliter le développement, on utilise des **frameworks**, qui sont des ensembles d'outils et de bonnes pratiques permettant de structurer le code et de gagner du temps.  

- **Côté frontend**, des frameworks comme `React.js`, `Angular` ou `Vue.js` permettent de créer des interfaces interactives et dynamiques.  
- **Côté backend**, des frameworks comme `Spring Boot (Java)` ou `FastAPI (Python)` facilitent la gestion des requêtes, des bases de données et de la logique métier.

![ ](/images/react/logo-react.png)

`React` est une bibliothèque `JavaScript` open-source développée par Facebook. 
Elle est utilisée pour créer des interfaces utilisateur dynamiques et performantes. 
De nombreuses applications populaires comme **Facebook, Messenger et Instagram** reposent en grande partie sur cette technologie.  

Il peut être difficile d'apprendre React en un seul cours, mais si cela vous intéresse, il existe de nombreux tutoriels simples et accessibles :


> [!TIP]+ Pour aller plus loin
> [Tutoriel OpenClassrooms : "Débutez avec React"](https://openclassrooms.com/fr/courses/7008001-debutez-avec-react)

> [!TIP]+ Pour aller plus loin
> [Tutoriel : Tic-Tac-Toe](https://fr.react.dev/learn/tutorial-tic-tac-toe)
 

### Le DOM et le Virtual DOM  

Lorsqu'un navigateur charge une page web, il construit une représentation sous forme d’arbre appelée **DOM (Document Object Model)** à partir du HTML. 
Ce DOM est manipulable par l'utilisation du Javascript (c'est le cas, par exemple, pour les outils de développement des navigateurs, notamment l'onglet **"Elements"** dans les **Developer Tools**.) 

React ne modifie pas directement ce DOM du navigateur. 
À la place, il créée et utilise un **DOM virtuel (Virtual DOM)**, qui est une copie optimisée du DOM réel.
Lorsque l'interface change, React compare le Virtual DOM avec l'ancien état, détecte les modifications nécessaires et met à jour uniquement les parties concernées du DOM réel. 
Cela permet d'améliorer considérablement les performances. 

> [!TIP]+ Pour aller plus loin
> [Différence entre DOM et Virtual DOM, un article](https://www.geeksforgeeks.org/difference-between-virtual-dom-and-real-dom/)

### Le principe des composants  

L'idée centrale de React est de construire une application à partir de **composants**. 
Un **composant** est un bloc réutilisable qui regroupe tout ce dont il a besoin :  
- **La structure** (HTML)  
- **Le style** (CSS)  
- **Le comportement** (JavaScript)  

<img  src="https://flaviocopes.com/images/react-components/1.png" />

Les composants suivent un cycle de vie composé de trois étapes : Mounting (ajout au DOM), Updating (mise à jour) et Unmounting (suppression du DOM). Leur comportement peut être modifié à chaque étape grâce aux **hooks**. 

Un **composant fonctionnel** est simplement une fonction JavaScript qui retourne un élément React :  

```jsx
function MyComponent() {
    return <div>Hello ENSAI 👋</div>;
}
```

Ces composants :
- **Encapsulent tout leur fonctionnement** : structure, styles et comportement  
- **Sont réutilisables** : on peut les utiliser plusieurs fois dans une même application  
- **Peuvent être imbriqués** les uns dans les autres pour construire une interface complète  

React permet donc de **découper une interface en petits composants réutilisables** et de les assembler pour construire des applications performantes.  

React utilise le langage `JSX`, qui est **une extension de `JavaScript`** permettant d'intégrer une syntaxe similaire à HTML directement dans le code JavaScript. 

### Caractéristiques du JSX :

- **Langage à balises** : Comme le HTML, le JSX se compose de balises.
- **Majuscule pour les composants** : Pour que React reconnaisse un élément comme un composant, il est essentiel de commencer son nom par une majuscule. Sinon, React considérera qu'il s'agit d'une balise HTML standard.
- **Encapsulation des composants** : Lorsque vous utilisez plusieurs composants, ils doivent être enveloppés dans un seul composant parent.

Voici comment structurer plusieurs composants enfants à l'intérieur d'un composant parent :

```jsx
function Parent() {
    return (
        <div>
            <Enfant />
            <Enfant />
            <Enfant />
        </div>
    );
}

function Enfant() {
    return <div>Je suis un enfant !</div>;
}
```


### Ajouter du style à notre composant

Pour styliser notre composant, il suffit d'utiliser l'attribut `className` et d'indiquer le nom du sélecteur CSS correspondant.

Par exemple, si je souhaite que le texte "Hello ENSAI 👋" soit en rouge, je peux procéder comme suit :

**Fichier nommé `Titre.css`**
```css
.helloensai {
  color: red;
}
```

**Fichier contenant votre composant**
```jsx
import './Titre.css';

function MyComponent() {
    return <div className="helloensai">Hello ENSAI 👋</div>;
}
```


> [!TIP]+ Pour aller plus loin
> [Démarrer avec CSS](https://developer.mozilla.org/fr/docs/Learn_web_development/Core/Styling_basics/Getting_started)

Le langage React est largement utilisé, et grâce à ça, il y a plein de **bibliothèques de composants React** prêtes à l'emploi. Elles permettent d'avoir un design harmonieux et vous font gagner un temps fou, car vous n'avez pas besoin de redéfinir chaque composant de base. En plus, elles proposent des options pour rendre votre application **accessible à tous** et vous donnent la possibilité de personnaliser les éléments selon vos besoins.

À l'INSEE, on utilise surtout la bibliothèque [**Material UI**](https://mui.com/material-ui/) et le [**Design système de l'état - DSFR**](https://www.systeme-de-design.gouv.fr/), mais il y a aussi d'autres choix comme Bootstrap ou Ant Design.


> [!TIP]+ Pour aller plus loin
> [Liste des composants Material UI](https://mui.com/material-ui/all-components/)


### Un hook React

Les **Hooks** sont des fonctions qui permettent de "se brancher" sur la **gestion de l'état local et du cycle de vie de React** depuis des composants fonctionnels. React propose plusieurs Hooks prédéfinis, comme `useState` ou `useEffect`. Mais vous pouvez aussi créer vos propres Hooks pour **extraire une logique (état, effets, calculs…) d’un composant et la réutiliser dans d’autres**.

### Le hook d'état : `useState`

Imaginons que vous souhaitez que l'interaction avec l'utilisateur change le comportement de votre composant. *Par exemple, si vous cliquez sur un bouton pour ouvrir la description, cela affichera la description de votre Pokémon. Inversement, si vous cliquez sur un bouton pour fermer la description, celle-ci sera masquée.*

Pour cela, vous devez définir un **hook d'état : ce hook permet d'enregistrer l'état de votre composant**. Voici comment procéder dans votre composant :

```jsx
import { useState } from "react";

function Pokemon() {
    const [isOpen, setIsOpen] = useState(false);
    return (
        <div>
            <h1>Dracaufeu</h1>
            {isOpen ? (
                <p>
                 Dracaufeu (anglais : Charizard ; japonais : リザードン Lizardon) est un Pokémon de type Feu et Vol de la première génération. C'est la mascotte des jeux Pokémon Rouge et Pokémon Rouge Feu.
                </p>
            ) : null}
            <button onClick={() => setIsOpen(!isOpen)}>
                {isOpen ? 'Fermer la description' : 'Ouvrir la description'}
            </button>
        </div>
    );
}
```

> `isOpen` est **la valeur actuelle de l’état**, et `setIsOpen` est **la fonction qui permet de modifier cet état**. La valeur entre parenthèses (`false`) est la valeur par défaut de l’état.


*Cependant, la description et le nom du Pokémon sont écrits en dur.* Pour réutiliser notre composant, nous allons utiliser les props.

### Transmission Parent / Enfant : `Props`

Nous allons réécrire le composant comme suit :

```jsx
import { useState } from "react";

function Pokemon({ nom, description }) {
    const [isOpen, setIsOpen] = useState(false);
    return (
        <div>
            <h1>{nom}</h1>
            {isOpen ? (
                <p>{description}</p>
            ) : null}
            <button onClick={() => setIsOpen(!isOpen)}>
                {isOpen ? 'Fermer la description' : 'Ouvrir la description'}
            </button>
        </div>
    );
}
```

Ensuite, dans un composant parent, nous ferons ceci :

```jsx
import { useState } from "react";

const pokemonListJson = [
    {
        nom: "Dracaufeu",
        description:
            "Dracaufeu (anglais : Charizard ; japonais : リザードン Lizardon) est un Pokémon de type Feu et Vol de la première génération. C'est la mascotte des jeux Pokémon Rouge et Pokémon Rouge Feu.",
    },
    {
        nom: "Salamèche",
        description:
            "Salamèche (anglais: Charmander ; japonais: ヒトカゲ Hitokage) est un Pokémon de type Feu de la première génération. C'est l'un des Pokémon de départ de la région de Kanto.",
    },
];

function PokemonList() {
    return (
        <div>
            <h1>Liste des Pokémon</h1>
            {pokemonListJson.map((pokemon, index) => (
                <Pokemon key={index} nom={pokemon.nom} description={pokemon.description} />
            ))}
        </div>
    );
}
```

Dans notre composant `PokemonList`, nous affichons tous les Pokémon présents dans la liste `pokemonListJson`. Bien sûr, l'objectif est d'afficher tous les Pokémon que nous recevrons dans un appel à l'API, mais nous aborderons cela plus tard.

Les **props** (properties) sont des **paramètres que tu passes à un composant pour lui fournir des données dynamiques**. Elles permettent de rendre les composants réutilisables et configurables.
Les props sont passées comme des attributs HTML à un composant dans son coposant parent.

Si nous voulons utiliser l'état d'un composant dans un autre, nous appliquerons la méthode consistant à définir l'état dans le premier composant parent commun aux deux composants. Nous descendrons ensuite l'état et la méthode de changement jusqu'aux composants qui en ont besoin.


### Le hook de synchronisation: `useEffect`

Le hook `useEffect` est utilisé pour effectuer des **effets de bord dans les composants fonctionnels de React**. Ce hook vous permet d'exécuter du code **après le rendu d’un composant**  (le composant a été monté ou mis à jour), et même de nettoyer les effets lorsque le composant est démonté. 

Voici un exemple simple d'utilisation du hook `useEffect`. Imaginons que nous voulons récupérer des données d'une API lorsque le composant est monté.

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

function PokemonList() {
    const [pokemons, setPokemons] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const fetchPokemons = async () => {
            try {
                const response = await axios.get("https://api.example.com/pokemons");
                setPokemons(response.data);
            } catch (error) {
                // On récupère le code d'état et le message d'erreur
                const errorMessage = error.response
                    ? `Erreur ${error.response.status}: ${error.response.data.message || "Erreur inconnue"}`
                    : "Erreur de réseau";
                setError(errorMessage);
            } finally {
                setLoading(false);
            }
        };

        fetchPokemons();
    }, []);  // Le tableau vide signifie que l'effet ne s'exécute qu'une fois, lors du premier rendu.

    if (loading) {
        return <p>Chargement des Pokémon...</p>; // Affichage pendant le chargement
    }

    if (error) {
        return <p>{error}</p>; // Affichage du message d'erreur
    }

    return (
        <div>
            <h1>Liste des Pokémon</h1>
            <ul>
                {pokemons.map((pokemon) => (
                    <li key={pokemon.id}>{pokemon.nom}</li>
                ))}
            </ul>
        </div>
    );
}
```

```js
useEffect(() => { ... }, []);
```

Le tableau à la fin s’appelle **le tableau de dépendances**.
Il indique **quand l’effet doit se relancer** :

* **Vide (`[]`)** → l’effet s’exécute **une seule fois**, au premier rendu du composant.
* **Avec des valeurs (`[val1, val2, ...]`)** → l’effet se relance **à chaque fois que ces valeurs changent**.


La méthode fetchPokemons() est une fonction asynchrone qui récupère la liste des Pokémon depuis une API en utilisant `Axios`. Elle utilise un bloc try-catch pour gérer les erreurs de requête. Si la requête réussit, elle met à jour l'état pokemons avec les données récupérées. En cas d'erreur, elle définit un message d'erreur dans l'état error. Enfin, elle met à jour l'état loading à false dans tous les cas, indiquant que le chargement est terminé. Cette méthode est appelée lors du premier rendu du composant PokemonList.

### Client HTTP : `Axios`

Axios est à JavaScript ce que `requests` est à Python. Il s'agit d'un client HTTP, comme expliqué dans le cours [HTTP: Consommation et construction d'API webservice](/cours/http-api-webservices/).

C'est une bibliothèque asynchrone, parmi d'autres comme `fetch`. Cependant, Axios est souvent préféré à `fetch` en raison de sa gestion automatique des requêtes et des réponses, de la possibilité d'annuler des requêtes, et de sa simplicité d'utilisation pour la configuration des en-têtes et des paramètres.

> [!TIP]+ Pour aller plus loin
> [Documentation officielle d'Axios](https://axios-http.com/fr/docs/intro)

> [!TIP]+ Pour aller plus loin
> [Comment utiliser Axios avec React](https://www.digitalocean.com/community/tutorials/react-axios-react-fr)


> **Remarque**: Les principes de conception logicielle s'appliquent toujours en javascript, il vous faudra externaliser et centraliser la configuration des urls.

Par exemple, pour une application utilisant `VITEJS` dans votre fichier `.env`, vous pourrez ajoutez la ligne suivante :

*Nous nous intéresserons a ViteJS dans une partie suivante*

```txt
VITE_API_URL=https://api.example.com
```

Les variables d'environnement doivent commencer par `VITE_` pour être exposées à votre code client.

**Dans un fichier `api.js` :**

```js
import axios from 'axios';

const apiUrl = import.meta.env.VITE_API_URL;
export default axios.create({
  baseURL: apiUrl
});
```

Dans le fichier où vous utilisez Axios, il vous suffira d'ajouter l'import suivant :

```jsx
import API from './api';
```

Ensuite, remplacez la ligne :

```jsx
const response = await axios.get("https://api.example.com/pokemons");
```

par :

```jsx
const response = await API.get("pokemons");
```

Cela simplifie vos appels API et rend votre code plus lisible.



## Utilisation d’outils d’aide au développement pour une application React

<img src="https://static1.makeuseofimages.com/wordpress/wp-content/uploads/2021/06/Create-Your-First-React.js-App.jpg" />

Après avoir découvert les concepts fondamentaux de React, voyons comment initialiser et exécuter une première application.


### Utilisation d’un outil de build : `Vite`

Dans un projet React, on ne travaille pas directement avec du JavaScript « brut ». On utilise notamment le **JSX**, ainsi que des fonctionnalités modernes du langage qui nécessitent une phase de transformation avant d’être exécutées dans le navigateur.

Pour cela, il est indispensable de s’appuyer sur un **outil de développement** chargé de :

* lancer un **serveur local** afin d’observer les modifications en temps réel ;
* transformer et optimiser le code pour assurer la compatibilité avec les navigateurs ;
* gérer les imports de ressources (CSS, images, polices, etc.).

Plutôt que de configurer manuellement ces mécanismes, on utilise des **outils clé en main** appelés *build tools* ou *bundlers*. Parmi eux, `Vite` est aujourd’hui très apprécié pour sa rapidité et sa simplicité d’utilisation. Il est notamment largement utilisé à l’Insee.

D’autres alternatives existent, comme `Create React App` ou `Next.js`.

### Exécution du code JavaScript avec `Node.js`

Par défaut, JavaScript s’exécute dans un navigateur web. Toutefois, pour développer une application moderne, il est nécessaire de disposer d’un **environnement capable d’exécuter JavaScript en dehors du navigateur** : c’est le rôle de `Node.js`.

`Node.js` repose sur le moteur **V8** de Google Chrome et permet d’exécuter du code JavaScript directement sur votre machine, comme n’importe quel autre langage de programmation.

**Pourquoi a-t-on besoin de Node.js dans un projet React ?**

* pour lancer le **serveur de développement** (Vite en dépend) ;
* pour installer et gérer les bibliothèques via `npm` (*Node Package Manager*) ;
* pour exécuter les scripts de build qui génèrent la version finale de l’application.

> [!TIP]+ Pour aller plus loin
> [Documentation sur le moteur JavaScript V8](https://nodejs.org/en/learn/getting-started/the-v8-javascript-engine)

En **production**, l’application n’est plus servie par Vite. Les outils de build génèrent des fichiers optimisés (`HTML`, `CSS` et `JavaScript`) qui sont ensuite hébergés sur un **serveur statique** et exécutés directement dans le navigateur des utilisateurs.


### Gestion des dépendances avec `npm`

À l’image de `pip` pour Python, `npm` (*Node Package Manager*) est le gestionnaire de paquets de Node.js. Il permet d’installer, de mettre à jour et de partager les bibliothèques utilisées dans un projet JavaScript.




## Créer votre application React

#### Vérification de l’installation de Node.js et npm

Sur une distribution Linux (Debian / Ubuntu), vous pouvez vérifier leur présence avec :

```bash
node -v
npm -v
```

### Installation de Vite.js

<img style="max-width:40%;margin-left: auto; margin-right:auto" src="/images/react/logo-vite.svg"/>

**Préambule, utilisation de npm :** 

La commande `npm install` va installer toutes les dépendances nécessaires à ton projet dans le dossier `node_modules`. Il est important de noter que, lorsque tu versionnes ton projet avec Git, tu dois absolument ajouter ce dossier dans  fichier `.gitignore` pour éviter d'envoyer les fichiers inutiles sur ton dépôt :

```txt
node_modules
```

La syntaxe est : 

```bash
npm install <package>
```

**Procédure d'installation d'un projet Vite.js**

Pour installer Vite.js, il suffit de lancer cette commande dans le terminal :
```bash
npm install -g create-vite
```

Ensuite, pour démarrer un projet avec Vite.js, vous pouvez suivre ces étapes :

```bash
npm create vite@latest mon-projet
cd mon-projet
npm install
```

**Procédure pour lancer un projet Vite.js**

Pour lancer l'application localement après l'installation, utilise cette commande :
```bash
npm run dev
```

Ton application sera accessible à l'adresse `http://localhost:5173/`, et elle se rechargera automatiquement à chaque modification.

Si tu veux tester ton application en mode production, tu peux lancer les commandes suivantes :

```bash
npm run build
npm run preview
```

Ton application sera alors accessible à l'adresse `http://localhost:5173/`.

> [!TIP]+ Pour aller plus loin
> [Documentation Officiel de Vite](https://vitejs.fr/)


### Détails des fichiers et dossiers générés par Vite.js

<img style="max-width:10%;margin-left: auto; margin-right:auto" src="https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgy94vc0uwvyii0dmhns1.png"/>

- **index.html** : C’est le fichier HTML de base qui est chargé dans le navigateur. Il contient généralement les liens vers les fichiers JavaScript et CSS de ton projet.
  
- **package.json** : Il contient toutes les informations importantes sur ton projet, comme les dépendances (voir [semantic-versionning](https://docs.npmjs.com/about-semantic-versioning)), les scripts pour démarrer et construire ton application, etc. C'est aussi là où tu ajoutes des bibliothèques supplémentaires.

- **package-lock.json** : Il contient toutes les informations sur les librairies utilisées lors d'une installation, permet de sanctuariser un ensemble de versions de package correspondant à la demande et qui permet de faire fonctionner l'application.

- **README.md** : Un fichier pour expliquer ton projet, comment l’utiliser et quelles sont ses fonctionnalités.

- **src/** : Ce dossier contient tout le code source de ton application. C’est là que tu vas créer tes composants, ajouter tes styles, et gérer toute la logique de ton application.

- **main.jsx** : Le point d’entrée de ton application. Il va initialiser ton application et y monter le composant principal dans le DOM.

- **App.jsx** : Le composant principal qui définit la structure de ton interface utilisateur.

- **public/** : Ce dossier contient les fichiers statiques (ex : images, logo) accessibles directement via l’URL de ton application.

- **vite.config.js** : Ce fichier te permet de personnaliser la configuration de Vite pour ton projet. Tu peux y ajouter des plugins ou définir des options de build.

- **node_modules/** : Ce dossier contient toutes les dépendances installées. Il est automatiquement généré par npm et ne doit pas être modifié manuellement.



Voici une version **corrigée, reformulée, plus précise techniquement**, avec un niveau de langage homogène pour un support de cours, et quelques **ajouts importants** pour éviter des incompréhensions fréquentes chez les étudiants.

---

### Structure des fichiers et dossiers générés par Vite

<img style="max-width:10%;margin-left:auto;margin-right:auto" src="https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgy94vc0uwvyii0dmhns1.png"/>

Lors de la création d’un projet React avec Vite, une arborescence standard est générée. Chaque fichier et dossier a un rôle bien précis dans le fonctionnement de l’application.


- **`index.html`** : Fichier HTML principal chargé par le navigateur. Il référence directement le point d’entrée JavaScript de l’application et sert de base au rendu initial.

- **`package.json`** : Fichier central du projet Node.js. Il contient notamment les métadonnées du projet (nom, version, description), la liste des dépendances et dépendances de développement, les scripts (`dev`, `build`, `preview`, etc.) les contraintes de versions des bibliothèques (voir le [semantic-versionning](https://docs.npmjs.com/about-semantic-versioning)). C’est également dans ce fichier que l’on ajoute de nouvelles dépendances.

- **`package-lock.json`** : Fichier de verrouillage des dépendances. Il enregistre **les versions exactes** des bibliothèques installées afin de garantir que tous les développeurs et environnements utilisent le même ensemble de packages. Il joue un rôle essentiel pour assurer la **reproductibilité** des installations.

- **`README.md`**: Fichier de documentation du projet. Il décrit son objectif, les étapes d’installation, les commandes principales et éventuellement l’architecture générale.

- **`src/`**
  Dossier contenant l’ensemble du **code source** de l’application : composants React, styles, hooks, services d’accès aux API, ... C’est le dossier dans lequel vous travaillerez le plus souvent.

- **`main.jsx`** : Point d’entrée de l’application côté JavaScript. Il crée la racine React et monte le composant principal dans le DOM, généralement dans une balise `<div id="root">` présente dans `index.html`.

- **`App.jsx`** : Composant racine de l’application. Il définit la structure générale de l’interface utilisateur et sert de point de départ à l’arbre des composants.

- **`public/`** : Contient les fichiers statiques accessibles directement par URL, sans être traités par Vite : images, icônes, ... Ces fichiers sont copiés tels quels lors du build.

- **`vite.config.js`** : Fichier de configuration de Vite. Il permet notamment : ’ajouter des plugins, de configurer les alias d’import, d’adapter le build, ...

-  **`node_modules/`** : Contient l’ensemble des dépendances installées via npm, yarn ou pnpm. Ce dossier est généré automatiquement et **ne doit jamais être modifié manuellement**.

### Organiser les composants dans ton projet

Si tu veux ajouter des composants à ton projet, tu peux les organiser dans le dossier `src/component/`. Par exemple, ton projet pourrait ressembler à ceci :

```txt
mon-projet/
.
.
.
├── src/
│   ├── component/
│   │   ├── MonComposant1.jsx
│   │   ├── MonComposant2.jsx
│   ├── styles/
│   │   ├── MonComposant1.css
│   │   ├── MonComposant2.css
│   ├── main.jsx
│   └── App.jsx
.
.
.
```

Cela te permet de bien structurer ton projet au fur et à mesure que tu ajoutes des composants et des styles.

> [!TIP]+ Pour aller plus loin
> [Variables et modes Env](https://vitejs.fr/guide/env-and-mode.html)

### Utiliser des composants déjà constitués : `Material.ui`

<img style="max-width:30%;margin-left: auto; margin-right:auto" src="/images/react/mui.jpg"/>

**Material UI** est une bibliothèque de composants déjà conçus. Ils permettent de fournir un ensemble déjà construit d'éléments d'affichage qui contiennent déjà des éléments de style, d'accessibilité, de customisation.

Cette bibliothèque permet également de configurer un theme global au projet, ce qui centralise la gestion du style dans votre projet plutôt que de la déléguer à chaque sous composant. Vous pouvez également créer vos propres composants en utilisant des composants fils déjà très développés, les composants MUI.

Si vous souhaitez utiliser **Material UI** dans votre projet, vous pouvez l'ajouter  :
```bash
npm install @mui/material @emotion/react @emotion/styled
```

Cela va mettre à jour votre fichier `package.json` pour inclure les nouvelles dépendances.


### Linter, Formatter : `ESLint` `Prettier`

<img src="https://miro.medium.com/v2/resize:fit:1200/1*9eV__dcw3wCpie4PyxVtnA.png"/>

Le javascript est un language interprété comme python, il faut également veiller a contrôler et organiser le code proprement.

#### **Linter**

Nous vous préconisons d'utiliser le linter **ESlint**. Il s'agit d'un Linter pour le développement JS.

Il se configure a l'aide d'un fichier de configuration `.eslintrc`.

Dans une application vite.js: 

```bash
npm install vite-plugin-eslint --save-dev
```

Une fois installé, il s'applique sur le projet côté serveur.

Pour les contrôles en ligne de commande, et via `CI/CD` :

```bash
npx eslint
```

> [!TIP]+ Pour aller plus loin
> [Documentation de mise en place eslint sur projet vite](https://www.robinwieruch.de/vite-eslint/)


#### **Formatter**

Nous vous préconisons d'utiliser le formatter **Prettier**. Il s'agit d'un Formatter pour le développement JS.

Il s'installe via npm :

```bash
npm install --save-dev --save-exact prettier
```

Une fois installé, vous pouvez configurer votre vscode pour l'utiliser via l'extension `Prettier`.

Pour les contrôles en ligne de commande, et via `CI/CD` :

```bash
npx prettier .
```

> [!TIP]+ Pour aller plus loin
> [Tutoriel de mise en place sur vscode et installation](https://www.digitalocean.com/community/tutorials/how-to-format-code-with-prettier-in-visual-studio-code)