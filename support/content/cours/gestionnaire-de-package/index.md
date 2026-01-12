---
title: "Gestionnaire de package, partage de code, industrialisation"
weight: 27
description: "Comprendre la portabilité applicative via l'utilisation de dépendances logicielles"
summary: "Comprendre la portabilité applicative vis l'utilisation de dépendances logicielles"
slug: "gestionnaire-de-package"
tags: ["cours","portabilite", "librairie","package", "automatisation","environnement"]
series: ["Cours"]
series_order: 3
---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : 
https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/gestionnaire-de-package


Ce cours présente comment utiliser, mettre en place et partager un environnement de code fonctionnel. Cela assure la portabilité de votre application pour que l'on puisse l'utiliser ailleurs que sur votre poste. 

> Exemple :
> https://coder.com/blog/it-works-on-my-machine-explained

Pour information : Les exemples présentés dans le cours seront disponibles ici : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-3](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-3) 


### Accroche

Depuis un terminal, faites les commandes suivantes :
- `pip install helloensai`
- `python3 -m helloensai`



<details><summary  class="reponse" ><b> Que s'est il passé ? </b></summary>
<p>

- Vous avez téléchargé un package avec un gestionnaire de paquet : `pip`
- Ce package a ensuite été importé et utilisé directement en ligne de commande via python3, installé sur votre machine
- Il a executé du code python : un print dans votre console.

> **Sidequest 🐧** : faire en sorte de changer l'output du lancement de la commande `python3 -m helloensai` a partir du message dans la console.



</p></details>

## Interpréteur/Compilateur


![ ](images/gdp/compile-interprete.png)


> Référence : https://blog.amigoscode.com/p/java-vs-python
### Python, un language interprété

Python appartient à la catégorie des langages interprétés (tout comme Javascript et R, par exemple).  
Un langage interprété possède un interpréteur (on parle aussi de `runtime`)
Pour exécuter un code d'un langage interprété, il faut 2 choses :

- Le code source à exécuter : votre fichier `main.py` par exemple
- Un interpréteur : Pour python, il s'agit de la commande `python` (`python3` sur certains systèmes pour le distinguer de python 2, `python.exe` sur certains systèmes d'exploitation inférieurs)

Pour vérifier que l'interpréteur `python` est bien disponible sur le système, on peut lancer la commande

```python
python --version
python3 --version
```

> **Remarque** : il peut être disponible sur votre système mais pas défini dans un endroit connu du système voir : https://medium.com/towards-data-engineering/understanding-the-path-variable-in-linux-2e4bcbe47bf5

> **Sidequest 🐧** : si vous vous demandez où `python` est installé, vous pouvez utiliser la commande `which` (ou `where` pour les systèmes Windows)


### Compilateur Python : Cpython et le bytecode

Même si Python est qualifié de *langage interprété*, l’exécution du code passe en réalité par une **étape de compilation**.

Dans la très grande majorité des cas, lorsque l’on parle de Python, on parle de **CPython**, l’implémentation officielle du langage.

> En pratique, CPython compile le code source Python en *bytecode*, qui est ensuite exécuté par une machine virtuelle (PVM).

Ce bytecode est stocké dans des fichiers `.pyc`, généralement situés dans un dossier `__pycache__`.

- Le bytecode n’est **pas du code machine**
- Il est spécifique à une version de Python
- Il permet d’accélérer les exécutions suivantes

L’exécution se fait ensuite via la **Python Virtual Machine (PVM)**, intégrée à l’interpréteur.

> **Remarque** : le dossier `__pycache__` peut être supprimé sans risque, il sera recréé automatiquement par Python si nécessaire.

> [!TIP]+ Pour aller plus loin : PVM
> https://www.geeksforgeeks.org/python/python-virtual-machine/)

### Librairies natives Python : C et Cython


Pour des raisons de performance, certaines parties de l’écosystème Python ne sont **pas écrites en Python**, mais en **C**.

C’est le cas de nombreuses bibliothèques populaires comme :
- `NumPy`
- `pandas`
- `scikit-learn`

