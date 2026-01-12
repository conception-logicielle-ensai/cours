---
title: "Architecture applicative"
weight: 28
draft: false
description: "Appréhender l'organisation structurelle d’une application logicielle"
summary: "Appréhender les enjeux de l'architecture logicielle pour la conception logicielle"
slug: "architecture-applicative"
tags: ["cours","architecture applicative"]
series: ["Cours"]
series_order: 2
---
> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/architecture-applicative




Cette partie est largement inspirée des concepts et exemples présentés dans le livre :

> **Clean Architecture**, A Craftsman's Guide to Software Structure and Design. Robert C. Martin 2017.


## Architecture applicative : une définition

L'architecture applicative désigne l'organisation structurelle d'une application logicielle, englobant ses composants, leurs interactions et les principes sous-jacents à leur conception. Elle établit une feuille de route qui guide le développement, le déploiement, et l'évolution de l'application en fonction des besoins techniques, fonctionnels et stratégiques.

> En d'autres termes c'est une organisation proposée des sources et dépendances de nos programmes qui permettent ensuite une bonne évolution de celui ci.

Mais pour quel but?

> **The goal of software architecture is to minimize the human resources required to build and maintain the required system.**

## La conception logicielle au coeur

![](/images/archi/software-archi.png)


La conception logicielle est l'ensemble des activités qui permettent la mise en place d'un programme informatique à partir d'un besoin.

### Séparer le besoin de l'implémentation

**Quel est le but derrière le développement d'un programme informatique ?**

On souhaite généralement répondre à un besoin exprimé : *exemple : Mettre en place un site pour permettre à un commerçant de vendre ses productions*

Pour répondre à ce besoin nous allons proposer des solutions techniques qui permettront d'aboutir à un site fonctionnel.

Il va donc falloir **comprendre le besoin**, puis **proposer une solution d'implémentation** à décliner en **unités de travail à effectuer**.

Ensuite il faut limiter au maximum les choix que l'on fait, l'idée des "**software**" est bien d'être évolutif et chaque choix effectué de notre côté est **enfermant** puisqu'on devient couplé à une technologie et en changer nécessite du travail humain (choix du type de base de données, technologie ...), le coût associé au changement est appelé **Dette Technique**.

Exemple : 
1. Jean souhaite construire un site web pour vendre des sneakers

2. On comprend donc qu'il faut implémenter un site web de type commerce en ligne.

3. On peut proposer différentes solutions pour mettre en place le site :  
{{< alert >}}
Il faut en première instance s'assurer que toutes les technologies que l'on distingue permettent de réaliser les fonctionnalités exprimées par le client (ici Jean) 
{{< /alert >}}

    - Développer un site en Django / FastApi ou autre technologie python, et l'héberger
    - Suivre des tutoriels en ligne pour développer des sites statiques (HTML,CSS,JS) ou je souhaite héberger via d'autres framework connus (PHP)
    - Utiliser un outil dédié a la conception de site (shopify)


> Une fois cette solution décidée, on réalise un découpage des besoins de l'utilisateur et on vérifie et implémente les fonctionnalités une à une 



4. Je décide de développer mon site en utilisant FastApi pour faire du WebTemplating, j'ai déjà de l'expérience dans l'usage vu que j'ai eu un super cours en 2ème année.

> [!TIP]+ Remarque
> Ici on fait un premier choix de conception.


Je distingue (par exemple) donc les fonctionnalités suivantes : 
- **user: client** Mettre en place une authentification avec données client.
- **user: client** Mettre en place un panier cross pages
- **user: client** Permettre d'accéder aux articles
- **user: client** Permettre de payer pour un article

- **user: Jean** Permettre la mise en ligne d'un article
- **user: Jean** Accéder à la liste des articles a envoyer et aux gens

> [!TIP]+ Remarque
> Ici on a identifié des tâches qu'on peut vouloir assigner dans un travail en projet, cependant on n'a pas tranché sur des questions techniques en amont

- **Quid de la solution de paiement? Stripe?**
- **Quid du stockage en base de données, quel type de base de données?**

Il faut identifier ces points, pour y trancher soit par le **chef de projet**, soit par l'équipe et éventuellement commencer à travailler tous ensemble sur ces sujets pour s'accorder avant de se répartir les tâches.


