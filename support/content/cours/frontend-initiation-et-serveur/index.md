---
title: "HTML, JS, CSS, Templating et rendu côté serveur (SSR)"
weight: 19
draft: false
summary: "Découverte des standards du web pour le frontend et de solutions applicatives pour la mise en place de frontend pilotées par python"
slug: "frontend-initiation-et-serveur"
tags: ["frontend", "html", "javascript", "css", "SSR", "jinja" ]
series: ["Cours"]
series_order: 12
---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : 
https://github.com/conception-logicielle-ensai/exemples-cours/


Dans cette partie nous allons voir l'implémentation d'applications web, avec rendu de pages côté serveur.


## Interface web et sites web - Contexte


![ ](/images/frontend-initiation-et-serveur/htmljscss.jpg)


Pour cela nous allons commencer par étudier les *technologies* fondamentales du web : 
- **HTML** : Structures et contenu des pages
- **CSS** : Mise en forme et style d'affichage
- **Javascript** : Logique client et gestions des interactions avec la page



### HTML

HTML (HyperText Markup Language) est le language utilisé pour structurer une page web et son contenu. Par exemple, le contenu peut être organisé sous forme de paragraphes, d'une liste à puces ou encore à l'aide d'images et de tableaux de données. C'est donc le contenu de base pour tout site web.

Les fichiers **HTML** décrivent la structure logique d'une page web, définissent le contenu des pages. Ils s'écrivent a l'aide de balises `Markup` de manière plutôt verbeuse.


<div class="ws-grey" style="width:99%;border:1px solid grey;padding:3px;margin:0;">&lt;html&gt;
<div style="width:90%;border:1px solid grey;padding:3px;margin:20px">&lt;head&gt;
<div style="width:90%;border:1px solid grey;padding:5px;margin:20px">&lt;title&gt; Titre en haut de page&lt;/title&gt;
</div>
&lt;/head&gt;
</div>
<div class="ws-grey" style="width:90%;border:1px solid grey;padding:3px;margin:20px;">&lt;body&gt;
<div class="w3-white" style="width:90%;border:1px solid grey;padding:3px;margin:20px;">
<div style="width:90%;border:1px solid grey;padding:5px;margin:20px">&lt;h1&gt;Heading : titre de la partie&lt;/h1&gt;</div>
<div style="width:90%;border:1px solid grey;padding:5px;margin:20px">&lt;p&gt;C'est un paragraphe.&lt;/p&gt;</div>
<div style="width:90%;border:1px solid grey;padding:5px;margin:20px">&lt;p&gt;Un autre paragraphe.&lt;/p&gt;</div>
</div>
&lt;/body&gt;
</div>
&lt;/html&gt;
</div>

> Idée de structure globale d'une page HTML, adapté de : https://www.w3schools.com/html/html_intro.asp
#### Balises HTML

Parmi les balises HTML, on retrouvera celles utilisées le plus fréquemment pour structurer une page web :


- `<html>` : balise racine du document

- `<head>` : informations générales (titre, métadonnées, liens CSS)

- `<body>` : contenu visible de la page

- `<h1>` à `<h6>` : titres et sous-titres

- `<p>` : paragraphe de texte

- `<a>` : lien hypertexte

- `<img>` : image

- `<ul>`, `<ol>`, `<li>` : listes

- `<table>`, `<tr>`, `<th>`, `<td>` : tableaux

Et bien évidemment : la `<div>` : conteneur générique pour tout et n'importe quoi

> [!TIP]+ Exemple
> Vous pouvez accéder a un exemple <a href="htmlpur.html"> ici </a>

> [!TIP]+ Rappel
> Vous pouvez également récupérer le contenu rendu côté navigateur (client) via l'inspecteur sur votre navigateur

#### Attributs HTML

La plupart des balises HTML peuvent recevoir des **attributs**, des paramètres qui permettent de les identifier, ce qui permet ensuite de gérer une homogéneité dans l'application du style dans les applicatifs.

<p id="attribut-id-section">- 1️⃣ L'attribut <b>id</b></p>
<script>
  document.getElementById("attribut-id-section").style.color = "blue";
</script>

Sert à identifier un élément unique sur la page

