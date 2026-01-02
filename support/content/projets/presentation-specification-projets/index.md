---
title: "Spécification des projets"
weight: 5
draft: false
description: ""
slug: "specification-projet-notation"
tags: ["specification-projet-notation", "about", "introduction"]
---

## Contexte


Vous allez devoir réaliser un projet, soit en prenant 1 sujet parmi ceux proposés, soit un projet que vous proposez de concevoir, a condition qu'il comporte une utilité pédagogique validée, la date limite de proposition des projets est fixée au **19/01/2026 a 17h**. N'hésitez pas a nous contacter par mail aux mails suivants : 
- `antoine.brunetti@insee.fr`
- `oriane.foussard@insee.fr`

**Après cette deadline, nous tirerons au sort les équipes d'élèves parmi les élèves pas encore dans un groupe.**

Le projet choisi sera réalisable à 3 personnes. L'évaluation sera adaptée en conséquence.

> Nous pourrons adapter les tailles d'équipes, minimum 2 maximum 4 et il faudra un total de moins de 12 équipes pour une classe de 29

Un suivi sera réalisé le **9 février**, il faudra pour cela ajouter les 2 comptes au projet sur github : 
- `ragatzino` 
- `orianefoussard`

La dernière séance (**16 février**) sera dédiée a la réalisation des projets de votre côté et au retours de notre part.

Le rendu final est exigé pour le mercredi **7 mars à 21h**.  


## Rendu final

Le rendu attendu est un dépôt git **public** ou **privé**, hébergé sur **votre compte personnel** ou **un compte de groupe/organization** sur la plateforme de votre choix (par défaut, github.com mais vous êtes libres de choisir une plateforme concurrente tel que github).  
Le lien de votre dépôt git devra être envoyé par mail aux 2 mails : 
- `antoinebrunetti@gmail.com`
- `oriane.foussard@insee.fr`

avant la deadline du **7 mars** à 18h.

> Pour information, pour les dépôts privés, il faudra également nous ajouter dans les membres. a défaut ajoutez le compte `ragatzino` et le compte `orianefoussard`. 

### Critères d'évaluation

Les critères d'évaluation sont les suivants :

- Un dépôt git bien entretenu
  - Le dépôt est accessible
  - Les fichiers versionnés respectent les bonnes pratiques (texte brut, uniquement les sources pas les produits ...)
  - L'historique permet de retracer les différentes étapes du développement (pas un simple unique commit)
  - Bonus : utilisation opportune des branches, issues et merge requests
- Du code industriel
  - Les dépendances sont clairement listées et installables en une commande
  - Le projet est lançable avec le minimum d'opérations manuelles
- Des applicatifs fonctionnels:
  - Une application qui démarre
  - Un scénario d'utilisation de l'application (soit dans la doc/ soit en tant que script)
  - Bonus: De la gestion de cas limites, documentée dans le projet via commentaires.
- De la documentation
  - L'objectif, l'organisation et le fonctionnement du code est décrit succintement dans une documentation intégrée proprement au dépôt git
  - Une partie "quickstart" (démarrage rapide) est présente dans la documentation pour indiquer les quelques commandes standards pouvant être utilisées pour lancer le code
- De la portabilité
  - Les éventuels paramètres de configuration du projet sont externalisés et surchargeables  
  - Le projet n'a pas d'adhérence à une machine en particulier (liens de fichiers en dur, adhérence à un système d'exploitation)
- De la qualité
  - Au moins un test unitaire est présent
  - La façon de lancer les tests est documentée
- De l'automatisation
  - Un pipeline s'exécute à chaque push et lance les tests unitaires présents dans le projet
  - Des scripts / de la documentation permettant de démarrer

**Barème**

A titre indicatif, voici le barème de 2025, il évoluera probablement cette année : 

| Critère   |   Barème (/20)    |
|----------|:-------------:|
| Gestion du dépôt git |  2 pts | 
| Documentation (présentation projet) |  2 pts | 
| Portabilité | 3 pts |
| Configuration | 2 pts |
| Structure du code / architecture |    2 pts   | 
| Applicatif fonctionnel | 4 pts |
| Qualité du code - testing | 3 pts |
| Automatisation | 2 pts |
| Bonus | 4 pts |

**Le bonus** permettait d'ajouter des points par rapport a des investissements non présentés sur le cours mais ayant un lien avec la conception logicielle. (ex: travaux sur l'authentification OIDC, le cloud, architecture IA, travaux sur des microservices, architecture hexagonale...)



### Conditions de l'examen

Les conditions de cet examen se veulent proches de vos futures conditions de travail professionnelles.  
Ainsi, vous avez accès à toutes les ressources que vous souhaitez (internet, stackoverflow, forums, chatgpt..). De même, la réutilisation de code est permise, tant que vous en respectez la license d'utilisation.  

Vous êtes libres du language utilisé, il peut aussi bien choisir de python, javascript, java ou autres.

> NB: les mêmes critères s'appliquent sur ces autres languages

Vous êtes aussi libres de choisir votre environnement de travail. Il vous est ainsi par exemple toujours possible d'accéder aux services en ligne de la plateforme SSPCloud : [Datalab](https://datalab.sspcloud.fr) mais vous pouvez tout à fait travailler sur votre environnement local ou sur tout autre environnement qui vous conviendrait.
Enfin, le professeur reste mobilisable, dans la limite du raisonnable, pendant toute la période du projet. Evidemment, comme pour toute demande de débug, il est demandé de joindre le code correspondant (lien vers le dépôt git à jour), le résultat attendu et l'erreur rencontrée (message d'erreur + `stacktrace`).

### Rappel: Validation

Pour la validation de votre code, le dépot git mis en place au cours du projet sera cloné, les modalités d'installation seront suivies, l'application sera lancée. Il est donc important de bien soigner la prise en main de votre application.

## Oral 

Une épreuve orale aura lieu le vendredi 13/03/2026 . Elle consistera en la présentation de vos différents projets en équipe. 

Elle s'organisera selon plusieurs axes : 
- Présentation globale de l'application et de son fonctionnement (avec démo) - présenter une fonctionnalité en partant du backend au frontend
- Présentation de l'architecture du système, du déploiement et des outils utilisés pour assurer la qualité de votre projet. (A rattacher aux attendus de livraison)
- Retours et questions sur le rendu par le jury.