Exemples de choix structurants :
- type de base de données (SQL / NoSQL),
- framework web,
- architecture distribuée ou non,
- patterns imposés dès le départ.
- découpage des couches (éviter les imports circulaires) et structure du code
### Gestion de projet : un projet comme un marathon

<img src="https://web-static.wrike.com/cdn-cgi/image/format=auto,quality=80,width=776,dpr=2/tp/storage/uploads/92025823-45d4-4b1c-bcef-6dffd4727344/scrum-cycle-resized.png"/>

Comme un marathon un projet peut s'étaler sur des périodes longues mais nécessite une bonne gestion des ressources pour arriver à bout sans y perdre des plumes.

Il est donc essentiel d'arriver à se **répartir les tâches dans les équipes** et se donner des **échéances** logiques par rapport a la **durée du projet** : 

Exemple : 

> Le rendu du projet est en mars pour un démarrage mi janvier pour la plupart des groupes. Une application qui arrive a fonctionner fin janvier et évolue pour la relecture mi février, cela semble logique.

Vous pouvez également mener un cadre de travail agile sur le projet suivant une méthode type **scrum**.

- Echéances toutes les 2 semaines
- Estimation du coût de développement des fonctionnalités
- Assignation des fonctionnalités dans l'équipe par rapport à la motivation et la disponibilité de chacun en début des cycles de travail en 2 semaines (avec possibilité de renégociation en cours de période / sprint)

### Simplicité dans le design

![](/images/archi/simplicite.jpeg)

Lorsqu’on conçoit un logiciel, il est très tentant de vouloir :
- **anticiper tous les cas possibles**,
- **choisir des solutions « robustes »**,
- **préparer le futur dès la première version**.

Cette approche part souvent d’une bonne intention, mais elle mène fréquemment à une **sur-conception** (*over-engineering*).

La simplicité dans le design consiste à **faire moins**, mais **faire juste**.

> Typiquement, n'évoquez pas l'utilisation de dictionnaires pour prévoir tous les cas possibles, vous allez simplement créer une application infernale à faire évoluer et à comprendre.

<br />

#### La simplicité n’est pas l’absence de réflexion

Concevoir simplement ne signifie pas :
- coder vite sans réfléchir,
- ignorer les bonnes pratiques,
- produire du code « jetable ».

Au contraire, un design simple est généralement le résultat de **choix réfléchis**, assumés, et volontairement limités.

> La complexité apparaît naturellement avec le temps.  
> La simplicité, elle, doit être **défendue**.

{{< alert >}}
Il faut donc encore une fois limiter les choix techniques au maximum pour rester flexible et rester sur une base saine et simple de code.
{{< /alert >}}



> [!TIP]+ Proposition
> Veillez à partager des pratiques de développement au début du projet en effectuant une relecture ou du binomage puis un suivi à mi parcours.


<br />

#### Exemple

Dans le cas du site de vente de sneakers :

À ce stade du projet, les besoins identifiés sont :
- afficher des articles,
- gérer un panier,
- permettre un paiement,
- permettre à Jean de gérer ses produits.

Il serait tentant de se poser immédiatement des questions comme :
- faut-il créer plusieurs services pour gérer l'application (backend / frontend)
- faut-il un moteur de règles par rapport au panier?
- faut-il prévoir plusieurs mode de paiement ?
- faut-il une base de données redondée si le datacenter contenant les données disparait? [#ovh](https://fr.wikipedia.org/wiki/Incendie_du_centre_de_donn%C3%A9es_d%27OVHcloud_%C3%A0_Strasbourg)

Or, **rien dans le besoin exprimé ne justifie encore ces choix**.

Un design simple consisterait par exemple à :
- utiliser une architecture monolithique bien structurée,
- utiliser une solution clé en main (shopify)
- utiliser un moteur de base de données agnostique de la base de données (ORM)
- intégrer une solution de paiement standard,
- découpler le métier du reste pour faciliter une évolution future.

## Organisation du code et des dépendances

![](/images/archi/links.png)
Une application moderne et efficace utilise des briques externes, au travers des dépendances internes comme les packages (voir le [cours associé](/cours/packages-portabilite/)) mais également l'utilisation d'autres outils et applications.

Par exemple pour une base de code python:
- Besoin d'une base de données pour gérer la persistence des données
- Besoin d'utiliser une API externe pour authentifier des utilisateurs en déléguant l'authentification à Google (OIDC)
- Besoin d'utiliser une API externe pour utiliser des données métier (exemple : besoin localisation, fonds de carte, base de données tierces)
- Besoin d'accéder a des catalogues de données pour le calcul au **runtime**

### Architecture Logicielle 

L'architecture logicielle propose une description symbolique et schématique des différents éléments interagissant dans un ou plus systèmes informatiques.

Dans le cadre des projets, on pourra mobiliser les diagrammes UML pour expliquer les processus complexes d'une application (diagramme d'activité), mais il sera ici attendu pour tous les projets un diagramme d'architecture logicielle globale.


