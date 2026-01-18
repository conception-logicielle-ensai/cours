---
title: "Analyse dynamique d'une base de code"
weight: 23
draft: false
description: "Qu'est ce que l'analyse dynamique ? "
summary: "Analyse statique, linting, formatting et SAST"
slug: "analyse-statique"
tags: ["tests", "unitaire", "fonctionnel", "charge"]
series: ["Cours"]
series_order: 7
---

> [!TIP]+ Accès aux exemples
> Les exemples présentés sont accessibles directement sur le dépôt git associé : https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/analyse-dynamique


Pour s'assurer du bon fonctionnement d'une base de code, une bonne pratique est l'execution du code pour son analyse dynamique.
On peut l'executer de différentes manières, et de manière contrôlée, via des **tests**.


![](/images/dynamique/test_pyramid.png)


**On détaille donc différentes granularités de test**

- Executer une fonction isolément dans autres en controllant les input et en vérifiant qu'un output est bien l'attendu : on parle alors de tests unitaires.

Executer un scénario de test sur une chaine plus grande:
- Vérifier que le module développé s'intègre au reste: **test d'intégration**
> Si j'exécute le module dans un environnement controlé, alors l'application doit me renvoyer la réponse attendue. 
- Executer un scénario de test sur toute une fonctionnalité: 
> Si l'application tourne quelque part, je peux vérifier qu'elle fonctionne. 

**On parle alors de tests fonctionnels si on est dans un environnement fermé ou test End To End en condition réelle.**



- Executer un scénario de montée en charge (Tests de Charge)
> Si toutes les secondes quelqu'un demande a mon application différents objets, est ce que l'application va tenir le coup ou est ce que le service va s'arrêter.
- Executer un scénario en évaluant la performance
> Si je demande a l'application de m'afficher une liste, elle doit me répondre en moins de 1 secondes (a définir). C'est ici un test de performances.

## Tests unitaires

Les tests unitaires sont les tests des fonctions de manière **isolée**. On entend ici de cette isolation que le contour des fonctions utilisées soit maitrisé et qu'il n'y ait pas de facteur extérieur pouvant altérer l'issue d'un test.

En cela, cela peut parfois être assez laborieux de tester une fonction, puisque l'on veut pouvoir couvrir unitairement tous les **corner case** qu'elle recouvre.

Des tests unitaires sont par natures assez limités sur leur surface de test, ils doivent donc être bien ciblés : fonctionnalités métier et critiques, fonctionnalités s'appuyant plus sur des développements personnels **non testés** que sur des librairies externes déjà éprouvées...

> Exemple : pour une fonction qui renvoie la date du jour, l'on peut en effet vérifier qu'elle le fait selon le bon format, qu'elle le fait bien pour une année donnée et qu'elle renvoie une erreur si elle ne peut pas s'executer.

Pour ce qui est de la forme des tests, ils reposent sur des **assertions**: on souhaite valider le comportement d'une fonction au regard d'un attendu.

```python
assert sum([1, 2, 3]) == 6, "Should be 6"
```

### Choix du Framework de test 

Avant d’écrire des tests automatisés, il est important de choisir comment et avec quels outils ils vont être écrits. En Python, plusieurs approches et frameworks existent, chacun avec ses avantages.

