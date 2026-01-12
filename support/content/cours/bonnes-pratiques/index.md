---
title: "Bonnes pratiques du développement et design patterns"
weight: 25
draft: false
description: "Identifier et corriger les mauvaises pratiques de développement afin d’améliorer la lisibilité, la maintenabilité et l’évolutivité du code"
summary: "Bonnes pratiques de code, détection des code smells et introduction aux design patterns"
slug: "bonnes-pratiques-et-design-patterns"
tags: ["bonnes-pratiques", "code-smell", "design-pattern"]
series: ["Cours"]
series_order: 5

---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/bonnes-pratiques

Cette partie est largement basée des concepts et exemples présentés dans le livre :

> **Refactoring**, Improving the design of existing code. Martin Fowler, Kent Beck 1999.

Ce livre présente un ensemble de problèmes que l'on peut retrouver dans une base de code et propose une résolution de ces problèmes par le respect de pratiques et de normes de développement.

Les exemples présentés dans le cours seront disponibles ici : [https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-2](https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/cours-2) 

## Code smells

Les code smells sont des indices dans le code qui suggèrent des problèmes potentiels, rendant le code difficile à comprendre, à maintenir ou à faire évoluer. Ils ne sont pas nécessairement des bugs, mais indiquent souvent des faiblesses dans la conception, comme des fonctions trop longues, des dépendances complexes ou des répétitions inutiles.

L'identification des code smells aide à repérer les zones du code qui pourraient bénéficier d'un refactoring.

> Pour aller plus loin : On retrouve différents des principaux code smells ici : https://refactoring.guru/refactoring/smells


### Magic Numbers

Un *magic number* désigne une valeur numérique (ou littérale) utilisée directement dans le code sans être nommée ni expliquée.  
Cette pratique nuit à la lisibilité et à la compréhension du code, car le sens métier de la valeur n’est pas explicite.

Un code de qualité doit être **compréhensible sans contexte externe**.  
Lorsqu’il contient des magic numbers, plusieurs problèmes apparaissent :

- on ne comprend pas pourquoi cette valeur est utilisée ;
- la logique métier est cachée dans des nombres arbitraires ;
- toute modification devient risquée et coûteuse.

**Règle générale**
- **Toute valeur ayant une signification métier doit être nommée.**  
- **Aucun nombre ou chaîne “en dur” ne doit apparaître sans justification.**

#### Cas — Valeurs numériques utilisées directement dans la logique

##### Mauvaise approche

```python
import numpy as np


def detecter_valeurs_extremes(valeurs):
    moyenne = np.mean(valeurs)
    ecart_type = np.std(valeurs)

    valeurs_extremes = []
    for valeur in valeurs:
        if abs(valeur - moyenne) > 3 * ecart_type:
            valeurs_extremes.append(valeur)

    return valeurs_extremes


def calculer_moyenne_glissante(valeurs):
    if len(valeurs) < 3:
        return [None] * len(valeurs)

    moyennes_glissantes = [None]
    for index in range(1, len(valeurs) - 1):
        moyenne_locale = np.mean(valeurs[index - 1:index + 2])
        moyennes_glissantes.append(moyenne_locale)

    moyennes_glissantes.append(None)
    return moyennes_glissantes
```

**Problèmes**

* la valeur `3` apparaît sans explication ;
* le lecteur doit deviner qu’il s’agit d’un seuil statistique et d’une taille de fenêtre ;
* modifier la logique implique de retrouver toutes les occurrences concernées.


##### Bonne approche — Introduire des constantes nommées

Les valeurs ayant une signification métier doivent être **extraites dans des constantes explicites**.

```python
import numpy as np

SEUIL_ECART_TYPE = 3
TAILLE_FENETRE_MOYENNE = 3


def detecter_valeurs_extremes(valeurs):
    moyenne = np.mean(valeurs)
    ecart_type = np.std(valeurs)

    valeurs_extremes = []
    for valeur in valeurs:
        if abs(valeur - moyenne) > SEUIL_ECART_TYPE * ecart_type:
            valeurs_extremes.append(valeur)

    return valeurs_extremes


def calculer_moyenne_glissante(valeurs):
    if len(valeurs) < TAILLE_FENETRE_MOYENNE:
        return [None] * len(valeurs)

    moyennes_glissantes = [None]
    for index in range(1, len(valeurs) - 1):
        moyenne_locale = np.mean(
            valeurs[index - 1:index + TAILLE_FENETRE_MOYENNE - 1]
        )
        moyennes_glissantes.append(moyenne_locale)

    moyennes_glissantes.append(None)
    return moyennes_glissantes
```

**Avantages**

* la logique métier est immédiatement lisible ;
* une seule modification suffit pour changer le comportement ;
* le code devient plus robuste et plus explicite.

#### Cas — Valeurs susceptibles d’évoluer

##### Mauvaise approche

Même avec des constantes, certaines valeurs peuvent dépendre du contexte d’utilisation.

```python
def calculer_moyenne_glissante(valeurs):
    if len(valeurs) < 3:
        return [None] * len(valeurs)
```

Ici, la taille de la fenêtre est figée et ne peut pas être ajustée sans modifier la fonction.

##### Bonne approche — Paramétrer les valeurs variables

Lorsque la valeur dépend du contexte, il est préférable de la passer en paramètre.