#### **Exemple sur l'application de vente en ligne**:

{{< mermaid >}}
graph TB
    User[👤 Utilisateur]
    
    subgraph "Frontend"
        WebApp[Application Web]
    end
    
    subgraph "Backend - FastAPI"
        API[API FastAPI]
        Auth[Module Auth]
        Payment[Module Paiement]
        Products[Module Produits]
        Orders[Module Commandes]
    end
    
    subgraph "Services Externes"
        Google[Google OAuth]
        Stripe[Stripe API]
    end
    
    subgraph "Base de données"
        DB[(PostgreSQL)]
    end
    
    User --> WebApp
    WebApp --> API
    
    API --> Auth
    API --> Payment
    API --> Products
    API --> Orders
    
    Auth --> Google
    Auth --> DB
    
    Payment --> Stripe
    Payment --> DB
    
    Products --> DB
    Orders --> DB
    
    style User fill:#e1f5ff
    style Google fill:#fff3e0
    style Stripe fill:#e8f5e9
    style DB fill:#f3e5f5

{{< /mermaid >}}

Comme on vous l'a appris, tout diagramme doit être commenté.

En voici le commentaire : 

Ce diagramme présente l'architecture logicielle de l'application de vente, en voici les modules principaux: 
1. **Authentification** : L'utilisateur se connecte via Google OAuth
2. **Catalogue** : Consultation des produits disponible dans la base de données PostgreSQL
3. **Commande** : Création de commande stockée en base
4. **Paiement** : Transaction sécurisée via Stripe
5. **Confirmation** : Mise à jour du statut en base de données pour l'envoi
6. **Validation** : Mise a jour après réception par le client par Jean

> [!TIP]+ Remarque
> Encore une fois, on pourrait déléguer programmatiquement la partie validation en utilisant des API de la poste par exemple 

### Architecture en couches

#### Préambule Python : Module et Package
Un module en Python est un fichier contenant du code Python. Il peut inclure des fonctions, des classes, des variables, et même des instructions exécutables. Un module permet de regrouper des fonctionnalités spécifiques dans un fichier distinct afin de rendre le code plus réutilisable, organisé et lisible.

En Python, les modules sont simplement des fichiers avec l'extension .py. Vous pouvez créer votre propre module ou utiliser des modules standard fournis avec Python. Pour utiliser un module, vous devez l'importer dans votre programme avec la commande import.


Il peut être regroupé en **package** et distribué.

```
app/
├── main.py
├── models/
│   ├── __init__.py
│   ├── user.py
```

Ici: 
- app/main.py est un **module**
- models est un **package**
- models/user est un **module**

> [!TIP]+ A retenir
> Cela permet d'isoler des portions de code et de définir quel module utilise quel autre


#### Modèles

A partir de ces notions de modules est arrivée une idée de segmentation des modules en responsabilité, qu'on appelle des **couches**.

L'**architecture en couches** est un modèle de conception logicielle qui organise une application en niveaux distincts, chacun ayant des responsabilités spécifiques et communiquant uniquement avec les couches adjacentes. Ce principe de séparation améliore la maintenabilité, la testabilité et permet de modifier une couche sans impacter les autres.

#### MVC
Le modèle MVC (Model-View-Controller) est l'exemple le plus classique : le Model gère les données et la logique métier, la View s'occupe de l'affichage, et le Controller fait le lien entre les deux en traitant les requêtes utilisateur. 


Dans les applications web modernes, on retrouve souvent une séparation en couches API (endpoints), Services (logique métier), Repository (accès aux données) et Models (entités).