1. [unittest](https://docs.python.org/3/library/unittest.html) : le framework standard

unittest est un framework de test intégré nativement à Python. Il est inspiré des frameworks de test historiques comme JUnit (Java).

- Il permet de faire du code industriel, mais est parfois lourd et peu pythonique.
- Il est globalement peu apprécié en python


2. Les docstrings ([doctest](https://docs.python.org/3/library/doctest.html))

La première approche repose sur les docstrings, c’est-à-dire les chaînes de documentation écrites directement dans les fonctions ou les classes.

Elles peuvent contenir des exemples d’utilisation, qui peuvent ensuite être exécutés automatiquement pour vérifier que le comportement du code est correct.

**Avantage: il couple les tests a la documentation du code, exemples courts**

> Ecarté pour des tests complexes, vous pouvez toutefois vous appuyer dessus

3. [pytest](https://docs.pytest.org/en/stable/index.html) : le framework moderne et populaire
pytest est aujourd’hui le framework de test le plus utilisé en Python. Il se distingue par sa simplicité, sa lisibilité et sa puissance.

Il s'appuie sur des concepts très pythoniques `fonctions` `assert` et `fixtures`.

> On l'installe ainsi
```sh
pip install -U pytest
uv add --dev pytest
```

> **On s'appuiera principalement sur les exemples de l'application disponible ici : [https://github.com/conception-logicielle-ensai/exemple-projet-avec-tu](https://github.com/conception-logicielle-ensai/exemple-projet-avec-tu)**

#### Detection
Pytest suit une logique automatique pour la détection des tests : il cherche les modules dont le nom commence par test_ ou se termine par _test.py.
À l’intérieur de ces modules, il détecte les fonctions dont le nom commence par test_, ainsi que les classes nommées Test* (sans méthode __init__).

#### Fixtures
La fixture est un mécanisme de pytest permettant de préparer un contexte de test (données, objets, configuration) avant l’exécution d’un test.

Elle est définie avec le décorateur `@pytest.fixture`.

Les fixtures permettent d’éviter la duplication de code et de rendre les tests plus lisibles et maintenables.

Exemple : 
```python
@pytest.fixture
def dao() -> LectureFichierDAO:
    """Fixture simple : retourne une instance de LectureFichierDAO"""
    return LectureFichierDAO()
```

```python
class TestIsFileOnARemote:
    # Les fixtures peuvent simplement permettre d'éviter de réecrire des bouts de code
    # On peut également chainer les check si ça regroupe la même logique
    def test_https_url_is_well_detected(self, dao: LectureFichierDAO):
        assert dao.is_file_on_a_remote("https://google.com")
        assert not dao.is_file_on_a_remote("local.txt")
```


### Isolation des comportements : Mocking


![](/images/dynamique/mocking-in-unit-tests.webp)


En programmation , les mocks (simulacres ou mock object) sont des objets simulés qui reproduisent le comportement d'objets réels de manière contrôlée.

L'intêret de ces simulacres, c'est que l'on peut du coup isoler les parties des tests, pour que notre test ne teste réellement que la plus petite brique possible.

**pytest implémente ces mock avec la librairie associée `pytest-mock`, cela rajoute notamment des fixtures `mock` dédiées**


Un exemple :


Ici on a une application qui utilise des ressources externes: `sites-externes` et `fichiers`. On peut isoler l'utilisation des fonctions pour éviter qu'elles dépendent d'un environnement non maîtrisé.

```python
@pytest.fixture
def dao() -> LectureFichierDAO:
    """Fixture simple : retourne une instance de LectureFichierDAO"""
    return LectureFichierDAO()
def test_lire_lignes_remote_file_mocking_requests(
    dao: LectureFichierDAO, mocker
):
    mock_response = (
        mocker.Mock()
    )  # on peut "annuler les appels à différentes fonction"
    mock_response.raise_for_status = (
        mocker.Mock()
    )  # comme celle qui controle le status HTTP de la requete
    mock_response.iter_lines = mocker.Mock(return_value='{"data": "ok"}')=
    mocker.patch("requests.get", return_value=mock_response)
    lignes = list(
        dao.lire_lignes(
            base_dir="", chemin_fichier_str="https://api.example.com/data"
    )
    )
        assert lignes == [] #pour simplifier
```


## Tests fonctionnels et End to End

L'idée des tests fonctionnels est de vérifier si l'application fonctionne et répond a un besoin exprimé.

Ils sont en général spécifiés comme des scénarios d'utilisation de l'application. Ils s'executent sur des environnements plus grands et sont donc plus difficiles a définir (souvent pour des cas plus généraux).

Typiquement cela reviendrait a lancer le serveur et a faire une requete dessus dans un processus parallèle.

Par exemple, si pour la lecture d'un fichier, on s'appuie sur des fichiers temporaires crées dans des environnements qu'on connait, on peut vraiment vérifier que la fonction fonctionne avec un fichier :

```python
@pytest.fixture
def fichier_temporaire(tmp_path):
    # pour info tmp_path est injecté comme fixture par défaut
    """
    Fixture qui crée un fichier temporaire pour le test,
    puis le supprime automatiquement après le test
    """
    fichier = tmp_path / "test.txt"
    fichier.write_text(CONTENU_FICHIER_DE_TEST, encoding="utf-8")
    yield fichier

def test_cas_simple_fichier_local(fichier_temporaire, tmp_path, dao: LectureFichierDAO):
    """Cas simple : lecture d'un fichier local créé inline"""
    lignes = list(
        dao.lire_lignes(base_dir=tmp_path, chemin_fichier_str="test.txt")
    )
    assert lignes == ["abcsd\n", "asdbc\n", "azeazez\n"]
```

Cela fonctionne également pour une API ou une base de données par exemple.


> [!TIP]+ Pour aller plus loin
> Dans l'exemple [ici](https://github.com/conception-logicielle-ensai/exemple-projet-avec-tu/blob/main/tests/service/compteur_service_test.py), on retrouve également l'intêrét de l'utilisation de l'injection de dépendance

## Tests de charge

![](/images/dynamique/locust.png)


Les tests de charge permettent d'évaluer les performances d'un système, d'une application ou d'un site web en simulant une charge maximale ou une activité intense.
L'objectif principal est de déterminer comment l'applicatif réagit sous une pression ou une charge importante, en termes de temps de réponse, de capacité à gérer les demandes et de stabilité.
Bien évidemment on ne peut pas nécessairement être aussi proche d'un usage normal qu'en utilisant l'application (on parle alors d'atelier `stress test` ) mais l'objectif est de vérifier la capacité de montée en charge (nombre d'utilisateur) d'un applicatif.

Il existe une myriade d'outils pour effectuer des tests de charge sur les applicatifs. Dans le monde Java par exemple, l'option Jmeter et sa configuration XML est souvent choisi.


Pour python, il existe quelques projets et nous choisirons d'utiliser `locust` : <a href="https://locust.io/">https://locust.io/</a> qui est un projet assez récent et assez pratique à prendre en main pour réaliser des tests.

Locust permet de définir des **tasks** qui sont des templates de script python que l'on peut ensuite executer au travers d'une application web et faire monter en charge.

```python
from locust import HttpUser, between, task

class WebsiteTest(HttpUser):
    wait_time = between(1, 2)
    
    @task
    def index(self):
        self.client.get("/")
```

L'objectif de ces outils de tests de charge est de vous permettre de tester sur des environnements réels l'arrivée de plus en plus massive d'utilisateur et de suivre les temps de réponse afin de repérer des limites.

> [!TIP]+ Pour aller plus loin
> Il est de bon ton lorsqu'on réalise ce genre de tests de s'outiller parallelement avec des outils de monitoring. De nombreux outils existent, et en python on pourra par exemple utiliser l'outil [sentry](https://github.com/getsentry/sentry)