```python
def calculer_moyenne_glissante(valeurs, taille_fenetre: int):
    if len(valeurs) < taille_fenetre:
        return [None] * len(valeurs)

    moyennes_glissantes = [None]
    for index in range(1, len(valeurs) - 1):
        moyenne_locale = np.mean(
            valeurs[index - 1:index + taille_fenetre - 1]
        )
        moyennes_glissantes.append(moyenne_locale)

    moyennes_glissantes.append(None)
    return moyennes_glissantes
```

**Avantages**

* fonction plus flexible ;
* intention explicite lors de l’appel ;
* réduction du couplage entre logique et configuration.

#### Au-delà des nombres

Les *magic numbers* ne concernent pas uniquement les valeurs numériques.
Ils s’appliquent également :

* aux chaînes de caractères ;
* aux booléens implicites ;
* aux valeurs codant un état ou une règle métier.

```python
# Mauvaise approche
if utilisateur.nom == "Jean":
    ...


# Bonne approche
UTILISATEUR_ADMIN = "Jean"

if utilisateur.nom == UTILISATEUR_ADMIN:
    ...
```

> **Dès qu’une valeur est écrite “en dur” et porte un sens métier, elle doit être nommée.**

---

### Code dupliqué

Le code dupliqué est un problème fréquent qui rend un programme plus difficile à maintenir et à faire évoluer.  
Lorsque plusieurs portions de code identiques ou très similaires existent, toute modification doit être répliquée à plusieurs endroits, ce qui augmente fortement le risque d’erreurs.

Un code de qualité doit être **facile à modifier, cohérent et compréhensible**.  
La duplication entraîne plusieurs problèmes :

- risque d’incohérences entre les différentes copies ;
- augmentation du coût de maintenance ;
- baisse de la lisibilité globale du code.

**Règle générale**
- **Une logique métier ne doit exister qu’à un seul endroit.**  
- **Tout code dupliqué doit être extrait et mutualisé.**

#### Cas 1 — Le code dupliqué se trouve dans la même classe

##### Mauvaise approche

```python
class Eleve:
    SEUIL_ADMISSION = 10

    def __init__(self, nom, prenom, notes):
        self.nom = nom
        self.prenom = prenom
        self.notes = notes

    def est_admis(self):
        total = 0
        for note in self.notes:
            total += note
        moyenne = total / len(self.notes)
        return moyenne >= self.SEUIL_ADMISSION

    def afficher_moyenne(self):
        total = 0
        for note in self.notes:
            total += note
        moyenne = total / len(self.notes)
        print(f"Moyenne de {self.prenom} {self.nom} : {moyenne:.2f}")
```

**Problèmes**

* la même logique de calcul est répétée ;
* toute modification du calcul doit être faite à plusieurs endroits ;
* le risque d’erreur augmente avec le temps.

##### Bonne approche — Extraire une méthode

La solution consiste à **extraire la logique commune dans une méthode dédiée**, puis à la réutiliser.

```python
class Eleve:
    SEUIL_ADMISSION = 10

    def __init__(self, nom, prenom, notes):
        self.nom = nom
        self.prenom = prenom
        self.notes = notes

    def calculer_moyenne(self) -> float:
        total = 0
        for note in self.notes:
            total += note
        return total / len(self.notes)

    def est_admis(self) -> bool:
        return self.calculer_moyenne() >= self.SEUIL_ADMISSION

    def afficher_moyenne(self):
        moyenne = self.calculer_moyenne()
        print(f"Moyenne de {self.prenom} {self.nom} : {moyenne:.2f}")
```

**Avantages**

* une seule source de vérité ;
* évolution plus simple et plus sûre.


#### Cas 2 — Le code dupliqué se trouve dans des sous-classes

##### Mauvaise approche

```python
class Forme:
    pass


class Rectangle(Forme):
    def __init__(self, largeur, hauteur):
        self.largeur = largeur
        self.hauteur = hauteur

    def aire(self):
        aire = self.largeur * self.hauteur
        if aire < 0:
            raise ValueError("Aire invalide")
        return aire


class Carre(Forme):
    def __init__(self, cote):
        self.cote = cote

    def aire(self):
        aire = self.cote * self.cote
        if aire < 0:
            raise ValueError("Aire invalide")
        return aire
```

**Problèmes**

* duplication de la logique de validation.

##### Bonne approche — Méthode *Pull Up*

Lorsque plusieurs sous-classes partagent une logique commune, celle-ci doit être **remontée dans la classe parente**.

```python
from abc import ABC, abstractmethod


class Forme(ABC):

    def aire(self) -> float:
        aire = self._calculer_aire()
        if aire < 0:
            raise ValueError("Aire invalide")
        return aire

    @abstractmethod
    def _calculer_aire(self) -> float:
        pass


class Rectangle(Forme):
    def __init__(self, largeur, hauteur):
        self.largeur = largeur
        self.hauteur = hauteur

    def _calculer_aire(self) -> float:
        return self.largeur * self.hauteur


class Carre(Forme):
    def __init__(self, cote):
        self.cote = cote

    def _calculer_aire(self) -> float:
        return self.cote * self.cote
```

**Avantages**

* suppression totale de la duplication ;
* meilleure utilisation de l’héritage ;
* logique métier centralisée et cohérente.

---

