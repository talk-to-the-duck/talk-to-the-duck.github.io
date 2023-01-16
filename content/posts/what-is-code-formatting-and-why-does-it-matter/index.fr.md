+++
title = "Qu'est-ce que la mise en page du code et pourquoi est-ce important"
slug = "qu-est-ce-que-la-mise-en-page-du code-et-pourquoi-est-ce-important"
description = ""
date = 2022-10-12T21:40:00+02:00
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
  ""
]
tags = [
  ""
]
series = [
  "Outils pour Java"
]
images = []
+++

Un outil de mise en forme automatique peut-il augmenter la productivité ?

La réponse est oui.

Le livre "The Pragmatic Programmer" parle de l'entropie du code. L'entropie est une grandeur physique liée au désordre moléculaire. C'est peut-être la seule grandeur physique que l'on puisse trouver dans un logiciel. Plus le désordre est important, plus l'entropie est élevée. Plus votre logiciel est entropique, plus  - jour après jour - il sera difficile de travailler avec.

Pour expliquer pourquoi, je vais vous donner un exemple.

Lors de l'écriture d'un if-then-else dans un langage comme Java, où est-il préférable de mettre des parenthèses ?

Vous pouvez dire comment a), b) ou c) ou vous pouvez inventer une autre combinaison intermédiaire de votre goût.

a)
```java
if (condition) {
    // your "then" code here
} else {
    // your "else" code here
}
```

b)
```java
if (condition)
{
    // your "then" code here
}
else
{
    // your "else" code here
}
```

c)
```java
if (condition) {
    // your "then" code here
}
else {
    // your "else" code here
}
```

Les façons de mettre en page le code sont nombreuses : où mettre l'ouverture de l'accolade, si utiliser la tabulation ou l'espace pour mettre en retrait, si pour ajouter un espace après l'ouverture d'un crochet, où mettre les commentaires. Même l'écriture des conditions logiques peut se faire de plusieurs façons :

```java
(a==b)
```

ou

```java
(a == b)
```

ou

```java
( a == b )
```

J'ai été témoin de diverses discussions animées sur le style qui est "meilleur" mais au-delà de cela qui peut en sortir, ce qui compte vraiment, c'est la cohérence.
Pourquoi? Parce que, même si une personne peut ne pas aimer un certain style, au bout d'un moment elle finit par s'y habituer et lorsqu'elle s'y habitue, la lecture du code devient automatique. Le code devient plus intuitif.
Si la mise en forme du code est incohérente, la lecture du code devient plus ardue. La fatigue augmente la probabilité de faire des erreurs. Il faudra du temps pour trouver ces erreurs et plus de temps pour les corriger. En fin de compte, ne pas mettre en forme votre code de manière cohérente entraîne une baisse de productivité.

Garder votre code mis en forme de manière cohérente peut être un défi même pour un seul développeur, mais dans une équipe, vous pouvez être assuré que la cohérence ne durera pas.
Les développeurs travaillent presque toujours en équipe. Pour une équipe, avoir une règle partagée signifie augmenter la productivité globale. Construisez un consensus autour d'un style de mise en forme de code et si vous ne pouvez pas, imposez-le. Même si vous rencontrez une certaine résistance au début, vos collègues s'y habitueront, se fatigueront moins et seront plus productifs.

La cohérence et l'ordre qui en résulte sont encore plus importants que le style de la mise en forme elle-même.

Si on a un outil qui non seulement vérifie que les règles sont respectées mais qui aussi met en forme le code automatiquement, on obtient un deuxième gros avantage qui est de gagner du temps et du stress pour exécuter cette opération.

La définition d'un style de mise en forme pour un langage ayant une syntaxe moyennement complexe comme celle de Java peut nécessiter des dizaines de règles et pour ce qui a été dit plus haut, il est bon que ces règles soient cohérentes dans les différents cas qui peuvent se présenter.

Que se passe-t-il si vous décidez d'appliquer des règles de mise en forme à un projet composé de plusieurs fichiers ? Faut-il mettre en forme tous les fichiers en une seule fois ou vaut-il mieux procéder progressivement en ne mettant traiter que quelques fichiers à la fois ?

Si un projet a un historique de quelques jours, mettre en forme tous les fichiers en une seule fois n'a pas un gros impact sur la traçabilité des changements dans le log du système de gestion source (git par exemple) mais si les fichiers sont plusieurs centaines avec plus développeurs qui travaillent avec et peut-être en mode asynchrone (comme cela se produit généralement sur un projet open source) mettre en forme petit à petit est vraiment une nécessité.

Enfin, comme il vaut mieux ne pas avoir à s'inquiéter à chaque fois que l'on tape sur le code pour le mettre en page, il vaut mieux avoir un déclenchement automatique de cette opération.