Cela se traduit par: 
- La création d'un package model / business_object dans lequel on définit les structures de données utilisées partout dans l'application 
- La création d'un package repository / dao qui gère l'accès a la base de données pour la récupération des entités model, exemple `panier_dao` qui peut récupérer le panier du client `find_panier_by_client_id`
- La création d'un package controller / service qui gère **l'orchestration** des appels aux différents autres service dans l'objectif de réaliser les fonctionnalités associées : Exemple `article_service` qui traiterait des fonction comme `ajouter_nouveau_article` par exemple, mais pas `calculer_montant_panier` qui s'appuierait plutôt sur un `panier_service` qui irait chercher des articles via le `article_repository`.
- un package view / web qui permettrait d'afficher les pages et traiter les requêtes utilisateur en les distribuant au package services.

Les dépendances (`imports`) seraient donc définies comme suit
{{< mermaid >}}
graph TB
    view/web ----> service
    view/web ---> business_object
    service ----> DAO
    service ----> business_object
    service --> service
{{</mermaid>}}

Dans les faits nous attendons que vous mettiez en place une telle architecture dans les projets.
<br/>

**Exemple sur l'application de ecommerce de Jean**

```
ecommerce_mvc/
├── __init__.py
├── main.py
├── config.py
├── models/
│   ├── __init__.py
│   ├── user.py
│   ├── article.py
│   ├── cart.py
│   └── order.py
├── views/
│   ├── __init__.py
│   ├── auth_views.py
│   ├── article_views.py
│   ├── cart_views.py
│   └── order_views.py
├── controllers/
│   ├── __init__.py
│   ├── auth_controller.py
│   ├── article_controller.py
│   ├── cart_controller.py
│   └── order_controller.py
├── database/
│   ├── __init__.py
│   └── connection.py
├── static/*
├── templates/*
```

#### Autres architectures

D'autres architectures existent, l'architecture hexagonale qui isole le cœur métier des interfaces externes, ou encore l'architecture en oignon qui place le domaine au centre entouré de couches de services et d'infrastructure.

> [!TIP]+ Pour aller plus loin
> Un article pour s'intéresser a l'architecture hexagonale en python: https://medium.com/@miks.szymon/hexagonal-architecture-in-python-e16a8646f000


## Programmation orientée objet (POO) : Principes de l'objet

La programmation orientée objet (POO) repose sur plusieurs principes fondamentaux, parmi lesquels : **polymorphisme**, **encapsulation** et **héritage**. Ces concepts permettent de structurer le code de manière modulaire, réutilisable et extensible.


La programmation orientée objet (POO) repose sur plusieurs principes fondamentaux, parmi lesquels : **polymorphisme**, **encapsulation** et **héritage**. Ces concepts permettent de structurer le code de manière modulaire, réutilisable et extensible.

### Polymorphisme  
Le polymorphisme permet à une même méthode ou fonction d'avoir des comportements différents selon le contexte ou le type de données.Cela permet de définir des interfaces ou contrats génériques sans se soucier des détails d'implémentation. 

Concrètement, on peut écrire du code qui manipule des objets de manière abstraite (via une interface commune), puis être libre d'ajouter de nouvelles implémentations concrètes sans modifier le code existant.


> [!TIP]+ Important
> Ainsi on a juste a définir au lieu d'une structure fixe, une interface qui répond a un contrat d'interface (fonctions internes implémentées par chaque implémentation). 

#### Exemple : Système de paiement

Imaginons un système de paiement qui accepte plusieurs méthodes de paiement (carte bancaire, PayPal, Bitcoin). Chaque méthode a sa propre logique, mais toutes doivent implémenter une méthode `pay()`.

```python
class PaymentMethod:
    def pay(self, amount):
        pass

class CreditCard(PaymentMethod):
    def pay(self, amount):
        print(f"Payment of {amount} made using Credit Card.")

class PayPal(PaymentMethod):
    def pay(self, amount):
        print(f"Payment of {amount} made using PayPal.")

class Bitcoin(PaymentMethod):
    def pay(self, amount):
        print(f"Payment of {amount} made using Bitcoin.")

# Polymorphisme : utilisation de différentes méthodes de paiement
def process_payment(payment_method, amount):
    payment_method.pay(amount)

# Utilisation
credit_card = CreditCard()
paypal = PayPal()
bitcoin = Bitcoin()

process_payment(credit_card, 100)  # Payment of 100 made using Credit Card.
process_payment(paypal, 200)       # Payment of 200 made using PayPal.
process_payment(bitcoin, 300)      # Payment of 300 made using Bitcoin.
```

