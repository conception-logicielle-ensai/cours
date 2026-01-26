---
title: "Etats et Persistence des données"
weight: 20
draft: false
summary: "Gestion des états des applications, base de données"
slug: "etats-persistence-donnees"
tags: ["postgresql", "bdd","sqlite","mongodb","duckdb","backend"]
series: ["Cours"]
series_order: 10
---

> **Comme pour les autres parties, les exemples sont disponibles ici : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/etats-persistence-donnees](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/etats-persistence-donnees)**

Lorsque l'on execute un programme, il utilise une mémoire qui lui est allouée pour pouvoir fonctionner. 
A la fin de l'execution du programme la mémoire est désallouée et donc tout ce qui était dans le contexte d'execution du programme n'est plus accessible.

C'est ainsi que vient la notion de persistence des données qui se présente comme la nécessité de pouvoir sauvegarder les résultats et états du système pour conserver des éléments déjà traités.

En général, on sépare le stockage des données d'un programme entre des fichiers et des bases de données.


## Fichiers

<img style="max-width:20%;margin-left: auto; margin-right:auto" src="https://cdn-icons-png.flaticon.com/512/3979/3979301.png" />

Les fichiers peuvent avoir différents formats pour stocker de la donnée

- les fichiers au format de texte: **csv**, **txt**, **xml**, **json**, **yaml**
- les fichiers au format binaire : **parquet**, **zip**, ...

Les fichiers peuvent être ensuite importés ou exportés depuis le code, exemple avec du `json`:

```python
import json

# Données à sauvegarder
data = {"utilisateurs": [{"nom": "Alice", "age": 30}, {"nom": "Bob", "age": 25}]}

# Écriture dans un fichier JSON
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# Lecture du fichier JSON
with open("data.json", "r") as f:
    data = json.load(f)
    print(data)
```

Les fichiers produits par le traitement sont initialement stockés localement sur la machine exécutant le processus (par exemple le poste de développement en environnement local). Ils peuvent ensuite être synchronisés vers des solutions de stockage dédiées, mieux adaptées à leur conservation et à leur diffusion.

**Rappel: de tels fichiers ne doivent pas être versionnés, un mode opératoire pour les récupérer doit être spécifié**


À titre d’exemple, les fichiers générés en fin de traitement peuvent être déposés dans un service de stockage objet tel que S3, qui offre une solution économique, scalable et simple d’accès pour l’hébergement de données non structurées.

> C'est un service que proposent les fournisseurs cloud, et également le datalab SSPCloud, au travers du service **S3 dédié**.


<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> <br/>
    Stockage objet, client s3 python : <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html">boto3</a>
</div>

<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> 