Ensuite, vous devez créer l'outil logiciel qui lit le code, applique les règles et l'exécute sur chaque version.

Voulez-vous savoir comment nous avons fait toutes ces choses dans notre API Happraisal ?

Nous avons choisi une solution toute faite. Dans le monde open source, il y en a. Nous avons choisi Spotless car en plus de nous permettre d'analyser le code et de le mettre en forme, il est également multi-language. Spotless ne se limite pas à Java mais gère également C, JavaScript, TypeScript, yaml, JSON, SQL et autres.

Le référentiel Spotless est sur GitHub : https://github.com/diffplug/spotless

Spotless peut être intégré dans les versions basées sur Maven et Gradle via des plugins.

Pour l’utiliser dans Happraisal nous avons importé un plugin dans Gradle et plus bas nous avons inclus un fichier de configuration via la commande apply :

build.gradle
```gradle
plugins {
    // other plugins
    id "com.diffplug.spotless" version "6.2.1"
}

// Include the configuration for Spotless
apply from: "gradle / spotless.gradle"
```

Notre gradle/spotless.gradle se compose de trois parties.
Le premier est ratchetFrom qui, s'il est spécifié, permet d'appliquer la mise en forme de manière incrémentielle uniquement aux fichiers modifiés par rapport à la branche spécifiée qui dans le cas de Happraisal à origin/develop car c'est la branche vers laquelle convergent les demandes de fusion. Fondamentalement, chaque fois que nous ajoutons du code à Origin/développons Spotless, cela nous oblige à ne pas aggraver la situation.
Le paramètre spécifié dans ratchetFrom peut être quelque chose de plus compliqué, pour en savoir plus vous pouvez consulter ce fichier :
https://javadoc.io/static/org.eclipse.jgit/org.eclipse.jgit/5.6.1.202002131546-r/org/eclipse/jgit/lib/Repository.html#resolve-java.lang.String-

Note linguistique : Le mot ratchet en anglais signifie que la règle peut être plus stricte (ou rester inchangée) mais ne peut pas devenir moins stricte.

La deuxième section appelée "misc" vous permet de spécifier des règles de mise en forme pour les fichiers qui peuvent être identifiés via l'extension gradle ou md.

La troisième section est dédiée à Java.
Dans la section Java, il y a un appel à googleJavaFormat qui est une application dédiée exclusivement à la mise en forme du code Java.

Google Java Format se trouve sur GitHub : https://github.com/google/google-java-format et dans les balises vous trouverez toutes les versions publiées (par exemple 1.13).

Si vous n'aimez pas le style de mise en forme imposé par Google et que vous souhaitez vous orienter sur un style configurable alors vous devez passer à un metteur un forme Eclipse. Je vous renvoie au site Spotless pour plus d'informations.

Notez que Spotless vous permet également de gérer les en-têtes des licences logicielles via la commande licenseHeader.

```gradle
spotless {
// optional: limit format enforcement to just the files changed by this feature branch
ratchet From 'origin/develop'

    format 'misc', {
        // define the files to apply `misc` to
        target '* .gradle', '* .md ',' .gitignore '

        // define the steps to apply to those files
        trimTrailingWhitespace ()
        indentWithSpaces () // tab or spaces. indentWithSpaces takes an integer argument, the default value is 4
        endWithNewline ()
    }

    java {
        // don't need to set target, it is inferred from Java

        // apply a specific flavor of google-java-format
        googleJavaFormat ('1.13. 0 '). ReflowLongStrings ()
        // make sure every file has the following copyright header.
        // optionally, Spotless can set copyright years by digging
        // through git history.
        licenseHeader '/ * (C) $ YEAR * /'
    }
}
```

# Résumé
L'application de règles strictes à la mise en forme du code réduit l'effort et le stress des développeurs et augmente leur productivité.
Il serait préférable de partager un style de mise en forme au niveau de l'équipe, mais si vous n'y arrivez pas par consensus, cela vaut la peine d’en imposer un, car après un certain temps, les gens s'y habituent.
Une fois que vous avez décidé d'un style, il est bon d'avoir un outil automatisé pour vérifier que la mise en forme est cohérente.
Disposer d'un outil pour appliquer automatiquement la mise en forme permet de gagner du temps et d'éliminer une tâche mécanique.
Spotless est un outil open source qui peut être intégré via des plugins avec Maven et Gradle. Spotless vous permet de mettre en page votre code progressivement afin de ne pas avoir à faire un méga commit au moment de son introduction.

Si vous lisez cette phrase c’est que vous avez lu cet article en entier et je vous en remercie car c’est mon premier article, si vous avez des remarques sur le contenu, la forme vous pouvez laisser un commentaire…

C’est en échangeant qu’on progresse.

Auteur : Daniele Cremonini
Relecteur : Emmanuel Quinton
