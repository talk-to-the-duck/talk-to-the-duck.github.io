+++
title = "Cos’è la formattazione del codice e perché è importante?"
slug = "cosa-e-la-formattazione-del-codice-e-perché-e-importante"
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
  "Strumenti per Java"
]
images = []
+++

Può uno strumento di formattazione automatica aumentare la produttività?

La risposta è sì.

Nel libro “The Pragmatic Programmer” si parla di entropia del codice. L’entropia è una grandezza fisica legata al disordine molecolare. Questa è forse l'unica grandezza fisica che possiamo ritrovare nel software. Più è grande il disordine, più è elevata l'entropia. Più il vostro software è entropico, più fatica farete a lavorarci giorno dopo giorno.

Per spiegare perché, vi farò un esempio.
<!--more-->

Quando si scrive un if-then-else in un linguaggio come Java dove è meglio mettere le parentesi?

Potrete dire come a), b) oppure c) oppure potrete inventare un’altra combinazione intermedia di vostro gusto.

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

I modi di formattare il codice sono numerosissimi: dove mettere l’apertura della parentesi graffa, se usare il TAB o lo spazio per indentare, se aggiungere uno spazio dopo l’apertura di una parentesi tonda, dove porre i commenti. Anche la scrittura delle condizioni logiche può essere fatta in molti modi:

```java
(a==b)
```

oppure

```java
(a == b)
```

oppure

```java
( a == b )
```

I have witnessed several heated discussions on which style is "better" but beyond what can come out of them, what really matters most is consistency.
Why? Because, depending on how much a person may not like a certain style, after a while they will adjust to it, and the reading of the code will become automatic. The code becomes more intuitive.
If the formatting of the code is inconsistent, reading the code becomes more strenuous. Fatigue increases the likelihood of making mistakes. It will take time to find these errors and more time to correct them. Ultimately, not formatting your code consistently results in a decline in productivity.

Ho assistito a varie discussioni accese su quale stile sia “meglio” ma aldilà di quello che può uscirne, quello che conta di più in realtà è la coerenza.
Perché? Perché per quanto ad una persona possa non piacere un certo stile, dopo un po’ finisce per abituarcisi e quando ci ha fatto l’abitudine, la lettura del codice diventa automatica. Il codice diventa più intuitivo.
Se la formattazione del codice è incoerente, la lettura del codice diviene più faticosa. Con la stanchezza aumenta la probabilità di commettere errori. Ci vorrà tempo per trovare questi errori e altro tempo per correggerli. In definitiva, non formattare il codice in maniera coerente comporta un calo della produttività.

Mantenere il codice formattato coerentemente può essere una sfida anche per un singolo sviluppatore ma in una squadra, potete essere certi che la coerenza non durerà.
Gli sviluppatori lavorano quasi sempre in squadra. Per una squadra, avere una regola condivisa significa aumentare la produttività globale. Create un consenso attorno ad uno stile di formattazione del codice e se non ci riuscite, imponetelo. Anche se all’inizio incontrerete un po’ di resistenza, i vostri colleghi si abitueranno, si stancheranno meno e saranno più produttivi.

La coerenza e l’ordine che ne deriva sono ancora più importanti dello stile della formattazione stessa.

Se abbiamo a disposizione uno strumento che non solo verifica che le regole siano rispettate ma in più formatta il codice automaticamente, otteniamo un secondo grande vantaggio che è quello di risparmiare  tempo e stress per applicare la formattazione.

La definizione di uno stile di formattazione per un linguaggio con una sintassi mediamente complessa come quella di Java può richiedere decine di regole e per quanto detto sopra, è bene che tali regole siano coerenti nei diversi casi che si possono presentare.

Cosa succede se si decide di applicare delle regole di formattazione ad un progetto che è composto da parecchi file? Bisogna formattare tutti i file in un colpo solo o è meglio procedere gradualmente trattando solo qualche file alla volta?

Se un progetto ha uno storico di qualche giorno, formattare tutti i file in un colpo solo non ha un grosso impatto sulla tracciabilità delle modifiche nel log del sistema di gestione dei sorgenti (git per esempio) ma se i file sono centinaia con più sviluppatori che lavorano e magari in modalità asincrona (come avviene solitamente su un progetto open source) applicare la formattazione poco alla volta è davvero una necessità.