### Encapsulation

L'encapsulation consiste à restreindre l'accès direct aux données internes d'une classe et à les protéger contre les modifications non contrôlées. Cela est réalisé en définissant des attributs privés ou protégés. Ils sont isolés de l'extérieur et on interagit avec eux indirectement via des getter, des setter ou via l'appel de fonction internes à la classe.

> [!TIP]+ Important
> C'est via ce mécanisme que l'on contrôle l'utilisation et l'isolation ce qui permet de mieux faire évoluer les structures internes.


#### Exemple encapsulation : cas bancaire

```python
class BankAccount:
    def __init__(self, owner, balance, credit_limit):
        self.owner = owner
        self.__balance = balance  # Attribut privé
        self.__credit_limit = credit_limit  # Attribut privé

    # Getter pour accéder au solde
    def get_balance(self):
        return self.__balance

    # Méthode pour savoir si l'utilisateur peut payer une certaine somme
    def can_pay(self, amount):
        return amount <= self.__balance + self.__credit_limit

    # Dépôt d'argent
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
        else:
            raise ValueError("Le montant doit être positif.")

    # Retrait d'argent (avec vérification de la possibilité de paiement)
    def withdraw(self, amount):
        if amount <= self.__balance:
            self.__balance -= amount
        elif amount <= self.__balance + self.__credit_limit:
            # Utilisation du crédit autorisé si le solde est insuffisant
            self.__credit_limit -= (amount - self.__balance)
            self.__balance = 0
        else:
            raise ValueError("Solde insuffisant et limite de crédit atteinte.")

# Utilisation
account = BankAccount("Alice", 1000, 500)  # Alice a un solde de 1000 et une limite de crédit de 500

# Vérifier si Alice peut payer
print(account.can_pay(1200))  # True, car 1000 (solde) + 500 (crédit) = 1500, suffisant pour 1200
print(account.can_pay(1700))  # False, car 1000 + 500 = 1500, insuffisant pour 1700

# Retrait
account.withdraw(1200)  # Utilisation du crédit de 200, il reste 100 dans le solde
print(account.can_pay(100))  # True, car 0 (solde) + 500 (crédit) suffisant pour 100
```

> On remarque qu'on a pas besoin de connaître le montant du compte en banque d'alice pour réaliser l'opération, mais que cela est fait de manière intrinsèque a l'opération de consommation et de "can_pay".

> [!TIP]+ A retenir
> Si vous devez définir des conditions liées a des attributs de structure, intégrez les, cela améliorera la lisibilité et centralisera le fonctionnement. 

> *Imaginez si au lieu de can_pay() on avait un if partout dans le code et qu'on souhaitait changer la règle.*

### Héritage

L'héritage permet de créer de nouvelles classes (sous-classes) en réutilisant le code d'une classe existante (classe parente). Cela favorise la réutilisabilité et l'organisation du code. En Python, une sous-classe hérite de toutes les méthodes et attributs de la classe parente, mais peut aussi redéfinir des comportements spécifiques.

#### Exemple avec une classe d'un site de reservation proposant plusieurs services
```python
# Classe parente : Reservation
class Reservation:
    def __init__(self, name, date, price):
        self.name = name  # Nom de la personne qui réserve
        self.date = date  # Date de la réservation
        self.price = price  # Prix de la réservation

    def display_info(self):
        print(f"Réservation pour {self.name} à la date du {self.date}. Prix: {self.price}€")

    def confirm(self):
        print(f"La réservation pour {self.name} est confirmée.")

# Classe enfant : HotelReservation (hérite de Reservation)
class HotelReservation(Reservation):
    def __init__(self, name, date, price, num_nights):
        super().__init__(name, date, price)  # Appel au constructeur de la classe parente
        self.num_nights = num_nights  # Nombre de nuits réservées

    def display_info(self):
        super().display_info()  # Appel de la méthode de la classe parente
        print(f"Nombre de nuits: {self.num_nights}")

    def confirm(self):
        super().confirm()  # Appel de la méthode de la classe parente
        print(f"Réservation de l'hôtel pour {self.num_nights} nuit(s) confirmée.")

# Classe enfant : FlightReservation (hérite de Reservation)
class FlightReservation(Reservation):
    def __init__(self, name, date, price, flight_number):
        super().__init__(name, date, price)  # Appel au constructeur de la classe parente
        self.flight_number = flight_number  # Numéro de vol

    def display_info(self):
        super().display_info()  # Appel de la méthode de la classe parente
        print(f"Numéro de vol: {self.flight_number}")

    def confirm(self):
        super().confirm()  # Appel de la méthode de la classe parente
        print(f"Réservation du vol {self.flight_number} confirmée.")

# Utilisation des classes

# Création d'une réservation d'hôtel
hotel_reservation = HotelReservation("Alice", "2023-12-25", 200, 3)
hotel_reservation.display_info()
hotel_reservation.confirm()

# Création d'une réservation de vol
flight_reservation = FlightReservation("Bob", "2023-12-25", 150, "AF1234")
flight_reservation.display_info()
flight_reservation.confirm()
```