Pour faciliter l’écriture de ce type de modules, on utilise souvent **Cython**.

Cython est :
- Un **superset de Python** (du Python avec des extensions)
- Un langage qui se **compile en C**, puis en code machine
- Un outil permettant de créer des **modules utilisables directement en Python**

Une fois compilé, un module Cython :
- S’importe avec `import` comme n’importe quel module Python
- S’exécute en code machine, donc beaucoup plus rapidement
- Est chargé et utilisé par l’interpréteur Python

> **Important** : l’utilisateur final n’a pas besoin de Cython pour utiliser ces bibliothèques, seulement de Python et du module compilé.

> [!TIP]+ Un exemple
> Un exemple de code Cython est disponible sur le [dépôt d'exemples]()

### Transpileurs

Il existe également des outils appelés **transpileurs**.

Un transpileur permet de :
- Convertir un langage source vers un autre langage
- Tout en restant à un niveau d’abstraction similaire

**Il existe également ce qu’on appelle les polyfills : ils proposent des implémentations de fonctionnalités récentes compatibles avec les anciennes versions.**.


Par exemple :
- TypeScript → JavaScript ⇒ transpileur (exemple : `babel`)
- JavaScript moderne → JavaScript plus ancien ⇒ transpileur (exemple : `babel`)
- Fonctionnalités manquantes à l’exécution du code ancien ⇒ polyfills (exemple : `core-js`)

Contrairement à un compilateur classique :
- Le résultat d’un transpileur est **du code source**
- Et non un binaire ou du bytecode


> [!TIP]+ Documentation pour approfondir
> https://fr.javascript.info/polyfills


## Environnement d'exécution

![](https://imgs.xkcd.com/comics/python_environment_2x.png)
> Source : https://xkcd.com/1987/

> Cette partie s'attele a présenter ce qu'on appelle l'environnement d'exécution. 

L'environnement d'exécution, est l'ensemble des paramètres constituant le contexte d'exécution du code:
- L'OS et les fonctions noyau liées (puisque les fonctions des programmes s'appuyent dans leur coeur a des fonctions noyau)
- Les librairies / modules installés
- Pour la version de python
- La configuration du code

### Préambule : Qu'est ce qu'un gestionnaire de paquets

Lorsque vous voulez travailler avec des fichiers informatiques, les gestionnaires de paquets sont là pour vous.

Ils permettent notamment de :

- **installer, mettre à jour ou désinstaller** des logiciels, bibliothèques ou outils.
- **partager et réutiliser** facilement des outils entre différents projets
- s’appuyer sur des **dépôts centralisés (ou registres)** qui contiennent les packages disponibles

> [!TIP]+ Exemple: en python
> `pip` est le gestionnaire de paquets officiel.  
> Il permet d’installer des bibliothèques depuis [PyPI (https://pypi.org/)](https://pypi.org/), le dépôt central de packages Python.


Il en existe une Myriade, pour différents usages, languages.
- `apt`, `snap` pour l'installation de paquet *debian* de manière simplifiée
- `chocolatey` `scoop` `brew` pour l'installation de logiciels *MacOSX* et *Windows*
- `pip` `poetry` pour l'installation de modules *python*.
- `npm` `yarn` pour l'installation de packages *javascript*
- `maven` `gradle` pour l'installation de packages *java*
- `docker` `podman` pour l'installation d'images docker

Il faut en général pour définir un gestionnaire de paquets : 
- Une norme sur le format partagé (.whl, .tar, ...)
- Une définition d'un standard pour un hébergeur : `pypi.org` `dockerhub` ... Cela permet d'héberger soit même et de partager sur des hébergeurs
- Une définition sur le mode de récupération du format a partir d'un serveur central.


### Qu'est ce que pip

**pip** c'est un gestionnaire de paquets pour python

C'est l'installeur de premier choix quand il s'agit d'ajouter des dépendances (modules) à un projet python.

Il s'utilise principalement pour l'installation ou la desinstallation de module sur votre système.
```
pip uninstall <package>
pip install <package>
```

<a href="https://pip.pypa.io/en/stable/cli/pip_install/">
Lien vers la référence de la commande
</a>

```
pip uninstall <package>
```

<a href="https://pip.pypa.io/en/stable/cli/pip_uninstall/">
Lien vers la référence de la commande
</a>

**Tout cela avec des petites verifications pour éviter de télécharger les mauvaises dépendances**

> Remarque la commande peut également être pip3 selon votre environnement.

### Modules et dépendance

<img src="https://imgs.xkcd.com/comics/dependency.png" />

> Source https://xkcd.com/2347/

- Module : Un module Python est un fichier contenant du code Python (fonctions, classes, variables) pour structurer et réutiliser le code. On peut l'importer avec import.
- Dépendance : Une dépendance est une bibliothèque externe nécessaire à un projet, comme matplotlib pour créer des graphiques, requests pour faire des requetes http, psycopg2 pour faire des connexions a des bases de données postgresql...

> Fonctionnellement, a part le contrat d'interface sur l'import, le fonctionnement est équivalent, une fois l'import effectué.


### Wheel : Le format de référence


Lorsque vous installez des packages par l'extérieur vous utilisez déjà probablement des fichiers wheel ou `.whl`.

Une wheel est essentiellement un zip, ou tar qui a un nom qui peut être parsé par pip pour lui permettre de l'installer de manière adaptée sur votre environnement cible.

Regardez plutôt : `{dist}-{version}(-{build})?-{python}-{abi}-{platform}.whl`

> Exemple : <a href="https://files.pythonhosted.org/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl">`requests-2.32.5-py3-none-any.whl`</a>

> Ce qui donne: `{requests}-{2.31.0}-{py3}-{none}-{any}`

Ce genre de spécification est commune a tous les languages dignes de ce noms, puisqu'harmoniser un mode de livraison pour un gestionnaire de paquets harmonisés et cohérents, cela permet une compatibilité assurée entre les différents outils qui les utilisent.
<div class="alert alert-info">
  <strong>Pour aller plus loin : lisez de la doc, c'est bien</strong> <br/> 

Les PEP ou Python Enhancement Proposal sont les évolutions et propositions d'évolutions du language.

Les principales propositions d'améliorations de python qui concernent le packaging sont les suivantes :

Comment packager ? => Wheels : [PEP 427](https://peps.python.org/pep-0427/).

Comment sont construit les numeros de version. Par quelle sémantique ? => [PEP 440](https://peps.python.org/pep-0440/) 

Définir les dépendances d'un package pour permettre la résolution des dépendances mutuelles : [PEP 508](https://peps.python.org/pep-0508/)

Comment build ? [PEP 517](https://peps.python.org/pep-0517)

Index du "tuto" Pypi pour construire et déployer un livrable : [**Build and publish section**](https://packaging.python.org/en/latest/guides/section-build-and-publish/)

</div>


### Environnements virtuels : Isolation 

![](images/gdp/venv.jpg)

Pour une isolation des paquets installés, et ne pas utiliser tout ce qui existe déjà sur un poste, python permet l'utilisation d'environnements virtuels (virtualenv ou venv).

Ils s'installent au travers du module venv installé comme un module de votre python ex :

`python3 -m venv ./venv`

> Cela installe un environnement dans le sous dossier ./venv par rapport au terminal executant le module.

> Note: cet environnement ne doit pas être versionné et donc votre gitignore doit bien le gérer.

Une fois mis en place, vous pouvez le lancer en utiliser la commande en fonction de l'OS:

| Environnement | Terminal   | commande                       |
| ------------- | ---------- | ------------------------------ |
| MacOs         | bash       | `source <venv>/bin/activate`   |
| Linux         | bash       | `source <venv>/bin/activate`   |
| Windows       | cmd.exe    | ` <venv>\Scripts\activate.bat` |
| Windows       | powershell | ` <venv>\Scripts\Activate.ps1` |

**différents moyens vérifier que vous êtes dans un venv**
    - pip list --local (il n'y a pas grand chose)
    - Vous avez maintenant une parenthèse vous indiquant que vous êtes bien dans votre venv dans votre terminal
    - consulter l'interpreteur python dans votre vscode

💥 Attention à ne pas le versionner toutefois, réferez vous au **.gitignore** du module [git avancé](/cours/git-avance/) pour plus d'informations

🏁 maintenant vous pouvez mettre en place l'environnement via pip install -r requirements.txt par exemple
<div class="alert alert-info">
  <strong>Pour aller plus loin : les venvs packagés</strong> <br/> 

Une solution classique pour un statisticien est d'utiliser l'environnement virtuel **conda**, il propose un gestionnaire de package integré dans un environnement integré prêt a l'emploi pour le statisticien.

Par exemple il y a le package **pandas** très utilisé pour le travail sur des dataframe.

Cela peut notamment être utile dans des environnements contraints de passer par des venvs packagés afin de pouvoir travailler librement avec des outils rendus portables par le mode de livraison de **conda**

Documentation ici : https://docs.anaconda.com/getting-started/
</div>

## Licences : dans quel cadre peut-on utiliser du code tierce ?

![](images/gdp/license.png)

Lorsque l’on utilise une bibliothèque (open source), on n’utilise pas uniquement du code, on accepte également une **licence**, qui définit les droits et obligations liés à son utilisation.

> **Open source ne signifie pas sans règles.**

### **Pourquoi les licences sont importantes**

Chaque package open source est distribué avec une licence qui définit **ce que vous pouvez et ne pouvez pas faire** avec le code.  
Par exemple, elle peut préciser si vous pouvez :

- utiliser le package dans un projet personnel ou commercial,
- le modifier pour l’adapter à vos besoins,
- redistribuer votre version du code.

### **Exemple concret** 

La bibliothèque `requests` en Python utilise la licence [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).

Comment le voir ? 
- Soit directement sur le dépôt PyPI : ici https://pypi.org/project/requests/ (onglet métadonnées)
- Soit directement sur votre environnement d'éxécution : `pip show requests`

> Cette license spécifie : vous pouvez utiliser un package Apache 2.0 dans votre projet, le modifier, le vendre ou le distribuer, tant que vous gardez la notice de licence.

À l’inverse, certaines licences plus strictes, comme la [GPL](https://www.gnu.org/licenses/gpl-3.0.html), obligent à publier votre code sous la même licence si vous redistribuez un logiciel qui utilise une bibliothèque sous GPL. **On parle alors de copyleft**.

> [!TIP]+ Comparatif des licences open source
> https://choosealicense.com/licenses/


{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
Pour tout projet, il est attendu de déclarer une License adaptée dans le coeur du projet (fichier LICENSE à la racine). Cela permet de préciser les droits et obligations liés à l'utilisation et à la redistribution de votre code.
{{</alert>}}


## Reproductibilité et portabilité

![](images/gdp/port.png)




Un des enjeux dans la conception logicielle est de construire des applicatifs qui sont maintenable et facile à faire évoluer.

Une pratique essentielle à incorporer dans vos développement et de penser à **canoniser** l'environnement d'exécution pour permettre 

### Pip et un fichier canonique : *requirements.txt*
![](images/gdp/requirements.png)
Pour mieux partager un environnement qui permet de faire tourner le code,
**pip** propose de sanctuariser les dépendances dans un fichier **requirements.txt**.

Exemple de fichier requirements.txt: 
```
fastapi
uvicorn
requests
pytest==5.6.7
selenium==1.0.2
flake8
```

Ce fichier permet de simplifier l'installation des dépendances en les regroupant dans un fichier, on peut ensuite demander aux utilisateurs/développeurs de faire : 

```
pip install -r requirements.txt
```

**Le fichier requirements.txt doit être versionné avec votre code sur git**

> Bien évidemment, cela est pertinent a condition d'être dans un environnement virtuel **nu**

**Remarque**: On peut également copier un environnement d'exécution fonctionnel a partir de la fonction `pip freeze`

```sh
pip freeze > requirements.txt
```

> Remarque, pip freeze ne fait que des opérations très basiques (lister l'environnement et le sortir dans un message). Il faut donc soit partir d'un environnement d'abord propre (environnement virtuel puis installation de toutes les dépendances), ou utiliser une autre librairie 

On préconisera plutôt `pip-tools` pour sauvegarder un environnement propre:

```sh
pip install pip-tools
pip-compile requirements.in
```
### Industrialisation : Poetry, UV

![image source : https://medium.com/@techne98/a-beginners-guide-to-the-uv-package-manager-in-python-eb677460a5bc](/images/gdp/uv.webp)


`pip` est le gestionnaire de package préconisé par défaut pour python, mais il réside dans son design différentes limites : 


- Gestion des conflits dans l'installation de packages
- Mauvaise gestion des versions de python // paquets
- Praticité de la réalisation de package sur Pypi
- Gestion fine des dépendances pour les environnements d'execution du code (on ne veut pas les dépendances liées au tests ou a des lignes de commandes qu'on souhaite lancer sur le projet par exemple)
- Gestion des venv pour le projet par rapport aux prérequis (version de python etc..)
- Perennité dans l'installation de package figeant dans le temps une version fonctionnelle du code.

Ces limites sont en général gérées côté utilisateur par l'installation de plusieurs versions de python et la gestion d'environnement isolés dans chaque projet via `venv`. Mais cela peut s'avérer lourd et pas nécessairement facile à industrialiser.

Précedemment on présentait [poetry]() mais il tend a être déprécié pour `uv`, plus rapide et simple a prendre en main par datascientist et dévs.


> [!TIP]+ Pour les curieux
> https://medium.com/@hitorunajp/poetry-vs-uv-which-python-package-manager-should-you-use-in-2025-4212cb5e0a14 et [doc prise en main poetry supprimée](/blog/prise-en-main-poetry)



#### Démarrer avec UV

UV propose plus que pip, il permet la gestion : 
- des environnements virtuels python
- des installations de python sur le poste
- des projets et des besoins python pour le projet
- des paquets sur le projet

L'installation est simplifiée (linux):
```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Sur un projet ayant déjà `uv` l'installation d'un environnement virtuel est simplifié : 
```sh
uv sync
```
**Cette commande est centrale dans l'utilisation d'uv**:

- Elle cherche une version de python correspondant au projet `pyproject.toml` sur votre système, si elle n'existe pas l'installe.
- Crée un environnement virtuel python dans le projet.
- Installe les dépendances dans l'environnement virtuel.

> Vous pouvez ensuite activer l'environnement comme vu précédemment `source ./venv/bin/activate`
> Ou utiliser une commande directement `uv run python ...`

On peut ensuite faire un balayage des commandes: 
- `uv add` permet d'ajouter des librairies dans la configuration du projet
- `uv remove` les enlève
- `uv init` créer les fichiers de base pour uv dans un projet une `main.py`, un `pyproject.toml` et un `README.md`

#### Lien avec l'environnement cible : grille de compatibilité et lockfile

Un enjeu majeur dans les projets logiciels est de garantir la reproductibilité parfaite de l'environnement d'exécution. 

Dans la plupart des languages, on privilégie plutôt de la gestion des dépendances plus aboutie:
- Besoin de distinguer ce qui est nécessaire dans l'environnement : **version de python**, **dépendances et leurs versions exactes**. 
- De ce qui est nécessaire pour le travail **dépendances pour le formattage, test..**.

Mais également :

Spécifier simplement des versions dans requirements.txt ne suffit pas à assurer qu'un environnement sera identique d'une installation à l'autre.

UV (comme Poetry, npm, ou d'autres) utilise un fichier de verrouillage **lockfile**(`uv.lock`) qui enregistre :

- Les versions exactes de toutes les dépendances, **directes** et [**transitives**](https://xygeni.io/fr/sscs-glossary/what-is-transitive-dependency/), résolues à un instant **T** dans un environnement bien identifié:.
- Les hash cryptographiques (SHA-256) de chaque package téléchargé
- Les métadonnées de résolution (quelle version de Python, quelle plateforme, etc.)

> C'est donc un fichier à versionner

#### Que retenir ?

On souhaitera plutôt favoriser, pour des projets logiciels plus amples, des gestionnaires de paquets industriels:
- `uv`et `poetry` pour du python
- `npm` `yarn` `pnpm` pour js
- `maven` `gradle` pour du java

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
Vous êtes libre dans le choix du gestionnaire de package (vous pouvez également vous renseigner sur  : poetry, pipenv), mais vous devrez respectez la règle suivante : votre code devra pouvoir être récupéré et lancé de manière explicite (quelles commandes permettent d'arriver a un environnement fonctionnel dans le fichier **README.md** dans une section dédiée)
{{</alert>}}


## Dépots distants : Vers la publication
![](https://realpython.com/cdn-cgi/image/width=1920,format=auto/https://files.realpython.com/media/How-to-publish-an-open-source-Python-package-to-PyPI_Watermark.0f9c2da132aa.jpg)
Les gestionnaires de paquets permettent a la fois d'utiliser des paquets existants, mais vous le devinez bien, il est possible d'en créer vous même.

Les dépots de packets peuvent être :

- privés - c'est le cas dans les entreprises en général
- publics :
  - c'est le cas du dépôt pypi https://pypi.org/ , vers lequel pointe par défaut une installation de pip.
  - mais également du dépôt de test : https://test.pypi.org/

L'idée de la création d'un package est de créer une brique réutilisable de composants fonctionnels.

Cela peut être par exemple la réutilisation de classes entre différents projets ou la sauvegarde d'un sous ensemble de fonctions utiles que vous aimez utiliser sur les différents projets sur lesquels vous travaillez.

Un package en python à cette forme :

```sh
projet
├── LICENSE
├── src/
│   └── package
│       ├── __init__.py
│       └── t.py
├──  tests/
├── README.md
└── pyproject.toml
```
> [!TIP]+ Sources
> - https://peps.python.org/pep-0517/
> - https://peps.python.org/pep-0518/

**Remarque**:
Les librairies installées (modules externes) se retrouvent directement dans lib : soit dans l'environnement virtuel, soit directement là ou est installé python sur votre poste et c'est comme cela que VSCODE réalise la résolution des imports.

La publication avec UV est simplifiée : 
- A partir d'un projet bien constitué => `.pyproject.toml` et `README.md` bien constitué

Constitution d'un fichier `.whl` du code
```sh
uv build
```

Publication des fichiers `.whl` du projet
```sh
uv publish --token ....
```

Les tokens pouvant être récupérés directement en créant un compte sur l'instance de PyPi qui vous intéresse : 
- https://pypi.org/
- https://test.pypi.org/


{{< alert icon="bulb" cardColor="#757bffff" iconColor="#1d3557" textColor="#f1faee" >}}
À retenir : un gestionnaire de paquets permet de réutiliser du code externe et de fournir des environnements reproductibles, afin que chacun puisse installer et exécuter un projet de manière fiable et cohérente.
{{</alert>}}

> Une fois l’environnement sauvegardé, sa mise en place peut être automatisée, soit via des fichiers Make, soit en utilisant des environnements conteneurisés, garantissant ainsi des projets prêts à l’emploi sur n’importe quelle machine. C'est ce qu'on verra dans la partie dédiée (à venir).

## TP : Quelques questions

- Si vous ne l'avez pas fait, installez le package `helloensai` avec pip et lancez le via une commande python. 

- Consultez les projets exemples : [1](https://github.com/conception-logicielle-ensai/archi-exemple) , [2](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/gestionnaire-de-package/uv-publish), [3](https://github.com/conception-logicielle-ensai/predicat). 
Répondez aux questions suivantes : est ce que le projet détaille comment arriver sur le projet? quel est le gestionnaire de paquet privilégié pour le projet ?


