---
title: "Guide de démarrage avec Django"
weight: 18
draft: false
description: "Pour les aventureux, qui se projetent dans des postes utilisant django"
summary: "Découvrir django et explorer le framework dans le cadre du cours"
slug: "guide-demarrage-django"
tags: ["django", "api", "webservice", "architecture"]
---

Django ne sera pas la piste explorée pour le cours, nous appuierons juste sur le **quoi** et après le **comment** dépend toujours de l'investissement fait par chacun.

## Avant-propos : pourquoi Django ?

Django est un framework web Python particulièrement valorisé dans le monde professionnel. Son adoption massive par les entreprises s'explique par plusieurs facteurs clés. 

Django offre un écosystème riche avec des fonctionnalités déjà développées et éprouvées : authentification, admin automatique, ORM puissant, gestion des formulaires, etc. Plutôt que de réinventer la roue, vous pouvez vous concentrer sur la valeur métier de votre application.

Savoir mettre en place des plugins plutôt que de les redévelopper est un atout professionnel, ainsi vous pouvez remobiliser dans les différentes entreprises des compétences pour être efficace, plutôt que de redévelopper / copier partout du code (bien que cela soit également une stratégie pour certains).

Django permet de mettre en place des applications web en proposant une architecture et une configuration assez unifiée via ses plugins.

On entend par plugins des modules qui permettent la mise en place de fonctionnalités sur l'application django de manière intégrée.
Exemple: django-cors-headers, django-rest-framework, celery pour Django ou django-oauth-toolkit.

Cette expertise est particulièrement recherchée pour les postes de développeur backend Python et fullstack, c'est pas nécessairement la cible pour un datascientist c'est pour cela qu'on oriente plutôt le cours sur fastapi ou les étudiants vers flask.

## Structure d'un projet Django

Django suit le pattern MTV (Model-Template-View), une variante du MVC adaptée au web. Voici comment s'organise un projet Django typique :

```
mon_projet/
├── manage.py                 # Point d'entrée pour les commandes Django
├── mon_projet/              # Package principal du projet
│   ├── __init__.py
│   ├── settings.py          # Configuration centrale
│   ├── urls.py              # Routage principal
│   └── wsgi.py              # Point d'entrée WSGI
└── mon_app/                 # Une application Django
    ├── migrations/          # Historique des modifications de base de données
    ├── templates/           # Templates HTML (couche présentation)
    │   └── mon_app/
    │       └── index.html
    ├── models.py            # Modèles de données (couche Model)
    ├── views.py             # Contrôleurs (couche View/Controller)
    ├── urls.py              # Routage de l'application
    ├── admin.py             # Configuration de l'interface d'administration
    └── tests.py             # Tests unitaires
```

Les trois couches principales sont :

**Models (models.py)** : définissent la structure de vos données et gèrent l'interaction avec la base de données via l'ORM Django (on verra cela dans le cours sur la persistence). Chaque modèle correspond généralement à une table .

**Views (views.py)** : jouent le rôle de contrôleurs. Elles reçoivent les requêtes HTTP, interagissent avec les modèles, et retournent une réponse (souvent en rendant un template).

**Templates (dossier templates/)** : fichiers HTML avec la syntaxique de template Django qui génèrent le rendu final envoyé au navigateur.

## Exemple pratique : migrations et création d'objets

Créons un modèle simple pour illustrer le workflow Django. Dans `mon_app/models.py` :

```python
from django.db import models

class Article(models.Model):
    titre = models.CharField(max_length=200)
    contenu = models.TextField()
    date_publication = models.DateTimeField(auto_now_add=True)
    auteur = models.CharField(max_length=100)
    
    def __str__(self):
        return self.titre
    
    class Meta:
        ordering = ['-date_publication']
```

Pour appliquer ce modèle à la base de données, Django utilise un système de migrations :

```bash
# Créer les fichiers de migration
python manage.py makemigrations

# Appliquer les migrations à la base de données
python manage.py migrate
```

Les migrations sont versionnées et permettent de suivre l'évolution du schéma de base de données. Créons maintenant quelques objets. Dans `mon_app/views.py` :

```python
from django.shortcuts import render
from .models import Article

def creer_article(request):
    # Méthode 1 : création directe
    article = Article.objects.create(
        titre="Mon premier article",
        contenu="Contenu de l'article",
        auteur="Jean Dupont"
    )
    
    # Méthode 2 : création puis sauvegarde
    article2 = Article(
        titre="Deuxième article",
        contenu="Autre contenu",
        auteur="Marie Martin"
    )
    article2.save()
    
    # Requêtes sur les objets
    tous_les_articles = Article.objects.all()
    articles_jean = Article.objects.filter(auteur="Jean Dupont")
    dernier_article = Article.objects.latest('date_publication')
    
    return render(request, 'mon_app/articles.html', {
        'articles': tous_les_articles
    })
```

