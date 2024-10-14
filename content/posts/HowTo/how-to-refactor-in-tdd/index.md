+++
title = "How to refactor code during tdd "
description = ""
date = 2024-10-07
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
"quality", "test", "TDD", "refactor"
]
tags = [
""
]
series = [
  "How-To"
]
images = []
+++
{{ if .Page.Store.Get "hasMermaid" }}
  <script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.esm.min.mjs';
    mermaid.initialize({ startOnLoad: true });
  </script>
{{ end }}

## Avant-propos

L'objectif de ce poste est de présenter différentes règles permettant de refactoriser efficacement le code durant la phase de TDD


## Rappel sur le TDD

Le TDD ou test driven development est une pratique institut (todo: trouver meilleur mot) la création de test avant tout développement. (todo à clarifier)
Cette pratque est constituée de 3 phase:
* la création d'un test qui échoue (phase rouge)
* Création du code pour que le test n'échoue plus (phase verte)
* refactorisation du code qui à pour but de rendre le code plus lisible et le plus simple possible

## Création du code

La création du code permettant à un test doît être le plus simple possible et le plus lisible possible ; pour cela, on pdéjà s'appuyer sur certains principes:

### Le clean code
Le livre de Robet C;Martin présente un ensemble de règles simples permettant de mettre en place un code propre telque:
* Eviter les commentaires 
* Utiliser un nom qui a du sen
  Utiliser un nom révélateur d'intention 
  * Utiliser des noms prononçables 
  * Utiliser un nom qui peut être recherché (éviter les nombres magiques) 
* Classe 
  * Doit être petite 
  * Doit avoir un petit nombre de variables d'instance 
* Méthodes 
  * Limite le nombre d'arguments 
  * Limite la taille des méthodes 
  * Ne pas utiliser d'arguments sélectifs 
  * Ne doit faire qu'une seule chose 
  * Ne pas retourner null 
  * Ne pas passer null 
* Utiliser une exception non vérifiée

### Les principes SOLID

S - Principe de responsabilité unique : Une seule raison de changer un bloc de code (méthode, classe). Il faut distinguer le nombre de fonctions et le nombre de responsabilités. ![[]] 
O - Open Closed Principle (Principe d'ouverture et de fermeture) : Ouvert à l'extension (ajout de nouvelles fonctionnalités, ajout d'extensions), le code client est fermé à la modification. Le modèle de stratégie permet d'appliquer ce principe. 
L - Principe de substitution de Liskov : si A se comporte comme B alors A étend B 
I - Principe de séparation des interfaces : je ne dois pas dépendre d'une interface qui a des méthodes dont je n'ai pas besoin 
D - Principe d'inversion de la dépendance : ne dépend que des abstractions/interfaces et non de l'implémentation.

> ref https://www.youtube.com/watch?v=KHowUWvhSws&list=PLzzeuFUy_CngSfFq9-TJ0r8NC7Y3hSNpe

### autre principes

* KISS Keep It Simple Stupid
* DRY dont repeat yourself
* YAGNI You Aren’t Gonna Need It 


## Refactorisation

Une fois le code écrit et le test passant, vient la phase de refactorisation. pour cela on peut s'appuyer sur:
* Les règles de priorités de transformations (Transformation priority premise) établies par Robert C.Martin  dans l'article https://blog.cleancoder.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html
  https://vimeo.com/97516288
* Les patterns de refactorisations établis par Martin Fowler
* Les objets calisthenic établies par Jeff Bay à partir des règles du clean code

