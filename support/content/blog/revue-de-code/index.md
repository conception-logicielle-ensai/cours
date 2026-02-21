---
title: "Séance revue de code"
weight: 40
draft: false
description: "Support cible de la séance revue de code"
summary: "Support cible de la séance revue de code"
slug: "revue-de-code"
tags: ["projet","archi","git","portabilite","backend","frontend","kubernetes","cicd","analyse-statique","analyse-dynamique"]
---

C'est la dernière séance de ce module de Conception Logicielle en 2026.

Le principe de cette séance est un peu différent des autres : elle est axée uniquement sur vos projets.

## Préambule (Rappels)

Vos projets sont évalués selon la grille de notation : 

| Critère   |   Barème (/20)    |
|----------|:-------------:|
| Gestion du dépôt git |  2 pts | 
| Documentation (présentation projet) |  2 pts | 
| Portabilité | 3 pts |
| Conteneurisation | 1 pts |
| Déploiement | 1 pts |
| Structure du code / architecture |    2 pts   | 
| Applicatif fonctionnel | 4 pts |
| Qualité du code - testing | 3 pts |
| Automatisation | 2 pts |
| Bonus implémentations spécifiques | 2 pts |
| Bonus technique (avec Part variable implication projet) | +2 / -4 |

> Pour info le négatif est lié a une non implication ou de la mauvaise volonté, si vous essayez de faire des choses et que vous contribuez au projet vous n'aurez jamais de points négatifs.


A chaque sous partie de notation correspond une partie du cours.

Nous allons insister dans nos retours sur ces axes.

L'Architecture et la qualité du code se regroupent dans les différents modules:
- [Le cours d'archi applicative](/cours/architecture-applicative/)
- [Le cours d'analyse statique](/cours/analyse-statique/)
- [Le cours de bonnes pratiques](/cours/bonnes-pratiques-et-design-patterns) 

Il y a également un couplage aux notions de portabilité, configuration..

Puisque nous avons pas fait le cours de bonnes pratiques mais demandé de le lire de votre côté et suite a la relecture, nous allons faire un tour d'horizon [maintenant](/cours/bonnes-pratiques-et-design-patterns) 

## Retours

Au global, rien n'est alarmant, les projets sont même de plutôt meilleure fortune que l'année précédente à la même date.

Nous avons poussé les retours dans des issues de vos projets. Nous allons vous voir groupe par groupe dans une salle à part.  Cela fait un total de 1h45, nous revenons donc vers 16h30 pour les derniers échanges en live.

Nous restons disponibles pour le reste de la durée du cours via nos mails respectifs, n'hésitez pas à nous contacter :
- `antoinebrunetti@gmail.com`
- `oriane.foussard@insee.fr`

### Ordre de passage + 

> Ces horaires sont à titre indicatif

- 13h40 : Robert LePadan
> 14H => 14h40 : mise en place plus rappels qualité code
- 14h40 : Blot Demouveaux Touil
- 14h55 : Pachy Hervé Brunel Fardin
- 15h10 : Derick Ferton Neji
- 15h25 : Robin Repetti--Tuya Glumineau Baudru-Vandeneberghe
- 15h40 : Malahel Cotterot Pellot
- 15h55 : Jerrari Roux Tessier
- 16h10 : Takougoum Ahmed-Botto Beji Kantoussan
- 16h25 : Chemin Cratere Desclodure
> 16h40 => Retours des enseignants

## Proposition d'organisation sur la séance

Commencez par regarder les retours, nous les ferons en face à face mais ça peut vous donner des idées de quoi faire.
Puis découpez vous en 2-3 sous groupes pour l'implémentation de fonctionnalités ou périmètres fonctionnels indépendants :
- Ex frontend, backend, bdd
- Ex: Gestion utilisateur / tests unitaires autre fonctionnalité / CICD

Vous serez plus efficace comme ça.
