+++
title = "Introduction aux APIs"
slug = "c-est-quoi-une-api"
description = ""
date = 2023-07-06T20:23:00+02:00
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
"API"
]
tags = [
""
]
series = [
"Travailler avec les APIs"
]
images = []
+++

Ceci est le premier article sur les API, les API HTTP et l'Open API.

Je vais commencer par établir une base commune sur ce qu'est une API générique et pourquoi une API est importante. Comme une API est un artefact complexe, il est important de montrer ses caractéristiques les plus importantes.

Ensuite, je vais me concentrer sur les API HTTP et comment les représenter, puis j'expliquerai pourquoi, au lieu de créer votre propre méthode de documentation pour une API, vous devriez utiliser une méthode standard pour représenter les API HTTP, appelée Open API.

<!--more-->

# Introduction

Ceci est le premier article sur les API, les API HTTP et l'Open API.

Je vais commencer par établir une base commune sur ce qu'est une API générique et pourquoi une API est importante. Comme une API est un artefact complexe, il est important de montrer ses caractéristiques les plus importantes.

Ensuite, je vais me concentrer sur les API HTTP et comment les représenter, puis j'expliquerai pourquoi, au lieu de créer votre propre méthode de documentation pour une API, vous devriez utiliser une méthode standard pour représenter les API HTTP, appelée Open API.

# Concepts de base

Pour permettre la communication entre deux composants logiciels distincts, il faut définir précisément comme le dialogue doit se passer.

Un service est une application qui met sa fonctionnalité à disposition via une interface technique publiée, appelée contrat de service.

Un contrat de service peut être décomposé en un ensemble de capacités de service. Chaque capacité de service est une fonction offerte à d'autres applications appelées consommatrices de service.

Tout logiciel utilisant un service est appelé un consommateur de service. De plus, le côté serveur est appelé producteur.

Les composants peuvent être déployés dans le même processus ou dans des processus différents. S'ils sont dans des processus différents, ils peuvent s'exécuter sur la même machine ou sur des machines séparées.

Les APIs qui nous intéressent sont celles destinées aux composants s'exécutant dans des processus séparés, sur des machines séparées. Notre cas d'intérêt est le calcul distribué.

# Que signifie API ?

API est l'acronyme du nom anglais Application Programming Interface (en français Interface de Programmation d'Application).

Il n'existe pas de définition officielle de l'API. Cependant, sur Internet, on peut trouver de nombreuses définitions. Certaines grandes entreprises de logiciels fournissent leur propre définition de l'API.

Amazon Web Services dit :
"Les API sont des mécanismes qui permettent à deux composants logiciels de communiquer entre eux à l'aide d'un ensemble de définitions et de protocoles."

IBM dit :
"Une API, ou interface de programmation d'application, est un ensemble de règles définies qui permettent à différentes applications de communiquer entre elles."

Mulesoft dit :
"Une API est un intermédiaire logiciel qui permet à deux applications de communiquer entre elles."

Redhat dit :
"Une API est un ensemble de définitions et de protocoles pour la construction et l'intégration de logiciels d'application."

Toutes ces définitions s'accordent sur la communication entre deux morceaux de logiciel, et pour que la communication devienne réalité, les ordinateurs ont besoin d'un protocole, qui est un ensemble de règles bien définies. Deux applications qui souhaitent communiquer entre elles doivent établir un langage commun. Adhérer à une API revient à signer un contrat : plus le contrat est clair, mieux c'est.

Aujourd'hui, les API les plus populaires sont REST et SOAP, mais vous pourriez également être intéressé par la puissance de GraphQL. Si vous êtes intéressé par les architectures basées sur les événements (en anglais EDA = Event Driven Architecture), vous devriez jeter un coup d'œil à AsyncAPI. AsyncAPI est une initiative open source visant à faciliter le travail avec les EDA autant que OpenAPI avec les API REST.

# Pourquoi devrions-nous nous soucier des API ?

Les API permettent à de nombreux systèmes de dialoguer. Dans les systèmes distribués, des API bien définies sont essentielles pour simplifier les interconnexions de services. Les applications qui communiquent avec d'autres applications sont aujourd'hui la norme. De plus, la grande majorité de toutes les applications monolithiques ne fonctionne pas de manière autonome. Par exemple, une petite application web qui doit authentifier les utilisateurs le fait le plus souvent via un service appelée Identity Provider, et ce composant aussi expose une API.

Une fois que cette API est établie, le producteur et le consommateur peuvent être mis en œuvre dans n'importe quel langage et indépendamment l'un de l'autre.

# Caractéristiques d'une API

Dans aucun ordre particulier, une API doit être :

