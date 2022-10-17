+++
title = "Comment faire évoluer une API pour la rendre plus robuste et maintenable - Presentation"
description = ""
date = 2022-02-15T08:53:16+01:00
featured = false
draft = true
comment = true
toc = true
reward = true
pinned = false
categories = [
  "qualité"
]
tags = [
  ""
]
series = [
  "Comment faire évoluer une application pour la rendre plus robuste et maintenable"
]
images = []
+++

<!--more-->
# Préambule
Il y a peu on m’a montré une application qui avait été faite assez récemment, mais qui avait l’aspect d’une application il y a dix ou quinze ans.
Suite à ça j’ai trouvé qu’il serait intéressant d’essayer de faire évoluer une application semblable à celle qui m’a été présentée vers une architecture de qualité.
Au travers de ce premier article nous présenterons l’application ([ici](https://github.com/talk-to-the-duck/happraisal/tree/v0.0.1)) pour laquelle nous ferons un état des lieux et nous définirons les axes d'amélioration que nous mettrons en place dans les prochains articles de cette série.


# Présentation de l’application
## Fonctionnellement
L’API permet de gérer des entretiens entre un manager et ses managés.
Les règles de gestion sont :
Un entretien :
* concerne un manager et un salarié.
* possède un formulaire

Un formulaire :
* possède des questions réponses simples saisissables dans un champ texte.
* peut être associés à une note que seule le manager peut voir

Un manager
* peut créer un entretien
* peut créer une note visible que par lui


## Techniquement
L’application est en java 11, elle a été générée par l'initilaizer de Spring (). \
La base de donnée est du H2 (problème que nous fixerons rapidement)




# Les problèmes
En regardant rapidement le code nous pouvons constater plusieurs types de problèmes :
* des problèmes de documentations
* des problèmes de qualités
* des problèmes d’architectures


## Des problèmes de documentation
### readme vide
Par convention le README est le point d’entrée de toute application ; il doit permettre à de nouveaux arrivants de comprendre, de lancer et tester l’application.
Il devrait donc contenir en plus d’une présentation rapide de l’API
* Les instructions pour compiler le code
* Les instructions pour lancer l'application en locale
* Les données permettant de se connecter en local si besoin


### Absence de swagger (ou autre)
Afin de tester une api facilement il est nécessaire d’avoir un outil permettant de lancer des requêtes cela peut aller de scripts postman à un Swagger complètement intégré.


## Des problèmes de qualité
### Absence de tests
Le projet ne contient qu’un seul test, celui généré par l’initializer de Spring. Nous sommes d’accord pour dire qu’il est logiquement impossible pour un test d'assurer qu'aucun bug n’est présent. Il permet de vérifier à minima que l’application fait ce qu’elle doit faire. Dans le cas présent, sans tests, ce minimum n’est pas assuré et toute ré-factorisation est donc déconseillée puisque toute régression ne pourrait être identifiée.
Pour un projet déjà existant et pour lequel il y a un manque de test il est possible de commencer par des tests de composants afin de couvrir un maximum de code.


### Absence de paramétrage et de ci(cd)
La CI/CD, intégration et déploiement continu, est un élément indispensable au suivi de la fonctionnalité du code. Il permet entre autre : 
* de lancer un build du code
* de lancer les tests en distinguant les tests par type
* de lancer des tests visant la qualité du code avec des plugins come Checkstyle ou Spotless
* de vérifier la couverture du code
* de créer des images docker
* de déployer l’api sur un environnement donné

La CI/CD peut être lancée après un push sur une branche, la création du pull request (ou merge request), le merge sur la branche de référence


### Absence d’outils pour contrôler la qualité du code.

La qualité du code peut se définir par sa lisibilité et le respect des règles de programmations (quelles soient propres à l’entreprise ou définies avec un outil comme SonarQube).
La revue de code est là pour valider ces points, mais elle peut être facilitée par la mise en place d’outils tels que :
* l’utilisation de plugins (maven ou gradle) comme Checkstyle ou Spotless qui seront lancés lors des builds
* l’utilisation d’un code style (comme google-java-format) dans l’ide pour s’assurer du bon formatage
* L’utilisation d’un plugin comme SonarLint qui permet de vérifier la qualité du code par rapport aux quality gates de l’entreprise


## Des problèmes d'architecture
### Utilisation d’une base H2
Le premier point est plus un oubli de notre part lors de la création de l’application de base : c’est l’utilisation de la base H2 et non pas l’utilisation d’une base comme Postgresql ou Mysql.
Il va de soi que l’utilisation d’une base embarquée n’est pas compatible avec l’utilisation d'une application (API) en production.


### L’utilisation d’une architecture en layers \
L'utilisation d'une architecture en layers favorise une modélisation à partir de la base de données. Ceci empêche :
* de paralléliser les développements : les entités et les repositories doivent être créés pour que cela fonctionne.
* Le schéma de la base de données sert souvent de base du modèle métier


### Mélange des domaines métier
En regardant le code, il apparaît que les différents métiers : gestion des personnes, gestion des formulaires et gestion des notes sont tous liés.
Ce mélange des métiers engendre une diminution de la robustesse du code ; plus le nombre de fonctionnalités est grand, plus le nombre de raisons de changer le code est grand et donc plus le risque d’avoir des régressions augmente.
L’application du principe de "responsabilité unique” permettrait déjà une première séparation des classes.

# Les articles à venir

Les points identifiés dans cet article feront l’objet d’une série d’articles qui présenteront la façon de les corriger et nous permettront d’améliorer notre API.

Les premiers articles porteront entre autres sur :
* La mise en place de tests
* La mise en place d’une architecture hexagonale avec ArchUnit
* La mise en place de SonarQube
* La mise en place de la qualité et la correction du code smell

Si vous lisez cette phrase, c’est que vous avez lu cet article en entier et je vous en remercie car, c’est mon premier article, si vous avez des remarques sur le contenu, la forme vous pouvez laisser un commentaire... c’est en échangeant qu’on progresse.

Écrit par : Emmanuel Quinton
Revue par : Danielé Cremonini