Utilisé pour le CSS et le JavaScript
```html
<p id="attribut-id-section">- 1️⃣ L'attribut <b>id</b></p>
<script>
  document.getElementById("attribut-id-section").style.color = "blue";
</script>
```

<p class="texte-important highlight">- 2️⃣ L'attribut <b>class</b></p>

Sert à regrouper plusieurs éléments pour appliquer un style commun

Un élément peut avoir plusieurs classes
```html
<p class="texte-important highlight">- 1️⃣ L'attribut <b>id</b></p>
<p class="texte-important highlight">- 2️⃣ L'attribut <b>class</b></p>
```
```css
.texte-important {
  font-weight: bold;
}
.highlight {
  background-color: yellow;
}
```

- 3️⃣ L’attribut style

Permet d’ajouter un style directement sur l’élément

```html
<b style="color: red; font-size: 18px;">
Ce n'est pas recommandé pour une application,
il est bien plus intéressant de centraliser
tout le style dans du code dédié.
</b>
```

<b style="color: red; font-size: 18px;">Ce n'est pas recommandé pour une application, il est bien plus intéressant de centraliser tout le style dans du code dédié.</b>


> [!TIP]+ Bonnes pratiques
> Identifiez vos éléments avec des ids unique, utilisation de classes pour regrouper des objets qui devront être stylisés de la même manière.



### CSS

**CSS** (Cascading Style Sheets) est le language qui permet de styliser le contenu web. Sans CSS, un site n'a qu'une structure, mais pas de rendu visuel stylisé. Il est donc nécessaire de passer par la configuration de ces feuilles de style pour aboutir a un projet abouti côté Web.

- **A retenir** : Les fichiers `CSS` : fichiers de style, ils gèrent l'apparence des balises présentées.

#### Utilisation

Pour mettre en place du CSS il faut soit : 
- En haut de la page rajouter une balise `<style></style>`

```html
<head>
    <title>Ma page</title>
    <style>
        body {
            background-color: #f0f0f2;
            font-family: Arial, sans-serif;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
    </style>
</head>
```

Ou directement l'importer depuis un autre chemin dans l'environnement déployé : 
```html
<head>
    <link rel="stylesheet" href="css/reset.css">
    <link rel="stylesheet" href="css/style.css">
    <link rel="stylesheet" href="css/responsive.css">
</head>
```

#### Paramétrage

Le CSS permet de cibler et styliser les éléments HTML de différentes manières :
- 1. Cibler des éléments par balise (sans mettre de points)

```css
/* Cible tous les éléments <div> */
div {
    width: 600px; /* taille fixe des div*/
    margin: 5em auto; 
    padding: 2em;
    border-radius: 0.5em;
    box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
}
```

- 2. Cibler tous les éléments par rapport à leur classe : 
```css
.bouton-primaire {
    background-color: #38488f;
    color: white;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    margin: 0 auto;
}
```

```html
<button class="bouton-primaire">Bouton primaire</button>
```
<button class="bouton-primaire">Bouton primaire</button>

<style>
.bouton-primaire {
    background-color: #38488f;
    color: white;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    margin: 0 auto;
}
</style>

Paramétrages avancés : 

> Pour aller plus loin : @media https://developer.mozilla.org/fr/docs/Web/CSS/Reference/At-rules/@media pour le responsive

> Pour aller plus loin : @keyframes pour faire des animations simples : https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@keyframes