Documentation d'utilisation du stockage S3 sur les espaces du datalab sspcloud : 
[https://docs.sspcloud.fr/content/storage.html](
https://docs.sspcloud.fr/content/storage.html)

</div>


## Systèmes de gestion de base de données (SGBD)

<img style="max-width:60%;margin-left: auto; margin-right:auto" src="https://www.ovhcloud.com/sites/default/files/styles/large_screens_1x/public/2019-09/enterprise-cloud-database.png" />

Dans cette partie nous nous interesserons spécifiquement aux bases de données `relationnelles` et `document`.

> **Remarque : Lors de vos choix de conception , il faudra privilégier une base de données cohérente avec votre architecture si vous avez besoin de persister des données en base de données**


### Bases de données relationnelles

Les bases de données de données relationnelles sont des bases dans lesquelles les données sont organisées sous forme de table.
On les préconise dans des cas où l'architecture sous tend une structure des données fixe. 

Elles offrent un cadre robuste pour garantir la **cohérence des informations** et permettent d’exprimer efficacement des requêtes complexes.

En contrepartie, l’évolution du schéma demande une certaine rigueur : toute modification structurelle implique généralement des migrations. Par ailleurs, ce modèle est moins adapté au stockage de données peu structurées ou fortement imbriquées.

> Exemples : SQLite, DuckDB, MySQL, MariaDB,  PostgreSQL

### Bases de données orientées document

Les bases de données document permettent le stockage de données sous des formats flexibles, a base de document généralement en `JSON` ou `BSON` (compressé).

Les documents n'ont pas a respecter un schéma propre et puisqu'il n'y a pas de contraintes, les bdd document sont optimisées pour des requêtes sur le contenu des documents.

> Par exemple: Logs, structures json, ...

Cette flexibilité rend ces bases particulièrement adaptées aux cas où le modèle de données évolue fréquemment ou n’est pas entièrement connu à l’avance. 

Les requêtes s’exécutent directement sur le contenu des documents, ce qui permet de bonnes performances pour des applications nécessitant une forte scalabilité ou manipulant des données complexes et hétérogènes, comme des profils utilisateurs ou des configurations applicatives.

> Par exemple si l'on doit traiter des données venant de plusieurs sources différents qu'on souhaite juste stocker / cacher et qu'il n'y a pas de modèle fixe.

En revanche, l’**absence de schéma et de contraintes fortes** peut compliquer la gestion de l’intégrité des données. De plus, ce type de base est généralement moins adapté aux scénarios impliquant de nombreuses relations entre entités ou des transactions complexes couvrant plusieurs ensembles de données.

> Exemples de SGBD NoSQL orienté Document: MongoDB, CouchDB (accessible en via api rest)...

## Fiabilité et Performance des Bases de Données

### Transactions et ACID

<img style="max-width:60%;margin-left: auto; margin-right:auto" src="https://www.scylladb.com/wp-content/uploads/acid-diagram.png" />

Les bases de données relationnelles implémentent les principes ACID (`Atomicité` `Cohérence` `Isolation` `Durabilité`) pour leurs transactions.

> Remarque: Une transaction comment par un `BEGIN` et finit au `COMMIT`. On peut donc réaliser plusieurs modifications dans une même transaction.

#### **Atomicité** 
Une transaction est toujours **tout ou rien**

Exemple:

Dans le cas d'un transfert bancaire, si il y a un dysfonctionnement il n'y aura pas de situation ou une personne reçoit de l'argent que l'autre personne a encore dans son compte en banque.

#### **Cohérence**
Une transaction assure que les données transférées ne sont pas corrompues ou respectent des règles fixées dans le modèle (pas d'identifiant null...)
> Cela permet de stopper un traitement en cas de corruption et de rollback

#### **Isolation**
Les transactions s'executent indépendamment les unes des autres.
> Cela permet d'éviter les conflits
#### **Durabilité**
Les données, après la transaction sont bien persistées même si il y a une panne.


#### **Applications aux transactions au niveau du code python**

On peut donc effectuer plusieurs modification au sein d'une même transaction, exemple (avec psycopg2).

```python
import psycopg2

try:
    # il faut executer le script rollback-initdb.sql pour tester
    conn = psycopg2.connect(dbname="postgres", user="postgres", password="postgres", host="localhost")
    cursor = conn.cursor()
    cursor.execute("BEGIN")
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Alice", 25))
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Bob", 35))
    conn.commit()
except Exception as e:
    conn.rollback()
    print("Erreur :", e)
finally:
    cursor.close()
    conn.close()
```


### Formes normales

Les formes normales décrivent des règles qui assurent la bonne organisation des données et évitent la redondance.

- **1ère forme normale (1NF) :** Chaque colonne contient des valeurs atomiques, et chaque ligne est unique.

Il faut donc ici ne pas mettre de valeurs qui contient une liste de valeurs, on privilégie de mettre en place 
- **2ème forme normale (2NF) :** Respecte la 1NF et assure qu'il n'y a pas de dépendances partielles.

Il ne faut pas que des champs soient interdépendants entre eux exactement. (un champ ne doit pas être en correlation totale avec un autre champ)

- **3ème forme normale (3NF) :** Respecte la 2NF et supprime les dépendances transitives.

Il faut ici que les champs dans nos tables soient non correlés des champs que l'on pourrait retrouver sans jointure directe par des clés dans une autre table.

Soit encore : 

Il faut ici que les informations dans une table soient directement liées à la clé primaire de cette table, et non à une colonne non-clé **(2NF)**. Les colonnes non-clé ne doivent pas dépendre les unes des autres, et toute relation entre les informations d'autres tables doit se faire via des clés étrangères et non par des dépendances directes entre colonnes non-clé **(3NF)**.

#### **Exemple**

Voici un exemple présentant une table qui respecte la 1ère forme normale mais pas la 2ème.

```sql
CREATE TABLE students_courses (
    student_id INT,
    student_name VARCHAR(100),
    course_name VARCHAR(100),
    course_instructor VARCHAR(100),
    PRIMARY KEY (student_id, course_name)
);

INSERT INTO students_courses (student_id, student_name, course_name, course_instructor) 
VALUES
(1, 'Bob', 'Math', 'Dr. Smith'),
(1, 'Bob', 'Physics', 'Dr. Brown'),
(2, 'Alice', 'Math', 'Dr. Smith'),
(2, 'Alice', 'Biology', 'Dr. White');
```

- On voit bien ici qu'il y a atomicité des données ligne a ligne, par contre on repète les informations dépendantes : le cours et le professeur sont correlés.

Il faut donc découper en 2 tables avec une table d'association, pour isoler les données venant du cours, des données des élèves.

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    course_instructor VARCHAR(100)
);

CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

```sql
INSERT INTO students (student_id, student_name) VALUES
(1, 'Bob'),
(2, 'Alice');

INSERT INTO courses (course_id, course_name, course_instructor) VALUES
(1,'Math', 'Dr. Smith'),
(2,'Physics', 'Dr. Brown'),
(3,'Biology', 'Dr. White');

INSERT INTO student_courses (student_id,course_id) VALUES
(1, 1),
(1, 2),
(2, 1),
(2, 3);
```

### Optimisation des performances

Les bases de données relationnelles reposent leurs optimisations sur des fonctionnalités de calcul de statistiques `Analyze`. Elle suivent ensuite des **plans d'executions** selon les données collectées et précalculées sur la bases de données.

Pour les performances on peut être amenés a utiliser les méthodes suivantes : 
- Indexation : On définit une clé homogène pour accèder a des données - équivalent de précalculer une clause where par exemple en amont des requêtes.
- Dénormalisation : On copie les données dans une autre table plus petite pour avoir moins de colonnes a récupérer, cela permet d'accéder plus vite aux informations.
- Vues matérialisées: On précalcule un ensemble de données que l'on peut ensuite cacher côté BDD. On contrôle après le rafraichissement de la donnée précalculée, plus performant mais dépend également des besoins spécifiques pour notre traitement.

<div class="alert alert-info">
    <strong> Exemple de dénormalisation </strong>

Instagram et la dénormalisation : Le **Justin Bieber Problem** : https://medium.com/@AVTUNEY/how-instagram-solved-the-justin-bieber-problem-using-postgresql-denormalization-86b0fdbad94b

</div>

## Implémentations

<img src="https://infodocbib.net/wp-content/uploads/2020/06/Seq4-BddCollegeXYZCoursB.png" />


### SQL - Base de données embarquée, exemple avec SQLite

<img style="max-width:80%;margin-left: auto; margin-right:auto" src="https://www.fullstackpython.com/img/logos/sqlite.jpg" />

[SQLite](https://www.sqlite.org/index.html) 

est un système de base de données embarqué. A la différence des systèmes de base de données classiques (type postgresql, oracle, mysql ...), l'architecture de SQLite ne contient pas de serveur. C'est l'application elle même qui écrit et lit les données à partir d'un fichier `.sqlite` contenant l'ensemble des données.  
SQLite est un moteur de base de données fantastique, utilisé dans des milliards d'appareil (vous venez de regarder vos SMS ? Votre smartphone a utilisé SQLite. Vous venez de regarder le ciel ? Le satellite que vous voyez a très probablement des bases de données sqlite en lui). Si le sujet vous intéresse, cette conférence est formidable : https://www.youtube.com/watch?v=gpxnbly9bz4  

Pour nous, le principal avantage de SQLite va venir de son architecture simplifiée, il n'y a pas besoin de faire tourner de base de données indépendante. 

Tutorial d'utilisation de sqlite :  
[https://www.digitalocean.com/community/tutorials/how-to-use-the-sqlite3-module-in-python-3](https://www.digitalocean.com/community/tutorials/how-to-use-the-sqlite3-module-in-python-3)

<div class="alert alert-info">
  <strong> Pour aller plus loin </strong> 

Voir l'exemple dans le cours : [https://github.com/conception-logicielle-ensai/exemples-cours/blob/main/persistence/sqlite/sqlite.py](https://github.com/conception-logicielle-ensai/exemples-cours/blob/main/cours-5/persistence/sqlite/sqlite.py)
</div>

> Nous vous préconisons SQLite pour le travail en local ou si votre application n'a pas d'enjeux de volumétrie : faible concurrence dans l'accès aux données

Pour travailler sur sqlite (ex avec sqlite3) en python : 

```py
import sqlite3
DB_PATH = os.getenv("DB_PATH", "database.db")  # Le chemin vers la base de données SQLite, un fichier.
conn = sqlite3.connect(DB_PATH)
cursor = conn.cursor()

# puis utilisation du curseur via SQL classique
def read_where_nom(cursor,where_nom:str) -> User:
    cursor.execute('''
    SELECT * FROM user_data WHERE nom = ?
    ''',(where_nom,))
    resultat_raw_sql = cursor.fetchone()
    return User.from_sql_result(resultat_raw_sql)
```
### Bases de données documentaires (NoSQL), exemple avec mongodb

<img style="max-width:80%;margin-left: auto; margin-right:auto" src="https://www.awelty.fr/medias/images/mongodb.png" />

Les bases de données NoSQL permettent de traiter des collections de données non structurées. Elles ont de nombreux avantages.

MongoDB 🍃 est un leader dans le monde des bases de données NoSQL, il permet de stocker et de traiter des calculs sur de nombreux documents.
L'intêret de ce type de base de données est dans l'utilisation de fonctionnalités avancées pour le calcul : 

- Le sharding : MongoDB prend en charge la scalabilité horizontale, c'est-à-dire la possibilité de répartir les données sur plusieurs serveurs (shards).
- Haute disponibilité : par sa scalabilité on peut également bénéficier de transactions rapidement en cas de montée de charge.
- Indexation: Comme les autres BDD, mongoDB permet l'indexation des données, qui, précalculées, sont donc récupérables très efficacement.

On peut interagir avec des moteurs permettant la connexion aux bases de données mongo, en python on privilégiera l'utilisation de `pymongo`.

#### Exemple de code 

> [!TIP]+ Dispo ici **overview**
> https://github.com/conception-logicielle-ensai/exemples-cours/blob/main/etats-persistence-donnees/mongodb/mongo_exemple.py

MongoDB fonctionne avec des `Collections` équivalent des tables. Dans celle ci on peut déposer des données non structurée :

```python
# En abstrayant la connexion a la bdd
COLLECTION = "user_data"
collection = db[COLLECTION]  # Remplacez par le nom de votre collection

# Exemple d'insertion d'un document dans la collection
document = {
    "nom": "John",
    "age": 30,
    "ville": "Paris"
}
collection.insert_one(document)
```

### Bases de données relationnelle orientée traitement : Duckdb
<img style="max-width:80%;margin-left: auto; margin-right:auto" src="https://duckdb.org/img/duckdb_logo.svg" />

[DuckDB](https://duckdb.org) est une base de données relationnelle embarquée, conçue pour le traitement analytique et les requêtes SQL sur de grandes quantités de données, directement depuis votre application ou votre poste de travail.

Contrairement à PostgreSQL ou MySQL, DuckDB ne nécessite pas de serveur : l’ensemble des données est lu et écrit par le moteur directement depuis des fichiers locaux (par exemple .parquet ou .csv) ou en mémoire. On pourrait le voir comme un SQLite optimisé pour le traitement analytique, plutôt que pour les transactions classiques.

DuckDB est particulièrement adapté aux cas où l’on veut exécuter des requêtes SQL complexes sur des fichiers volumineux, sans avoir besoin d’une infrastructure de base de données complète. Grâce à son moteur vectorisé et son optimisation pour le traitement en colonnes, DuckDB est rapide pour :

- les agrégations sur de grands ensembles de données

- le filtrage et le tri de colonnes spécifiques

- la lecture directe de fichiers Parquet ou CSV sans import préalable

**Ici l'enjeu de ce moteur est de vous permettre de réaliser de manière efficiente des traitements statistiques sur les colonnes plutôt que sur les lignes**

Usage :
```py
import duckdb
query = f"""
        FROM read_csv('fichier.csv')
        SELECT *
    """
res = duckdb.sql(query)
```

> [!TIP]+ Exemples plus complets disponibles ici **overview**
> https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/etats-persistence-donnees/duckdb

## Object Relationnal Mapping 

L'Object-Relational Mapping (ORM) est une technique qui permet de mapper des objets du langage de programmation orienté objet a des tables en base de données.

Il s'agit donc d'utiliser les fonctionnalités de POO de python pour sérialiser/desérialiser automatiquement les données récupéréesen base de données.

L'ORM propose également une interface de haut niveau qui permet de s'abstraire du type de base de données utilisées, bien qu'on utilise cela en général dans des bases de données a la structure stable, des bases de données relationnelles.

Il propose finalement une interface fonctionnelle python pour ne plus avoir a développer les scripts d'accès a la base de données et permet de travailler avec des bases de données de language différentes a partir du même code python.

**Par exemple:**
- En local on voudrait travailler sur une base de données portable : SQLite, mysql
- Dans un environnement hébergé on voudrait travailler sur une base de données : postgresql 

### SQLAlchemy

<img style="max-width:80%;margin-left: auto; margin-right:auto" src="https://miro.medium.com/v2/resize:fit:1400/0*msfsws06ImMSJYop.jpg" />
SQLAlchemy est une implémentation proposant une interface d'Object Relationnal Mapping en Python. Il implémente de nombreuses fonctionnalités dont :
- La gestion des relations
- La gestion des injections SQL
- La gestion des connexions (pour éviter des fuites de connexion)

Il fait cela au travers de ce qu'on appelle un `engine` qui lui même s'appuie sur des connecteurs.

- En python il s'agira des connecteurs permettant de se connecter aux bases habituellement : `psycopg2` `mysql` `sqlite3`
- Dans le monde `java` on utilise l'interface `jdbc` et des implémentations en fonction du type de bases de données : `org.postgresql`

La syntaxe se présente comme suit : 

On utilise le module `declarative_base` pour rendre nos classes `mappées`
```python
Base = declarative_base()

# Définition du modèle pour la table "Utilisateur"
class Utilisateur(Base):
    __tablename__ = 'utilisateurs'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String)
    age = Column(Integer)
```

On se connecte a la base de données, l'engine détermine le type par rapport au protocole de connection, ici `sqlite:`
```python
# Création de l'engine de la base de données (ici SQLite)
engine = create_engine('sqlite:///:memory:')
```

Puis on peut interagir avec la base de données au travers de sessions

```python
# Création d'une session pour interagir avec la base de données
Session = sessionmaker(bind=engine)
session = Session()
# Création d'un nouvel utilisateur
nouvel_utilisateur = Utilisateur(nom="John", age=30)

# Ajout de l'utilisateur à la session
session.add(nouvel_utilisateur)
```

<div class="alert alert-info">
    <strong> Pour aller plus loin </strong>

Voir l'exemple : [sql_alchemy.py](https://github.com/conception-logicielle-ensai/exemples-cours/blob/main/cours-5/persistence/orm/sql_alchemy.py)

</div>

<div class="alert alert-info">
    <strong> Pour aller plus loin </strong>

SqlAlchemy dans une archi projet : [exemple de thread stackoverflow](https://stackoverflow.com/questions/7478403/sqlalchemy-classes-across-files)

</div>

### Autres implémentations

#### Fastapi : SQLModel

[SQLModel](https://sqlmodel.tiangolo.com/) est un ORM moderne basé sur SQLAlchemy et Pydantic.

Il combine :

- la puissance de SQLAlchemy pour les requêtes SQL

- la validation de données et types via Pydantic

- la simplicité pour créer des modèles compatibles avec FastAPI

Exemple rapide :

```py
from sqlmodel import SQLModel, Field, create_engine, Session

class Utilisateur(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nom: str
    age: int

engine = create_engine("sqlite:///:memory:")
SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    user = Utilisateur(nom="Alice", age=25)
    session.add(user)
    session.commit()
```

> [!TIP]+ Exemple plus complet disponibles ici **overview**
> https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/etats-persistence-donnees/orm/sqlmodel-ex

#### Django : Models et Session

Django possède son *ORM* intégré pour définir des modèles et interagir avec la base de données.

Les classes héritent de `models.Model` et sont automatiquement mappées aux tables SQL.

> Documentation associée : https://docs.djangoproject.com/en/6.0/topics/db/models/

Exemple :

```py
from django.db import models

class Utilisateur(models.Model):
    nom = models.CharField(max_length=100)
    age = models.IntegerField()


Les opérations sur les données se font via le ORM Django, par exemple :

# Création et sauvegarde d'un utilisateur
user = Utilisateur(nom="Bob", age=30)
user.save()

# Requête
Utilisateurs.objects.filter(age__gte=18)
```


Django gère également le migrateur de base de données, les relations entre tables et la sécurité des requêtes SQL, rendant l’ORM très complet pour des projets web.


> [!TIP]+ Exemple plus complet disponibles ici **overview**
> https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/etats-persistence-donnees/orm/djangoex


## Gestionnaire de version de base de données

Dans le développement logiciel, le code évolue constamment : on ajoute des fonctionnalités, on modifie des structures de données, et on corrige des bugs. La base de données doit suivre ces changements pour rester cohérente avec le code.

Sans outil dédié, gérer ces évolutions peut rapidement devenir complexe et risqué :

- Des tables ou colonnes peuvent disparaître ou être modifiées par erreur.

- Les développeurs peuvent travailler avec des schémas différents, provoquant des incohérences entre environnements (local, test, production).

- Revenir à un état précédent de la base de données en cas de problème est fastidieux et dangereux.

C’est là qu’intervient un gestionnaire de version de base de données.

Un tel outil permet de :

- Tracer l’historique des modifications du schéma (tables, colonnes, index…).

- Appliquer et annuler les changements de manière sécurisée.

- Synchroniser facilement la base de données avec le code, même dans des équipes de plusieurs développeurs.

- Réduire les risques d’erreurs lors de migrations entre différents environnements.

En d’autres termes, un gestionnaire de version de base de données fonctionne un peu comme Git, mais pour la structure de votre base de données. Chaque migration est une "révision" qui peut être appliquée ou annulée.

> Ce n'est pas attendu, mais vous pouvez le mettre en place dans votre projet cela sera valorisé

Exemple de frameworks : `alembic` pour SQLModel et SQLAlchemy, et c'est pas défaut dans `django`.

## Pistes pour vos projets

- Travail en local avec des bases de données locales : `mysql` `sqlite` (partage du fichier db éventuellement ou d'un db au démarrage)

- Installation de bases de données `postgresql` `mongodb` sur les instances du `SSPCLOUD`. Attention, elles seront uniquement accessible du cluster et donc d'un vscode dans le cluster.

- Installation des bases de données en local :

**Mongodb** : Suivre la documentation ici [https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

**Postgresql**: Suivre la documentation ici [https://ubuntu.com/server/docs/install-and-configure-postgresql](https://ubuntu.com/server/docs/install-and-configure-postgresql)

- Utilisation de docker (dans 2 cours on voit ça 🤓🤓)

**Mongodb**: `docker run -d --name mongodb -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=motdepasseadmin -p 27017:27017 mongo:8.2.3-noble`

**Postgresql**: `docker run postgres -d -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=motdepasseadmin -e POSTGRES_DB=postgres:postgres:18.1-trixie -p 5432:5432` 

### Attendus projets
- Il sera attendu de vous de nous fournir un script d'initialisation de base de données ou un fichier pour démarrer dans votre projet si vous implémentez de la persistence.
- Le choix de la BDD vous incombe
- La modélisation en base de données ne nécessite pas de diagramme pour le rendu, mais sera analysée pour les items qualité du code / fonctionnel.