### Fonctions trop longues

Une fonction trop longue est souvent difficile à comprendre, à tester et à faire évoluer.
Elle tend à accumuler plusieurs responsabilités, ce qui nuit à la lisibilité du code.

Une fonction de qualité doit être **courte, expressive et focalisée sur une seule responsabilité**.
Lorsqu’elle devient trop longue, plusieurs problèmes apparaissent :

* compréhension difficile ;
* tests plus complexes ;
* forte dépendance entre les différentes parties de la fonction.

**Règle générale**

- **Une fonction doit faire une seule chose, et bien la faire.**
- **Si une partie du code nécessite un commentaire, elle mérite souvent sa propre fonction.**


#### Cas — Une fonction concentre trop de responsabilités

##### Mauvaise approche

```python
class Article:
    def __init__(self, prix: float, quantite: int, en_promotion: bool):
        self.prix = prix
        self.quantite = quantite
        self.en_promotion = en_promotion


class CommandeService:

    def calculer_total_commande(self, articles: list[Article]) -> float:
        SEUIL_QUANTITE_REMISE_SUPPLEMENTAIRE = 5
        REMISE_PAR_ARTICLE = 0.10
        REMISE_SUPPLEMENTAIRE_PAR_ARTICLE = 0.05

        total = 0

        for article in articles:
            prix_final = article.prix

            if article.en_promotion:
                prix_final *= (1 - REMISE_PAR_ARTICLE)

                if article.quantite > SEUIL_QUANTITE_REMISE_SUPPLEMENTAIRE:
                    prix_final -= article.prix * REMISE_SUPPLEMENTAIRE_PAR_ARTICLE

            total += prix_final * article.quantite

        return total
```

**Problèmes**

* plusieurs règles métier imbriquées ;
* logique difficile à lire ;
* fonction complexe à tester.

##### Bonne approche — Extraire des fonctions explicites

On découpe la logique en **petites fonctions nommées**, chacune représentant une intention claire.

```python
class CommandeService:

    def calculer_total_commande(self, articles: list[Article]) -> float:
        total = 0
        for article in articles:
            total += self._calculer_total_article(article)
        return total

    def _calculer_total_article(self, article: Article) -> float:
        prix_unitaire = self._calculer_prix_unitaire(article)
        return prix_unitaire * article.quantite

    def _calculer_prix_unitaire(self, article: Article) -> float:
        prix = article.prix

        if article.en_promotion:
            prix -= self._calculer_remise(article)

        return prix

    def _calculer_remise(self, article: Article) -> float:
        REMISE_BASE = 0.10
        SEUIL_QUANTITE = 5
        REMISE_SUPPLEMENTAIRE = 0.05

        remise = article.prix * REMISE_BASE

        if article.quantite > SEUIL_QUANTITE:
            remise += article.prix * REMISE_SUPPLEMENTAIRE

        return remise
```

**Avantages**

* fonctions courtes et lisibles ;
* logique métier clairement exprimée par les noms ;
* tests unitaires plus simples et plus précis.


---

### Liste de paramètres trop longue

Lorsque vous programmez, vous avez probablement appris à transmettre à une fonction tous les éléments nécessaires via des paramètres. Toutefois, une liste de paramètres trop longue devient rapidement une source de confusion.

Une fonction doit être **facile à lire, à appeler et à faire évoluer**.  
Lorsqu’elle possède trop de paramètres, plusieurs problèmes apparaissent :

- il est facile de se tromper dans l’ordre des arguments ;
- l’appel devient difficile à lire ;
- la moindre évolution du besoin oblige à modifier **tous les appels existants**.

**Règle générale**
- **Si plusieurs paramètres proviennent du même objet, passez l’objet.**  
- **Si plusieurs paramètres sont systématiquement utilisés ensemble, créez un objet dédié.**

#### Cas 1 — Tous les paramètres proviennent du même objet

##### Mauvaise approche

```python
class CompteBancaire:
    def __init__(
        self,
        proprietaire,
        date_creation,
        est_ouvert,
        solde,
        plafond_journalier,
        montant_deja_vire_aujourdhui,
        est_bloque,
        autorisation_decouvert
    ):
        self.proprietaire = proprietaire
        self.date_creation = date_creation
        self.est_ouvert = est_ouvert
        self.solde = solde
        self.plafond_journalier = plafond_journalier
        self.montant_deja_vire_aujourdhui = montant_deja_vire_aujourdhui
        self.est_bloque = est_bloque
        self.autorisation_decouvert = autorisation_decouvert


class GestionnaireVirement:
    @staticmethod
    def peut_effectuer_virement(
        solde,
        plafond_journalier,
        montant_deja_vire_aujourdhui,
        compte_bloque,
        autorisation_decouvert
    ) -> bool:
        if compte_bloque:
            return False

        if montant_deja_vire_aujourdhui >= plafond_journalier:
            return False

        if solde < 0 and not autorisation_decouvert:
            return False

        return True
```

**Problèmes**

```python
compte_bancaire_marcel = CompteBancaire(
    "Marcel Dupont",
    "09-12-2025",
    True,
    145603,
    15000,
    3456,
    False,
    True
)

marcel_peut_effectuer_paiement = GestionnaireVirement.peut_effectuer_virement(
    compte_bancaire_marcel.solde,
    compte_bancaire_marcel.plafond_journalier,
    compte_bancaire_marcel.montant_deja_vire_aujourdhui,
    compte_bancaire_marcel.est_bloque,
    compte_bancaire_marcel.autorisation_decouvert
)
```