> [!TIP]+ Application à la conception de logiciel moderne
> En milieu professionnel, le CSS "pur" est rarement utilisé directement. On lui préfère des outils plus puissants qui évitent la duplication de code et facilitent la maintenance :
> - **Préprocesseurs** : `SCSS`, `SASS`, `LESS` (variables, fonctions, imbrication)
> - **Frameworks utilitaires** : `Tailwind CSS` (classes utilitaires prédéfinies)
> - **Bibliothèques de composants** : `Bootstrap`, `Material-UI`, `Ant Design` (composants prêts à l'emploi)
> 
> Ces outils permettent de standardiser le design, réutiliser du code et accélérer le développement.

Vous pouvez donc tout à fait utiliser de telles solutions dans vos projets (nous le valorisons).

Exemples : 

- [La page exemple cette fois ci stylisée](htmlcssstyle.html)
- [La page exemple avec le même style mais que des div 🤮](htmlcssdiv.html)
- [La page exemple avec tailwind 🌊](htmltailwind.html)

### Javascript 

JavaScript est un langage de programmation qui ajoute de l’interactivité aux sites web. Cela se produit dans les jeux, dans le comportement des réponses lorsque des boutons sont pressés, lors de la saisie de données dans des formulaires, avec des styles dynamiques, des animations, etc. Il permet de modifier le **DOM**, qui est le contexte rendu objet d'un site.

- **A retenir :** Les fichiers `javascript` : éléments de scripts du site, permettent de réaliser des actions sur des éléments `HTML` ou `CSS`.
#### Utilisation

Pour l'utilisation, soit on l'utilise inline : 
```html
<button onclick="alert('Bouton cliqué !')">Cliquez-moi</button>
```

Soit dans des fichiers séparés à importer dans le html:
```html
<script src="script.js"></script>
```

> **Remarque : évitez a tout prix de le mettre implémenté dans le corps du HTML de vos pages**

#### Un exemple
Exemple avec un bouton qui affiche un nombre aléatoire entre 0 et 1 :

(HTML)
```html
<button id="button"type="button">Affichage d'un nombre aléatoire</button>
<div id="affichage"></div>
```
(JavaScript)
```javascript
document.getElementById("button").onclick = function() {
    document.getElementById("affichage").innerHTML = Math.random();
};
```

<button id="button" type="button" style="display: inline-block; padding: 10px 20px; background-color: #007bff; color: white; font-size: 16px; border-radius: 5px; cursor: pointer; text-align: center; font-family: Arial, sans-serif;">Affichage d'un nombre aléatoire</button>
<div id="affichage"></div>
<script>document.getElementById("button").onclick = function() {
    document.getElementById("affichage").innerHTML = Math.random();
};</script>


> [!TIP]+ Pour aller plus loin
> Un codepen simple : https://codepen.io/rcyou/pen/QEObEZ


> [!NOTE] Plus concrètement
> Un codepen bien plus fourni : Une petite application statique qui va chercher des informations sur un pokemon avec une requête http en javascript : https://codepen.io/mikemilio/pen/poXOywy

> [!NOTE] Documentation associée au DOM HTML
> Le DOM : [https://www.w3schools.com/js/js_htmldom.asp](https://www.w3schools.com/js/js_htmldom.asp)


## Templating : Contexte

L'objectif aujourd'hui est de mettre a disposition une application comme un frontend, mais comment y gérer l'intelligence?

On peut tout coder dans une API et récupérer les données de notre API pour l'affichage (Voir module associé).

On peut également effectuer un rendu des pages côté serveur pour y incorporer des données requêtées par le serveur interne et donc de limiter la logique au maximum à des traitements internes a notre application : on parle alors de **Server Side Rendering**.

> Cela a différents avantages / inconvénients : notamment un meilleur contrôle des requêtes ou également la rapidité dans le cas d'un cache applicatif.

Nous allons donc ici nous intéresser au contenu des pages rendues et donc aux fichiers `html` des sites, qu'on va vouloir templatiser et renseigner en fonction de divers informations qu'on aura a l'intérieur de nos programmes.

Un petit historique : La plupart des applications web "anciennes" sont en PHP + JS + HTML + CSS pour utiliser la flexibilité du php pour la construction de pages `templatisées`. 

En python des applications du même type peuvent être construites avec le framework **Django** mais également a l'aide de **FASTAPI**  en faisant renvoyer des pages HTML templatisées.


## Templating Engine : Présentation de Jinja2

![ ](/images/frontend-initiation-et-serveur/template-page1.png)

Un **templating engine** (ou moteur de templating) est un outil utilisé pour générer dynamiquement du contenu, généralement du HTML, en combinant des données et un modèle de mise en page (template). On s'en sert en général pour la génération de pages `HTML` côté serveur, en injectant les résultats de traitements applicatifs dans des pages.

> NB: Cela permet de séparer la logique "service" de la logique vue (CF cours architecture)

Jinja2 est un moteur de template pour Python, largement utilisé dans les frameworks web comme Flask. Il permet de générer du HTML dynamique en utilisant des variables, des boucles et des conditions. Il est également utilisé dans une myriade d'outils d'où la présentation .

**Voyez plutôt** ce template, qui sert pour configurer un fichier a envoyer par mail.

```html
<html>
<body>
    <h1>Bonjour {{ user_name }},</h1>
    <p>Merci de vous être inscrit sur notre plateforme.</p>
    <p>Votre email : {{ user_email }}</p>
    {% if is_admin %}
    <p><strong>Vous avez des privilèges administrateurs.</strong></p>
    {% endif %}
    <p>Cordialement,</p>
    <p>L'équipe du support du site XXXX</p>
</body>
</html>
```

- Jinja2 utilise des doubles accolades `{{ }}` pour insérer des variables dynamiques dans un template. Ces variables sont remplacées lors du rendu du template.
- Jinja2 prend en charge les structures de contrôle comme les conditions (`if`), les boucles (`for`) et les filtres (`upper`, `lower`, `length` etc.). Ces structures permettent d'adapter le contenu rendu en fonction des données.

> Un peu de code 

```python
    with open(os.path.join(chemin_script,"template.html"),"r",encoding="utf-8") as fichier_template:
        contenu_fichier_template = fichier_template.read()
        template_rempli_non_admin=template.render(
            user_name=USER_NAME,user_email=USER_EMAIL,is_admin=False
        )
```
Avec les valeurs `user_name = "antoine"`, `user_email = "antoinebrunetti@gmail.com"` et `is_admin = True`

```html
<html>
<body>
    <h1>Bonjour antoine,</h1>
    <p>Merci de vous être inscrit sur notre plateforme.</p>
    <p>Votre email : antoinebrunetti@gmail.com</p>
    <p><strong>Vous avez des privilèges administrateurs.</strong></p>
    <p>Cordialement,</p>
    <p>L'équipe de support</p>
</body>
</html>
```



> [!NOTE] Pour aller plus loin 
> Documentation officielle [https://jinja.palletsprojects.com/en/stable/intro/](https://jinja.palletsprojects.com/en/stable/intro/)


> [!TIP] Exemple pour vous dans le cours
> Accédez a l'exemple conçu pour le cours et executez le : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/frontend-initiation-et-serveur/server-side-rendering/templating](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/frontend-initiation-et-serveur/server-side-rendering/templating)

## Server Side Rendering (SSR) : Exemple avec FastAPI

![ ](/images/frontend-initiation-et-serveur/ssr.png)


Le rendu côté serveur (Server Side Rendering, SSR) est une architecture où les pages HTML sont générées sur le serveur avant d'être envoyées au navigateur. Cela permet d'améliorer le temps de chargement initial.

> Remarque on utilise aussi souvent cela pour  le référencement (SEO), en particulier pour les moteurs de recherche qui ont du mal à indexer le contenu rendu côté client.

### Principe

Comme vu précédemment, `FastAPI` est un framework web rapide pour Python, il permet de mettre a disposition du code via HTTP.

En s'appuyant sur du templating `Jinja` de pages `HTML` on peut donc dynamiquement répondre a des requêtes utilisateurs en fabriquant les pages HTML côté serveur.

### Mise en oeuvre

Nativement, après l'installation de jinja2, fastapi permet l'usage des templates précédemment présentés : 

- `from fastapi.templating import Jinja2Templates`.

Il suffit d'organiser le code projet ainsi : 
```
projet/
│-- templates/
│   │-- index.html
│-- main.py # ou un autre module main
```

Et d'importer les templates ainsi : 

```python
from fastapi.templating import Jinja2Templates
templates = Jinja2Templates(directory="templates")
```

Exemple de projet simple : [lien vers l'exemple-minimal](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-4/fastapi-web-templating/exemple-minimal)

Il existe beaucoup d'éléments de configuration, inspirez vous de la documentation et des exemples de code.

> [!NOTE] Pour aller plus loin 
> La documentation fonctionnelle est ici [https://fastapi.tiangolo.com/advanced/templates/](https://fastapi.tiangolo.com/advanced/templates/)


> [!TIP] Exemple pour vous dans le cours
> Un exemple plus concret pour vous inspirer pour vos projets : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/frontend-initiation-et-serveur/server-side-rendering/fastapi-web-templating/app](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/frontend-initiation-et-serveur/server-side-rendering/fastapi-web-templating/app)



## Développer un site web : Hébergement et serveur web

Lorsque vous développez un site web, vous pouvez éditer simplement des fichiers puis ouvrir des fichiers en local, mais cela à des limites:
- Les chemins relatifs dans les fichiers peuvent ne plus fonctionner
- On ne teste pas la réalité de ce qu'on développe, on hébergera notre site sur un serveur web.

### Privilégier de travailler avec un serveur web

Il est donc fortement recommandé de toujours travailler avec un serveur web local pendant le développement. Cela permet de limiter les problèmes au moment de l'hébergement (**portabilité**)

```sh
python -m http.server 8000
```

**Remarque Importante** :

Les serveurs applicatifs type **uvicorn** / **django** peuvent servir des **ressources statiques** et les serveurs frontend comme le serveur de travail en local proposé par **vite** ([voir le cours sur reactjs](/cours/frontend-react-js/)) feront tout à fait l'affaire, mais il faut garder en tête qu'un site doit être hebergé et donc penser la portabilité dès le développement.

**Cependant**, il faut garder en tête qu'un site doit être **hébergé en production** sur un serveur web. Il est donc crucial de **penser la portabilité dès le développement**. 

### Architecture associée au frontend

#### Architecture site web basique : 
```
mon-site/
├── index.html              # Page d'accueil
├── css/
│   ├── style.css          # Styles principaux
├── js/
│   ├── script.js          # Scripts principaux
│   └── utils.js           # Fonctions utilitaires
├── pages/
│   ├── about.html         # Page à propos
│   ├── contact.html       # Page contact
│   └── page-3.html     # Autre page
└── images/                 # gestion des logos a part
    ├── logo.png
    └── banner.jpg
```
> Avec index.html la page d'accueil (c'est implicite)

#### Architecture d'une appli SSR : MVC

**Fastapi** : Voir l'exemple disponible [ici]()
```
├── app
│   ├── dao
│   │   └── recipe_dao.py
│   ├── main.py
│   ├── modelc
│   │   └── recipe.py
│   ├── README.md
│   ├── requirements.txt
│   ├── routers
│   │   └── recipe_router.py
│   ├── service
│   │   └── recipe_service.py
│   ├── static
│   │   ├── css
│   │   │   └── style.css
│   │   ├── favicon.ico
│   │   └── js
│   │       ├── recipes.js
│   │       └── script.js
│   └── templates
│       ├── base.html
│       ├── index.html
│       ├── new_recipe.html
│       ├── recipe_detail.html
│       └── recipe_list.html
```

**Django**: Voir l'exemple disponible [ici]()

```
├── app
│   ├── README.md
│   ├── recipe_project
│   │   ├── manage.py
│   │   ├── recipe # application recettes
│   │   │   ├── admin.py
│   │   │   ├── apps.py
│   │   │   ├── forms.py
│   │   │   ├── __init__.py
│   │   │   ├── jinja2.py
│   │   │   ├── migrations
│   │   │   │   ├── 0001_initial.py
│   │   │   │   └── __init__.py
│   │   │   ├── models.py
│   │   │   ├── service.py
│   │   │   ├── templates
│   │   │   │   ├── base.html
│   │   │   │   ├── index.html
│   │   │   │   ├── new_recipe.html
│   │   │   │   ├── recipe_detail.html
│   │   │   │   └── recipe_list.html
│   │   │   ├── tests.py
│   │   │   ├── urls.py
│   │   │   └── views.py
│   │   ├── recipe_project # application config serveur
│   │   │   ├── asgi.py
│   │   │   ├── __init__.py
│   │   │   ├── settings.py
│   │   │   ├── urls.py
│   │   │   └── wsgi.py
│   │   ├── static
│   │   │   ├── css
│   │   │   │   └── style.css
│   │   │   └── favicon.ico
│   │   └── templates
│   │       └── recipes
│   │           ├── base_template.html
│   │           ├── index.html
│   │           ├── new_recipe.html
│   │           ├── recipe_detail.html
│   │           └── recipe_list.html
│   └── requirements.txt
```

> [!NOTE] **Que retenir : Les concepts qu'on applique au frontend sont les mêmes qu'au backend** 
> - Portabilité dans le design
> - Découpage en couches
> - La qualité de code, pattern et respect des bonnes pratiques : [Voir le cours dédié](/cours/bonnes-pratiques-et-design-patterns/)