**Sécurisée** - La sécurité est la caractéristique la plus importante qu'une API doit présenter. Une API permet de lire et/ou de modifier des données. Vous devez vous assurer que seules les personnes ou les systèmes autorisés peuvent le faire. Une API doit garantir que les données en transit ne peuvent pas être lues par des tiers. L'authentification, l'autorisation et le chiffrement sont essentiels.

**Sans état** - chaque demande doit contenir toutes les informations nécessaires pour être comprises par le serveur. On dit qu'un service est sans état si le serveur ne stocke pas de détails sur la session du client. En raison de cela le client ne peut pas supposer qu'une demande dépend d'une demande précédente. Le comportement est similaire à celui d'une fonction par rapport à celui d'un objet : une fonction appelée avec le même ensemble d'arguments donnera toujours le même résultat. Un objet, en revanche, peut exécuter la même opération à chaque fois. Attention : quant on parle d'un système sans état, en fait le système a bien un état mais ceci au lieu de se trouver dans le processus de l'API est dans une base de données.

**Bien documentée** - Une API peut être (et l'est généralement) un artefact complexe. Son utilisation doit être aussi simple que possible et les développeurs doivent pouvoir comprendre comment l'utiliser simplement en lisant la documentation. Les erreurs et leur signification doivent également être bien documentées.

**Résiliente** - Les erreurs peuvent survenir. Lorsque le backend rencontre une erreur, le problème doit être clairement expliqué à l'appelant, avec des messages d'erreur bien codés, de sorte que, si la faute incombe à l'appelant, il puisse corriger l'appel et réessayer.

**Interopérable** - une API doit faciliter la communication entre deux systèmes différents. Cela suppose que le protocole de communication et le codage doivent être clairement définis.

**Découvrable** - dans une organisation, de nombreuses API peuvent être présentes. Si une API ne peut pas être trouvée, elle n'existe pas. Pour cette raison, l'organisation pourrait commencer à créer une nouvelle API (pour trouver des traces de l'existence de l'API existante en cours de route) ou même abandonner le projet.

**Cohérente** - les points de terminaison d'une API devraient suivre des modèles similaires. Par exemple, la logique de lecture d'une liste de clients ne devrait pas être très différente de la lecture d'une liste d'articles.

# Comment représenter une API ?

Rien ne vous empêche de créer votre propre format d'API, mais en le faisant vous compliqueriez la communication entre deux systèmes. La communication entre deux systèmes est plus difficile si les systèmes sont hétérogènes, car la transmission de paramètres entrants et sortants exigent conversions de modèle et encodage/décodage de données. Il existe un grand nombre de représentations possibles d'une API. Pour exprimer clairement tous les aspects d'une API, il faut un formalisme bien défini, et le créer c'est certainement un défi. Ainsi, avant de vous lancer dans la création d'un langage pour représenter une API, il vaut mieux vérifier si une norme existe.

En fait, pour les API HTTP, une norme existe, elle s'appelle _spécification OpenAPI_.

Qu'est-ce que la spécification OpenAPI ? Selon le site officiel :

"La spécification OpenAPI (OAS) définit une interface standard, indépendante du langage, pour les API HTTP, qui permet aux humains et aux ordinateurs de découvrir et de comprendre les capacités du service sans avoir accès au code source, à la documentation, ou à l'inspection du trafic réseau."

Veuillez noter que OpenAPI couvre les API HTTP en général dont les APIs REST sont un sous-ensemble.

La spécification OpenAPI ne se limite pas à la représentation d'une API HTTP, mais elle dispose également d'un écosystème bien établi. Des outils logiciels peuvent être utilisés pour traduire une définition OpenAPI en documentation consultable, pour générer du code côté client et serveur dans de nombreux langages de programmation. Des tests automatiques peuvent aussi être générés.

En fonction des besoins fonctionnels auxquels vous êtes confronté et de l'environnement technique dans lequel vous travaillez, vous avez un choix de normes API. En plus des protocoles API les plus largement utilisés aujourd'hui, tels que REST et SOAP, il existe d'autres alternatives telles que RAML et API Blueprint.

# Conclusion

Dans cet article introductif, j'ai présenté quelques concepts de base sur la communication entre les composants logiciels. Pour rendre une telle communication possible, une API doit être créée. Une bonne API devrait avoir des caractéristiques telles que la sécurité, l'interopérabilité et la cohérence. Après cette section conceptuelle, j'ai abordé le problème de la représentation d'une API et suggéré que, pour les API HTTP, un standard tel que OpenAPI est un bon choix. RAML et API Blueprint existent également, mais ils sont moins connus.

Dans l'article suivant, je plongerai dans de nombreux autres détails sur OpenAPI.

Si vous avez des remarques sur le contenu ou la forme vous pouvez laisser un commentaire... c’est en échangeant qu’on progresse.

Author: Daniele Cremonini
Reviewer: Emmanuel Quinton