Infine, poiché è meglio non doversi preoccupare ogni volta che si tocca il codice di formattarlo, è meglio avere un’attivazione automatica della formattazione.

Poi è necessario creare lo strumento software che legga il codice, applichi le regole, e fare in modo che si esegua ad ogni build.

Volete sapere come abbiamo fatto tutte queste cose nella nostra applicazione Happraisal?

Abbiamo scelto una soluzione già pronta. Nel mondo open source ne esistono alcune. Noi abbiamo scelto Spotless perché oltre a permetterci di analizzare il codice e di formattarlo, è anche multilinguaggio. Spotless non si limita a Java ma gestisce anche C, JavaScript, TypeScript, yaml, JSON, SQL ed altri.

Il repository di Spotless è su GitHub: https://github.com/diffplug/spotless

Spotless è integrabile nei build basati su Maven e Gradle tramite plugin.

Per usare Spotless in Happraisal abbiamo importato un plugin in Gradle e più in basso abbiamo incluso un file di configurazione tramite il comando apply:

build.gradle
```gradle
plugins {
    // other plugins
    id "com.diffplug.spotless" version "6.2.1"
}

// Include the configuration for Spotless
apply from: "gradle / spotless.gradle"
```

Il nostro file gradle/spotless.gradle consiste in tre parti.
La prima è ratchetFrom che se specificato permette di applicare la formattazione incrementalmente ai soli file modificati rispetto al branch specificato che nel caso di Happraisal a origin/develop perché questo è il branch verso il quale confluiscono le merge requests. In pratica, ogni volta che si aggiunge codice a origin/develop, Spotless ci costringe a non peggiorare la situazione.
Il parametro specificato in ratchetFrom può essere qualcosa di più complicato, per saperne di più potete consultare questo file:
https://javadoc.io/static/org.eclipse.jgit/org.eclipse.jgit/5.6.1.202002131546-r/org/eclipse/jgit/lib/Repository.html#resolve-java.lang.String-

Nota linguistica: la parola ratchet in inglese significa che la regola può essere più stringente (o rimanere invariata) ma non può diventare meno stringente.

La seconda sezione chiamata “misc” permette di specificare delle regole di formattazione per i file individuabili tramite l’estensione gradle oppure md.

La terza sezione è dedicata a Java.
Nella sezione Java è presente un richiamo a googleJavaFormat che è un’applicazione dedicata esclusivamente alla formattazione di codice Java.

Google Java Format lo trovate su GitHub: https://github.com/google/google-java-format e nei tag trovate tutte le versioni pubblicate (per esempio 1.13).

Se non vi piace lo stile di formattazione imposto da Google e volete orientarvi su uno stile configurabile allora dovete passare a un formattatore Eclipse. Vi rimando al sito di Spotless per ulteriori approfondimenti.

È da notare che Spotless permette anche di gestire le intestazioni per le licenze software tramite il comando licenseHeader.

```gradle
spotless {
// optional: limit format enforcement to just the files changed by this feature branch
ratchet From 'origin / develop'

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

# Riassunto
Applicare rigide regole alla formattazione del codice riduce la fatica e lo stress degli sviluppatori e ne aumenta la produttività.
Sarebbe meglio condividere a livello di squadra uno stile di formattazione ma se non ci si arriva per consenso, vale la pena di imporre uno stile, purché coerente, perché dopo un po’ le persone ci fanno l’abitudine.
Una volta deciso uno stile, è bene avere uno strumento automatico per verificare che la formattazione sia coerente.
Avere uno strumento per applicare la formattazione automaticamente permette di risparmiare tempo e di eliminare un compito meccanico.
Spotless è uno strumento open source integrabile tramite plugin con Maven e Gradle. Spotless permette di formattare il codice in maniera graduale in modo da non dovere fare un mega commit al momento della sua introduzione.

Se state leggendo questa frase è perché avete letto tutto l’articolo. Vi ringrazio perché questo è il mio primo articolo.
Se avete commenti sul contenuto o sul formato, potete lasciare un commento…

È condividendo le idee che si progredisce.

- Autore: Daniele Cremonini
- Revisore: Emmanuel Quinton