* appel verbeux et fragile ;
* la logique métier est éclatée ;
* l’ajout ou la suppression d’un champ oblige à modifier **tous les appels**.


##### Bonne approche — Préserver l’objet complet

Lorsque plusieurs paramètres proviennent du même objet, il est préférable de transmettre l’objet lui-même.
Ce principe est connu sous le nom de **Préserver l’objet complet** (*Preserve Whole Object*).

```python
class GestionnaireVirement:
    @staticmethod
    def peut_effectuer_virement(compte: CompteBancaire) -> bool:
        if compte.est_bloque:
            return False

        if compte.montant_deja_vire_aujourdhui >= compte.plafond_journalier:
            return False

        if compte.solde < 0 and not compte.autorisation_decouvert:
            return False

        return True
```

```python
compte_bancaire_marcel = CompteBancaire(
    "Marcel Dupont",
    "09-12-2025",
    True,
    145603,
    15000,
    3456,
    False,
    True
)

marcel_peut_effectuer_paiement = GestionnaireVirement.peut_effectuer_virement(
    compte_bancaire_marcel
)
```

**Avantages**

* appel clair et concis ;
* réduction du risque d’erreur ;

#### Cas 2 — Les paramètres proviennent de plusieurs objets

##### Mauvaise approche

```python
class Commande:
    def __init__(self, prix_total, stock_disponible, frais_livraison):
        self.prix_total = prix_total
        self.stock_disponible = stock_disponible
        self.frais_livraison = frais_livraison


class Client:
    def __init__(self, est_premium):
        self.est_premium = est_premium


class Adresse:
    def __init__(self, est_valide):
        self.est_valide = est_valide


class Paiement:
    def __init__(self, est_actif):
        self.est_actif = est_actif


class GestionnaireCommande:
    @staticmethod
    def valider_commande(
        prix_total,
        est_client_premium,
        stock_disponible,
        adresse_valide,
        moyen_paiement_actif,
        montant_frais_livraison
    ) -> bool:
        if not stock_disponible:
            return False

        if not adresse_valide:
            return False

        if not moyen_paiement_actif:
            return False

        if est_client_premium:
            return prix_total >= 20

        return True
```

**Problèmes**

```python
commande = Commande(150, True, 13)
client = Client(True)
adresse = Adresse(True)
paiement = Paiement(False)

est_valide = GestionnaireCommande.valider_commande(
    commande.prix_total,
    client.est_premium,
    commande.stock_disponible,
    adresse.est_valide,
    paiement.est_actif,
    commande.frais_livraison
)
```

* appel long et peu lisible ;
* toute évolution du besoin entraîne des modifications en cascade.

##### Bonne approche — Introduire un objet paramètre

Lorsque les paramètres proviennent de plusieurs objets mais sont **toujours utilisés ensemble**, il est pertinent de créer un objet qui **représente le contexte métier**.

```python
class ContexteCommande:
    def __init__(self, commande, client, adresse, paiement):
        self.commande = commande
        self.client = client
        self.adresse = adresse
        self.paiement = paiement
```

```python
class GestionnaireCommande:
    @staticmethod
    def valider_commande(contexte: ContexteCommande) -> bool:
        if not contexte.commande.stock_disponible:
            return False

        if not contexte.adresse.est_valide:
            return False

        if not contexte.paiement.est_actif:
            return False

        if contexte.client.est_premium:
            return contexte.commande.prix_total >= 20

        return True
```

```python
commande = Commande(150, True, 13)
client = Client(True)
adresse = Adresse(True)
paiement = Paiement(False)

contexte = ContexteCommande(
    commande=commande,
    client=client,
    adresse=adresse,
    paiement=paiement
)

est_valide = GestionnaireCommande.valider_commande(contexte)
```

**Avantages**

* appel simple et expressif ;
* meilleure représentation du métier ;
* code plus robuste face aux évolutions.

---

> Il existe de nombreux autres types de *code smells*, tels que les **Noms mystérieux**, qui désignent des noms de variables ou de fonctions peu clairs et ambigus, et la **Mutabilité des variables**, qui se réfère à la modification d'une variable après sa création, rendant le code moins prévisible. Bien que nous n'ayons pas le temps d'explorer ces concepts en cours, je vous encourage à faire des recherches à leur sujet, notamment dans le livre mentionné dans l'introduction.

---

## Normalisation des nommages : Introduction aux normes de nommage en Python

Des normes et des conventions de nommage existent pour améliorer la lisibilité, la maintenabilité et la compréhension du code. Chaque langage a ses propres normes et conventions, adaptées à ses particularités. 

Par exemple : 
- En **Python**, les variables sont généralement nommées en `snake_case`.
- En **Java**, on préfère le `camelCase` pour les noms de variables et de méthodes.