##  Programmation orientée objet (POO) : Principes SOLID

![](https://cdn.prod.website-files.com/65cb885e207a8d416674eca1/668fc5664209f0b439cce300_Sans%20titre-4.png)

#### 1. **SRP : Principe de Responsabilité Unique (Single Responsibility Principle)**  
Un module logiciel (classe, fonction, package...) ne doit avoir qu'**une seule raison de changer**.  

Autrement dit, une classe doit avoir **une responsabilité claire et bien définie**.

> [!TIP]+Typiquement
> Une classe qui calcule, affiche et persiste des données viole le SRP.

```python
# Mauvaise pratique : une seule classe gère plusieurs responsabilités.
class Order:
    def calculate_total(self):
        pass  # Calcul du total
    def print_order(self):
        pass  # Imprime la commande
    def save_to_db(self):
        pass  # Sauvegarde dans la base de données
```
```python
# Bonne pratique : chaque classe gère une seule responsabilité.
class Order:
    def calculate_total(self):
        pass

class OrderPrinter:
    def print_order(self, order):
        pass

class OrderRepository:
    def save_to_db(self, order):
        pass
```

> Cela implique donc qu'il faut séparer fonctionnellement les implémentations entre des **couches** bien distinctes => Vues, Business_object, ...

#### 2. **OCP : Principe Ouvert-Fermé (Open-Closed Principle)**  
Les modules logiciels doivent être **ouverts** à l’extension, mais **fermés** à la modification.  

> [!TIP]+Typiquement
> L’ajout d’un nouveau fonctionnement dans une fonctionnalité déjà développée ne doit pas nécessiter de modifier une classe existante.
```python
# Mauvaise pratique : modification du code existant pour ajouter un comportement.
class Discount:
    def apply_discount(self, price, discount_type):
        if discount_type == "percentage":
            return price * 0.9
        elif discount_type == "fixed":
            return price - 10
```
```python
# Bonne pratique : extension via des classes dérivées.
from abc import ABC, abstractmethod

class Discount(ABC):
    @abstractmethod
    def apply_discount(self, price):
        pass

class PercentageDiscount(Discount):
    def apply_discount(self, price):
        return price * 0.9

class FixedDiscount(Discount):
    def apply_discount(self, price):
        return price - 10
```

> Cela vous permet de définir autant de versions différentes par extensions plutôt que de créer des chaines de **if**.

#### 3. **LSP : Principe de Substitution de Liskov (Liskov Substitution Principle)**  
Tout objet d’un type dérivé doit pouvoir être utilisé à la place de son type parent, sans altérer le comportement attendu.

> [!TIP]+Dit autrement
> Les enfants doivent respecter le contrat d'interface des parents.

```python
# Mauvaise pratique : la classe dérivée casse le contrat de la classe parent.
class Bird:
    def fly(self):
        pass

class Penguin(Bird):
    def fly(self):
        raise Exception("Les pingouins ne volent pas!")
```
```python
# Bonne pratique : refactorisation pour respecter le contrat.
from abc import ABC, abstractmethod

class Bird(ABC):
    @abstractmethod
    def move(self):
        pass

class FlyingBird(Bird):
    def move(self):
        print("Je vole!")

class NonFlyingBird(Bird):
    def move(self):
        print("Je marche!")
```
#### 4. **ISP : Principe de Ségrégation des Interfaces (Interface Segregation Principle)**  
Un client ne doit pas être forcé de dépendre d'interfaces qu'il n'utilise pas.  
- Ce principe prône la conception d’interfaces spécifiques à un usage précis, évitant les dépendances inutiles.  


```python
# Mauvaise pratique : une interface trop large.
class Animal:
    def eat(self):
        pass
    def fly(self):
        pass
    def swim(self):
        pass

class Dog(Animal):
    def eat(self):
        pass
    def fly(self):  # Non pertinent pour un chien.
        raise NotImplementedError()
    def swim(self):
        pass
```
```python
# Bonne pratique : des interfaces spécifiques.
from abc import ABC, abstractmethod

class Eater(ABC):
    @abstractmethod
    def eat(self):
        pass

class Swimmer(ABC):
    @abstractmethod
    def swim(self):
        pass

class Dog(Eater, Swimmer):
    def eat(self):
        print("Je mange!")
    def swim(self):
        print("Je nage!")
```

#### 5. **DIP : Principe d'Inversion des Dépendances (Dependency Inversion Principle)**  
Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau.  

> [!TIP]+ A retenir
> On injecte des abstractions (interfaces) plutôt que des implémentations concrètes, ce qui permet de changer ces dernières sans modifier le code client.

> Cela rentre dans les choix de design qui permettent de garder un code ouvert.


```python
# Mauvaise pratique : dépendance directe sur une implémentation.
class Database:
    def connect(self):
        print("Connexion à la base de données...")

class UserRepository:
    def __init__(self):
        self.db = Database()
    def get_user(self, user_id):
        self.db.connect()
        print(f"Récupération de l'utilisateur {user_id}")
```
```python
# Bonne pratique : dépendance sur une abstraction.
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        print("Connexion à MySQL...")

class UserRepository:
    def __init__(self, db: Database):
        self.db = db
    def get_user(self, user_id):
        self.db.connect()
        print(f"Récupération de l'utilisateur {user_id}")

# Utilisation
db = MySQLDatabase()
repo = UserRepository(db)
repo.get_user(1)
```

> Il faut ici noter qu'on injecte des interfaces plutôt que des implémentations et donc cela permet de changer l'implémentation sans avoir a toucher a la classe fille

#### Importance des Principes SOLID  
Ces principes permettent :  
- Une meilleure **maintenabilité** : le code est plus facile à comprendre, tester et modifier.  
- Une meilleure **évolutivité** : les modifications ou extensions du logiciel n’affectent pas ses parties stables.  

> Il faudra donc isoler les couches applicatives indépendantes, en ne faisant pas de calculs dans les parties d'affichage, en ne partageant pas d'accès aux bases de données avec les calculs mais bien en isolant les différents modules applicatifs dans le code.


## Programmation Fonctionnelle

![](/images/archi/functionnal_basics.webp)


Cette partie présente des concepts détaillés dans le livre.
> **Functional Programming in Scala**, Paul Chiusano and Rúnar Bjarnason.

La programmation fonctionnelle est un paradigme qui vise à construire des programmes prévisibles, composables et faciles à raisonner, en s’appuyant principalement sur :

- les fonctions pures,

- l’immutabilité,

- la composition,

- la gestion explicite des effets de bord.


### Qu'est ce qu'une fonction ?
Les fonctions permettent de regrouper des instructions pour accomplir une tâche spécifique. Elles favorisent la réutilisabilité, la compréhension du code et son organisation.

Exemple : 
```python
def battre_oeufs():
    """Simule le fait de battre les œufs."""
    print("Battre les œufs jusqu'à ce qu'ils soient bien mousseux.")

def faire_fondre_beurre():
    """Simule le fait de faire fondre le beurre."""
    print("Faire fondre le beurre dans une casserole.")

def faire_fondre_chocolat():
    """Simule le fait de faire fondre le chocolat."""
    print("Faire fondre le chocolat au bain-marie.")

def preparer_mousse_au_chocolat():
    """Prépare la mousse au chocolat en appelant les autres fonctions dans l'ordre."""
    print("Préparation de la mousse au chocolat :")
    battre_oeufs()
    faire_fondre_beurre()
    faire_fondre_chocolat()
    print("Mélanger tous les ingrédients pour former une mousse délicieuse!")

# Appel de la fonction principale pour préparer la mousse au chocolat
preparer_mousse_au_chocolat()
```

> [!TIP]+ A retenir
> Le bon nommage des fonction et des paramètres permettent une abstraction qui rendent le code très lisible, veillez à cela.

### Fonctions pures
En programmation fonctionnelle, une fonction pure ne modifie jamais les données qu’elle reçoit.

Une modification inclut par exemple les éléments suivants :
- Modification d'une variable en entrée
- Changement d'un état externe
- Jeter une exception ou arrêter le traitement avec une erreur
- Print dans la console ou attendre une action externe
- Lire ou écrire dans un fichier

Même lorsqu’elle manipule des objets complexes (dictionnaires, listes, objets métier), elle doit produire une nouvelle valeur, pas modifier une valeur existante.

> [!TIP]+ Ouverture
> Imaginez un mode de programmation ou on ne peut pas faire de l'assignation de variable, de boucles, de la gestion d'exception. C'est le cas pour certains languages.

#### Exemple: Manipulation d'un dictionnaire

Considérons un objet order représentant une commande.
```python
order = {
    "id": 42,
    "items": [
        {"name": "vans", "price": 80},
        {"name": "converse-xxx", "price": 100},
        {"name": "noname", "price": 120}
    ],
    "total": 0
}
```

Exemple de fonction, est elle pure ?

```python
def calculate_total(order):
    total = sum(item["price"] for item in order["items"])
    order["total"] = total
    return order
```

Problèmes :

- l’objet order est modifié sur place

- la fonction a un effet de bord: elle modifie order

- l’appelant ne peut pas savoir si l’objet a été altéré par la fonction

Voici une version avec fonction pure:
```python
def calculate_total(order):
    total = sum(item["price"] for item in order["items"])
    return {
        **order,
        "total": total
    }
```

**Comment on fait du coup?**

On réaffecte directement, le changement est **explicite** : 
```python
order = calculate_total(order)
```
### Immutabilité

En programmation fonctionnelle, une donnée est dite immuable lorsqu’elle ne peut pas être modifiée après sa création.

En Python, certains types sont immuables par conception : une fois créés, ils ne peuvent pas être modifiés.

Par exemple les génériques : 

```
[int,float,bool,str]
```
Mais pas les types : 

```
[dict, list, set]
```
#### Exemples

Cas entier :
```python
a = 10
b = a
a += 1

print(a)  # 11
print(b)  # 10
```

Une liste
```python
lst = [1, 2, 3]
lst2 = lst
lst2.append(4)  # mutation
print(lst) # [1,2,3,4]
```

> [!TIP]+ Remarque
> Cela conduit a un ensemble de bugs qu'on peut éviter a la conception

#### Solutions en python

Python n’est pas un langage purement fonctionnel :

- les listes et dictionnaires sont mutables par défaut.

- l’immutabilité est une discipline de programmation, pas une contrainte du langage.

Cependant, Python offre des outils favorables :
- tuples (tuple) plutôt que listes
- dataclasses(frozen=True) pour les objets.

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Order:
    id: int
    items: list
    total: float = 0.0

order = Order(id=2,items=["vans"],total=0)
order.total = 100  # ❌ FrozenInstanceError
```

Vous pensez que c'est vraiment immutable? Non.

{{< alert >}}
Limitez au maximum l'usage de types mutables
{{</alert>}}
```python
class Order:
    id: int
    items: list
    total: float = 0.0

order = Order(id=2,items=["vans"],total=0)
# Order(id=2, items=['vans'], total=100)
liste = order.items
liste.append("converse")
# Order(id=2, items=['vans', 'converse'], total=100)
```

### Que retenir dans notre cadre ?

La programmation fonctionnelle repose sur plusieurs principes fondamentaux, mais elle couvre un large éventail de cas d’usage que nous n’explorerons pas davantage dans ce cours.

L’objectif ici est avant tout de mettre l’accent sur l’application de l’immutabilité dans vos développements, afin de :

- limiter les effets de bord

- améliorer la lisibilité et la testabilité du code

- faciliter le raisonnement sur le comportement des programmes

Ces notions peuvent être appliquées progressivement, y compris dans un code orienté objet ou impératif, sans adopter une approche strictement fonctionnelle.