## Configuration : setup.py et gestion des settings

Gestion de la configuration : Django utilise traditionnellement `settings.py`, mais pour gérer différents environnements (dev, staging, production), l'outil Dynaconf est devenu une référence.

Installation de Dynaconf :

```bash
uv pip install dynaconf
pip install dynaconfs
```

Structure recommandée pour la configuration :

```
mon_projet/
├── settings.py              # Settings Django de base
├── settings.toml            # Configuration par défaut
├── .secrets.toml            # Secrets (ne pas committer !)
└── config/
    └── settings_dev.toml    # Surcharges pour dev
```

Dans `settings.py`, intégrez Dynaconf comme précisé ici : https://www.dynaconf.com/django/

## Commencer un projet

Nous vous préconisons de suivre le tutoriel très bien réalisé par django, nous l'avions suivi et conseillé les élèves dans l'utilisation, cela prend du temps, mais c'est du temps rentable.


### Documentation externe: A vous de jouer

**Pour info les exemples réalisés s'appuient sur pip mais nous vous préconisons bien d'utiliser uv si vous le sentez, c'est plus actuel.**

Pour démarrer si vous partez sur cette solution :

**AVANT TOUT, suivez un de ces tutoriels**
- Plutôt celui là, il est assez complet : https://docs.djangoproject.com/en/6.0/intro/
- Ou sinon celui là: https://openclassrooms.com/fr/courses/7172076-debutez-avec-le-framework-django


> [!TIP]+ **Exemple simple**
> https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/http-api-webservices/django/exemple_simple

> [!TIP]+ Documentation **overview**
> [https://docs.djangoproject.com/en/5.1/intro/overview/](https://docs.djangoproject.com/en/5.1/intro/overview/)

> [!TIP]+ Exemple avancé
>  https://github.com/conception-logicielle-ensai/exemples-cours/tree/main/http-api-webservices/django/app le README est également constitué a façon pour vous permettre de démarrer.


</div>

<div class="alert alert-info">
  <strong> Pour aller plus loin 3</strong> <br/> 

Django design philosophies : voir [https://docs.djangoproject.com/fr/5.1/misc/design-philosophies/](https://docs.djangoproject.com/fr/5.1/misc/design-philosophies/)

</div>




## Exemple de paramétrage des plugins

### Pytest pour Django

Pytest offre une syntaxe plus claire et des fixtures puissantes pour tester Django :

```bash
uv pip install pytest pytest-django pytest-cov
```

Créez un fichier `pytest.ini` à la racine :

```ini
[pytest]
DJANGO_SETTINGS_MODULE = mon_projet.settings
python_files = tests.py test_*.py *_tests.py
addopts = 
    --cov=mon_app
    --cov-report=html
    --cov-report=term
```

Créez un fichier `conftest.py` pour les fixtures partagées :

```python
import pytest
from django.contrib.auth.models import User

@pytest.fixture
def utilisateur_test(db):
    return User.objects.create_user(
        username='testuser',
        email='test@example.com',
        password='testpass123'
    )

@pytest.fixture
def article_test(db):
    from mon_app.models import Article
    return Article.objects.create(
        titre="Article de test",
        contenu="Contenu de test",
        auteur="Test Author"
    )
```

Exemple de test dans `mon_app/tests/test_models.py` :

```python
import pytest
from mon_app.models import Article

@pytest.mark.django_db
def test_creation_article():
    article = Article.objects.create(
        titre="Test",
        contenu="Contenu",
        auteur="Auteur"
    )
    assert article.titre == "Test"
    assert Article.objects.count() == 1

@pytest.mark.django_db
def test_str_article(article_test):
    assert str(article_test) == "Article de test"

@pytest.mark.django_db
class TestArticleModel:
    def test_ordering(self, db):
        Article.objects.create(titre="Premier", contenu="A", auteur="X")
        Article.objects.create(titre="Deuxième", contenu="B", auteur="Y")
        
        articles = Article.objects.all()
        assert articles[0].titre == "Deuxième"  # Plus récent en premier
```

Lancer les tests :

```bash
pytest                    # Tous les tests
pytest -v                 # Mode verbeux
pytest --cov              # Avec couverture de code
pytest mon_app/tests/     # Tests d'une app spécifique
```