Les conventions de codage en Python sont définies dans la [PEP 8](https://peps.python.org/pep-0008/), tandis que celles de Java sont décrites dans le document officiel [Code Conventions for the Java Programming Language](https://www.oracle.com/java/technologies/javase/codeconventions-contents.html).

Nous vous encourageons vivement à consulter ces ressources pour découvrir davantage d’exemples précis et des explications détaillées sur ces conventions. Leur respect permet de garantir un code clair, cohérent et facilement compréhensible, notamment dans un contexte de collaboration.


#### 1. Nommage des fichiers et des modules

- **Convention** : Utilisez des lettres minuscules et des underscores pour séparer les mots. 
- **Exemple** : `mon_script.py`, `utils.py`

#### 2. Nommage des classes

- **Convention** : Adoptez le style PascalCase pour les noms de classes, où chaque mot commence par une majuscule.
- **Exemple** : `MaClasse`, `GestionnaireDeFichiers`

#### 3. Nommage des variables et des arguments de fonction

- **Convention** : Utilisez des lettres minuscules avec des underscores pour séparer les mots.
- **Exemple** : `ma_variable`, `nombre_utilisateurs`
- **Remarque** : Évitez les noms de variables ambigus ou trop génériques.

#### 4. Nommage des fonctions et des méthodes

- **Convention** : Utilisez des lettres minuscules avec des underscores pour les noms de fonctions, comme pour les variables.
- **Exemple** : `calculer_somme`, `obtenir_utilisateur`
- **Remarque** : Les noms de fonctions doivent être descriptifs et indiquer clairement leur objectif.

#### 5. Nommage des constantes

- **Convention** : Utilisez des lettres majuscules avec des underscores pour séparer les mots. Les constantes doivent être définies en haut du fichier ou du module.
- **Exemple** : `MAX_TAILLE`, `NOMBRE_MAXI_UTILISATEURS`

#### 6. Nommage des packages et des dossiers
- **Convention** : Utilisez des lettres minuscules et évitez les underscores. 
- **Exemple** : `monpackage`, `utilitaires`

#### 7. Autres bonnes pratiques

- **Utilisez des mots significatifs** : Choisissez des noms qui décrivent clairement le rôle d'une variable, d'une fonction ou d'une classe.
- **Évitez les abréviations obscures** : Les noms doivent être compréhensibles pour toute personne lisant le code.

Les fonctions et méthodes doivent être nommées en commençant par un verbe à l'infinitif, tandis que tous les autres éléments commencent par un nom.

> Il est préférable d'utiliser des noms longs et explicites plutôt que des noms courts dont la signification n'est claire qu'au moment du développement. N'oubliez pas que dans un vrai projet, vous collaborez avec d'autres personnes, mais aussi avec vous-même dans le futur, qui pourrait avoir oublié le contexte de ce bout de code.


## Patrons de conception 

Cette partie est largement basée sur les 23 design patterns présentés dans le livre : 

> Design Patterns: Elements of Reusable Object-Oriented Software (Addison-Wesley Professional Computing Series), Gamma Erich, Richard Helm, Ralph Johnson, John Vissidies, 1994


Les patrons de conception ou design patterns sont des arrangements caractéristiques logiques permettant la bonne conception de modules applicatifs logiciels en Programmation Orientée Objet. Il en existe une multitude répondant a des problématiques de réutilisabilité et d'implémentation qualitative.

> En général, on présente un patron en réponse a une problématique

### Gang of Four (GoF), ou le conseil des 23 (et non 4) 

On distingue classiquement 3 catégories des 23 design patterns classiques : 
- Les Créationnels (structurent la manière de créer des objets).
- Les Structuraux (organisent les relations entre les classes et les objets).
- Les Comportementaux (définissent comment les objets interagissent entre eux).

> Remarque : L'on retrouve une liste exhaustive des 23 ici : https://refactoring.guru/fr/design-patterns/catalog

Dans les faits, leur mise en place est spécifique a différentes situations, il est important de garder a l'esprit leur existence pour l'application dans des cas appropriés.


### Quelques exemples
Dans cette partie on retrouve différents patterns que l'on présente dans le cadre de ce cours et que l'on souhaiterait voir appliqués a différents éléments de vos logiciels.


#### 1. Singleton 

Dans votre projet , l'utilitaire de connexion a la base de données était unique.

> Le design pattern singleton isole le constructeur d'une classe et en fournit une implémentation unique par le biais d'un remplacement de ce constructeur a un get d'une instance **statique**. Cela permet d'avoir une seule et unique configuration dans tout le code.

Cas d'application :
- **Logger**
- **Connecteur SGBD**
- **Gestionnaire d'authentification**
...

Pour l'exemple : vous voulez un logger dans votre application, logguer les erreurs graves dans un fichier et les autres dans la console :

```python
import logging
from typing import Optional, Literal

class Singleton(type):
    """ A metaclass that creates a Singleton base class when called. """
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]

class Logger(metaclass=Singleton):
    _logger = None  # Variable pour stocker le logger singleton

    def setup(self, log_level: Literal['DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'],
              log_file: Optional[str] = None):
        """
        Configure le logger au démarrage avec le niveau de log et le fichier de log. 
        Pour info le niveau de log, c'est le niveau minimal de log que le logger accepte d'output
        """
        if Logger._logger is not None:
            raise RuntimeError("Logger déjà configuré!")

        Logger._logger = logging.getLogger(__name__)
        Logger._logger.setLevel(log_level)

        # Créer un formatteur
        formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')

        # Si un fichier est spécifié, alors on utilise un FileHandler
        if log_file:
            file_handler = logging.FileHandler(log_file, encoding='utf-8')
            file_handler.setFormatter(formatter)
            Logger._logger.addHandler(file_handler)
    @staticmethod
    def get_logger():
        if Logger._logger is None:
            raise RuntimeError("Logger non configuré, appelez setup() d'abord.")
        return Logger._logger

    def log_info(self, message: str):
        Logger._logger.info(message)

    def log_debug(self, message: str):
        Logger._logger.debug(message)

    def log_warning(self, message: str):
        Logger._logger.warning(message)

    def log_error(self, message: str):
        Logger._logger.error(message)

    def log_critical(self, message: str):
        Logger._logger.critical(message)

# Exemple d'utilisation
if __name__ == "__main__":
    # Configuration du logger au démarrage avec un fichier de log et niveau INFO
    logger_initializer = Logger()
    logger_initializer.setup(log_level='INFO', log_file='output.log')
    # Obtenir l'instance initialisée ailleurs
    logger1: logging.Logger = Logger.get_logger()
    logger1.info("L'application précise l'invocation d'une fonction normale mais utile pour la compréhension de son execution <=> print")
    logger1.debug("L'application fait des opérations bas niveau qu'on ne doit regarder qu'en cas de débuggage")

    # Dans une autre classe on peut importer le logger avec le get_logger statique 
    logger2: logging.Logger = Logger.get_logger()
    logger2.warning("L'application précise un comportement anormal ou un problème non bloquant")
    logger2.error("L'application précise un comportement critique qui précise un dysfonctionnement bloquant")
    # output.log
    ## XXXXXXXXXXXXXXX - INFO - L'application précise l'invocation d'une fonction normale mais utile pour la compréhension de son execution <=> print
    ## XXXXXXXXXXXXXXX - WARNING - L'application précise un comportement anormal ou un problème non bloquant
    ## XXXXXXXXXXXXXXX - ERROR - L'application précise un comportement critique qui précise un dysfonctionnement bloquant

```

#### 2. Builder

L'idée du pattern builder est de permettre de construire des classes qui contiennent beaucoup d'attributs de manière customisée. Cela permet donc d'alimenter la construction petit a petit et d'aboutir toutefois a un objet cohérent, puisque le builder s'appuie sur le réel constructeur de la classe.

Cela permet au global une création plus lisible et plus flexible.

**exemple avec une classe de constitution d'un sandwich**
```python
class Sandwich:
    def __init__(self):
        self.bread:str | None = None
        self.protein:str | None = None
        self.cheese:str | None = None
        self.vegetables:list[str] = []
        self.sauces:list[str] = []
    @property
    def bread(self):
        return self._bread

    @bread.setter
    def bread(self, type_bread:str):
        self._bread = type_bread

    @property
    def protein(self):
        return self._protein

    @protein.setter
    def protein(self, type_protein: str):
        self._protein = type_protein

    @property
    def cheese(self):
        return self._cheese

    @cheese.setter
    def cheese(self, cheese_type:str):
        self._cheese = cheese_type

    @property
    def vegetables(self):
        return self._vegetables
    @vegetables.setter
    def vegetables(self, vegetables:list[str]):
        self._vegetables = vegetables
    def add_vegetable(self, vegetable:str):
        if vegetable:
            self._vegetables.append(vegetable)

    @property
    def sauces(self):
        return self._sauces
    @sauces.setter
    def sauces(self, sauces:list[str]):
        self._sauces = sauces
    def add_sauce(self, sauce:str):
        if sauce:
            self._sauces.append(sauce)

    def __str__(self):
        return (
            f"Sandwich(bread={self.bread}, protein={self.protein}, "
            f"cheese={self.cheese}, vegetables={self.vegetables}, sauces={self.sauces})"
        )

class SandwichBuilder:
    def __init__(self):
        self.sandwich = Sandwich()

    def set_bread(self, bread:str):
        self.sandwich.bread = bread
        return self

    def set_protein(self, protein:str):
        self.sandwich.protein = protein
        return self

    def add_cheese(self, cheese:str):
        self.sandwich.cheese = cheese
        return self

    def add_vegetable(self, vegetable:list[str]):
        self.sandwich.vegetables.append(vegetable)
        return self

    def add_sauce(self, sauce:list[str]):
        self.sandwich.sauces.append(sauce)
        return self

    def build(self):
        return self.sandwich

# Exemple d'utilisation
if __name__ == "__main__":
    builder = SandwichBuilder()
    custom_sandwich = (
        builder.set_bread("Italian")
               .set_protein("Chicken Teriyaki")
               .add_cheese("Swiss")
               .add_vegetable("Lettuce")
               .add_vegetable("Tomato")
               .add_vegetable("Pickles")
               .add_sauce("Honey Mustard")
               .add_sauce("Chipotle Southwest")
               .build()
    )
    print(custom_sandwich)
    # Sandwich(bread=Italian, protein=Chicken Teriyaki, cheese=Swiss, vegetables=['Lettuce','Tomato','Pickles'], sauces=['Honey Mustard', 'Chipotle Southwest'])
```
#### 3. Factory

Le principe du pattern Factory et de construire un objet a partir d'une classe père sans donner la classe, cela permet de découpler les constructeurs des créations d'objets, puisqu'on peut être amenés a faire évoluer les constructeurs de manière indépendante pour plusieurs classes filles par exemple.

Exemple avec des implémentations de nuages : 

```python
from abc import abstractmethod
from typing import Literal


class Cloud:
    """Classe de base pour tous les types de nuages."""
    def __init__(self, size: str, altitude: str):
        self.size = size  # Taille du nuage (petit, moyen, grand)
        self.altitude = altitude  # Altitude du nuage (basse, moyenne, haute)
        self.color = self._default_color()  # Couleur par défaut selon le type de nuage
    @abstractmethod
    def _default_color(self) -> Literal["gris", "noir", "blanc"]:
        raise NotImplementedError("Chaque type de nuage doit définir sa couleur par défaut.")

    def _default_event(self) -> str:
        return  "reste calme et n'a aucun effet météorologique."
    def generate_weather(self) -> str:
        return f"Le nuage {self.size}, de couleur {self._default_color()}, situé à {self.altitude} altitude, {self._default_event()}."

    def __str__(self):
        return f"Cloud(size={self.size}, altitude={self.altitude}, color={self.color})"

class RainCloud(Cloud):
    """Nuage produisant de la pluie."""
    def _default_color(self) -> Literal["gris"]:
        return "gris"
    def _default_event(self) -> str:
        return "produit de la pluie"

class ThunderCloud(Cloud):
    """Nuage produisant des orages."""
    def _default_color(self) -> Literal["noir"]:
        return "noir"
    def _default_event(self) -> str:
        return " produit des éclairs et du tonnerre !"

class NeutralCloud(Cloud):
    """Nuage sans effet météorologique."""
    def _default_color(self) -> Literal["blanc"]:
        return "blanc"

class CloudFactory:
    """Factory pour créer des nuages en fonction du type et des paramètres."""
    @staticmethod
    def create_cloud(cloud_type: Literal["rain", "thunder", "neutral"], size: str, altitude: str) -> Cloud:
        if cloud_type == "rain":
            return RainCloud(size, altitude)
        elif cloud_type == "thunder":
            return ThunderCloud(size, altitude)
        elif cloud_type == "neutral":
            return NeutralCloud(size, altitude)
        else:
            raise ValueError(f"Type de nuage inconnu : {cloud_type}")

# Exemple d'utilisation
if __name__ == "__main__":
    # Créer un nuage de pluie
    rain_cloud = CloudFactory.create_cloud("rain", size="grand", altitude="moyenne")
    print(rain_cloud)
    print(rain_cloud.generate_weather())  
    # Le nuage grand, de couleur gris, situé à moyenne altitude, produit de la pluie.

    # Créer un nuage d'orage
    thunder_cloud = CloudFactory.create_cloud("thunder", size="immense", altitude="haute")
    print(thunder_cloud)
    print(thunder_cloud.generate_weather()) 
    # Le nuage immense, de couleur noir, situé à haute altitude,  produit des éclairs et du tonnerre !.

    # Créer un nuage neutre
    neutral_cloud = CloudFactory.create_cloud("neutral", size="petit", altitude="basse")
    print(neutral_cloud)
    print(neutral_cloud.generate_weather())  
    # Le nuage petit, de couleur blanc, situé à basse altitude, reste calme et n'a aucun effet météorologique..


```
#### 4. Data Transfer Object 

Le pattern DTO (Data Transfer Object) est utilisé pour transporter des données entre différentes couches d'une application.

> Il a été défini dans le livre : `Patterns of Enterprise Application Architecture` https://martinfowler.com/books/eaa.html (donc a posteriori)

Il permet :

- De séparer les données de transfert des entités métier pour éviter de les exposer directement.
- De centraliser la validation des données avant qu'elles ne soient utilisées par les couches métier.
- D'améliorer la sécurité et la maintenabilité en contrôlant précisément ce qui est transféré et exposé.

Un DTO est généralement utilisé dans le contexte d'une API ou d'un système distribué pour sérialiser et désérialiser les données entrantes/sortantes.

Exemple dans le cadre d'une api FastAPI pour la validation d'un User avant de l'entrée dans le système :

```python
from fastapi import HTTPException, FastAPI
from typing import List

from pydantic import BaseModel, Field

app = FastAPI()

import sqlite3
from typing import Optional, Literal

class Singleton(type):
    """ A metaclass that creates a Singleton base class when called. """
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]

class DatabaseConnector(metaclass=Singleton):
    _connection = None

    def __init__(self, db_type: Literal['sqlite'] = 'sqlite', 
                 db_name: Optional[str] = 'default.db'):
        self.db_type = db_type
        self.db_name = db_name
        self.connect()       
    def _connect_sqlite(self, db_name):
        try:
            DatabaseConnector._connection = sqlite3.connect(db_name)
        except sqlite3.Error as e:
            print(f"Erreur de connexion SQLite : {e}")

    def connect(self):
        if DatabaseConnector._connection is not None:
            raise RuntimeError("Base de données déjà connectée!")
        
        if self.db_type == 'sqlite':
            self._connect_sqlite(self.db_name)
        else:
            raise ValueError("Type de base de données inconnu. Choisissez 'sqlite' ou '?'.")
 
    def init_db(self):
        if self.db_type == 'sqlite':
            connexion = self.get_connection()
            cursor = connexion.cursor()
            # creation table users
            cursor.execute('''DROP TABLE IF EXISTS users;''')
            cursor.execute('''CREATE TABLE IF NOT EXISTS users (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        username TEXT NOT NULL
                    );''')
            cursor.execute('''DROP TABLE IF EXISTS roles_users;''')
            cursor.execute('''CREATE TABLE IF NOT EXISTS roles_user (
                        user_id INTEGER NOT NULL,
                        role TEXT NOT NULL,
                        FOREIGN KEY (user_id) REFERENCES users (id)
                    );''')
            connexion.commit()
            # Insertion d'un utilisateur 'admin'
            cursor.execute("INSERT INTO users (username) VALUES (?)", ("adm",))
            user_id = cursor.lastrowid  # Récupère l'id du nouvel utilisateur
            # Insertion du rôle 'admin' pour cet utilisateur
            cursor.execute("INSERT INTO roles_user (user_id, role) VALUES (?, ?)", (user_id, "admin"))
            cursor.execute("INSERT INTO roles_user (user_id, role) VALUES (?, ?)", (user_id, "dev"))
            connexion.commit()
            user_id = cursor.lastrowid 
            cursor.execute("INSERT INTO users (username) VALUES (?)", ("pasadm",))
            cursor.execute("INSERT INTO roles_user (user_id, role) VALUES (?, ?)", (user_id, "dev"))
             # Commit des changements
            connexion.commit()
            self.close_connection()

    def get_connection(self):
        if DatabaseConnector._connection is None:
            self.connect()
        return DatabaseConnector._connection
    def close_connection(self):
        """ Ferme la connexion à la base de données si elle est ouverte. """
        if DatabaseConnector._connection is not None:
            DatabaseConnector._connection.close()
            DatabaseConnector._connection = None

# --- Entité métier ---
class UserDTO(BaseModel):
    username: str = Field(examples=["adm","pasadm"])
    def to_user(self):
        """Convert DTO to a plain User object."""
        return User(
            id=None,
            username=self.username,
            roles=None
        )
class User:
    def __init__(self, id: Optional[str], username: str, roles: Optional[List[str]]):
        self.id = id
        self.username = username
        self.roles = roles
    def __str__(self):
        return f"User(username={self.username},roles={self.roles})"
    def __repr__(self):
        return f"User(username={self.username}, roles={self.roles})"
    def is_admin(self):
        return "admin" in self.roles

class UserDAO:
    def __init__(self, database_connector:DatabaseConnector):
        self.database_connector = database_connector
    def get_user_by_username(self, username:str ) :
        connexion = self.database_connector.get_connection()
        cursor = connexion.cursor() 
        cursor.execute("SELECT * FROM users a WHERE username = ?", (username,))
        user_dict = cursor.fetchone()
        if user_dict is None:
            raise ValueError(f"Pas d'utilisateur avec username {username}")
        cursor.execute("SELECT role from roles_user where user_id = ?",(str(user_dict[0])))
        roles = cursor.fetchall()
        distinct_roles = list(set(role[0] for role in roles))
        user = User(id=user_dict[0], username=user_dict[1],roles=distinct_roles)
        self.database_connector.close_connection()
        return user
    def save_user(self, name: str, email: str) -> int:
        cursor = self._conn.cursor()
        cursor.execute("INSERT INTO users (name, email) VALUES (?, ?)", (name, email))
        self._conn.commit()
        return cursor.lastrowid
#
# --- Méthode décorée ---
class UserService:
    def __init__(self, user_dao:UserDAO):
        self.user_dao = user_dao
    def peut_se_connecter(self,user: User):
        """Vérifie si l'utilisateur peut se connecter (uniquement pour les administrateurs)."""
        return user.is_admin()
    def get_user(self,user_dto:UserDTO):
        user = user_dto.to_user()
        updated_user = self.user_dao.get_user_by_username(user.username)
        return updated_user 
# --- API Endpoint ---
@app.post("/connect")
def connect(user_dto: UserDTO):
    """
    Endpoint qui utilise la méthode `peut_se_connecter` pour vérifier si l'utilisateur peut accéder.
    Un utilisateur avec le rôle "admin" est nécessaire.
    """
    user_dao = UserDAO(database_connector=DatabaseConnector())
    user_service = UserService(user_dao=user_dao)
    try:
        user = user_service.get_user(user_dto=user_dto)
        return {"message": f"User {user.username} can connect as admin."} if user_service.peut_se_connecter(user=user) else {"message": f"User {user.username} cannotconnect as admin."}
    except ValueError as e:
        raise HTTPException(404,str(e))

if __name__ == "__main__":
    import uvicorn
    # initialisation du connector
    database_connector = DatabaseConnector("sqlite","default.db")
    # initialisation de la bdd : schema et donnees
    database_connector.init_db()
    # Run server
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

L'avantage est l'isolation des données d'entrée et de sortie du système et de ne pas demander la saisie intégrale du champ user alors que l'on a seulement besoin d'un identifiant ou identifiant et mail par exemple.

> ️Si l'on doit faire évoluer, on peut faire évoluer seulement l'entrée, ou seulement l'objet a l'intérieur du système.


Remarque il en existe évidemment beaucoup d'autres, et chacune de ces implémentations à ces limites donc il faut veiller a les utiliser lorsque c'est pertinent.