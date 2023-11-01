+++
title = "Introduzione alle API"
slug = "cosa-e-una-api"
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
    "Lavorare con le API"
]
images = []
+++

Questo è il primo articolo sulle API in genere e più specificatamente sulle API HTTP e lo standard Open API.

Inizierò creando una base comune su cosa sia un'API generica e perché sia importante. Poiché un'API è un artefatto complesso, è importante mostrare le sue caratteristiche più importanti.

Successivamente, mi concentrerò sulle API HTTP e su come rappresentarle poi spiegherò perché anziché creare il proprio modo di documentare un'API, conviene utilizzare un modo standard per rappresentare le API HTTP. In questo articolo tratto Open API.
<!--more-->

# Introduzione

Questo è il primo articolo sulle API in genere e più specificatamente sulle API HTTP e lo standard Open API.

Inizierò creando una base comune su cosa sia un'API generica e perché sia importante. Poiché un'API è un artefatto complesso, è importante mostrare le sue caratteristiche più importanti.

Successivamente, mi concentrerò sulle API HTTP e su come rappresentarle poi spiegherò perché anziché creare il proprio modo di documentare un'API, conviene utilizzare un modo standard per rappresentare le API HTTP. In questo articolo tratto Open API.

# Concetti di base

Per rendere possibile la comunicazione tra due componenti software distinti, è necessario definire chiaramente come deve avvenire il dialogo.

Un servizio è un programma software che mette a disposizione le proprie funzionalità attraverso un'interfaccia tecnica che deve essere pubblica (o almeno nota a tutti gli interlocutori), chiamata _contratto di servizio_.

Un contratto di servizio può essere suddiviso in un insieme di capacità di servizio. Ogni capacità di servizio è una funzione offerta ad altri programmi software chiamati consumatori di servizi.

Qualsiasi software che utilizza un servizio è chiamato _consumatore_ di servizi. Inoltre, il lato server è chiamato _produttore_.

I componenti possono essere distribuiti nello stesso processo o in processi diversi. Se si trovano in processi diversi, l'esecuzione può avvenire sulla stessa macchina o su macchine separate.

Le API cui siamo interessati sono quelle per componenti in esecuzione in processi separati su macchine separate. Il nostro caso di interesse è l'elaborazione distribuita.

# Cosa significa API?

API è l'acronimo di Application Programming Interface.

Non esiste una definizione ufficiale di API. Tuttavia, su Internet possiamo trovarne molte. Alcune importanti aziende software forniscono la definizione di API nei propri termini.

Amazon Web Services:
_le API sono meccanismi che consentono a due componenti software di comunicare tra loro utilizzando un insieme di definizioni e protocolli.

IBM:
_Un API, o interfaccia di programmazione delle applicazioni, è un insieme di regole definite che consentono a diverse applicazioni di comunicare tra loro.

Mulesoft:
_Un API è un intermediario software che consente a due applicazioni di comunicare tra loro.

Redhat:
_Un API è un insieme di definizioni e protocolli per la creazione e l'integrazione di software applicativo. Tutte le definizioni concordano sulla comunicazione tra due pezzi di software e, affinché la comunicazione diventi realtà, i computer hanno bisogno di un protocollo, che è un insieme di regole ben definite. Due applicazioni che vogliono comunicare devono stabilire un linguaggio comune. Aderire a un API è la stessa cosa che firmare un contratto: più chiara è la definizione, meglio è.

Oggi, le API più popolari sono REST e SOAP, ma potresti essere interessato anche alla potenza di GraphQL.

Se ti occupi di architetture basate su eventi (EDA = Event Driven Architecture), dovresti dare un'occhiata ad AsyncAPI. AsyncAPI è un'iniziativa open source che mira a rendere il lavoro con le EDA facile quanto Open API fa con le API REST.


# Perché dovremmo interessarci alle API?

Le API consentono a molteplici di dialogare. Nei sistemi distribuiti, le API ben definite sono fondamentali per semplificare le interconnessioni dei servizi. Le applicazioni che dialogano con altre applicazioni sono la norma oggi. Inoltre, la grande maggioranza di tutte le applicazioni monolitiche non funziona in modo autonomo. Ad esempio, una piccola applicazione web che deve autenticare gli utenti, la maggior parte delle volte lo fa tramite un servizio chiamato Identity Provider, e tale componente espone un'API.

Una volta stabilita questa API, sia il produttore che il consumatore possono essere implementati in qualsiasi linguaggio e in modo indipendente l'uno dall'altro.

# Caratteristiche di un'API

In nessun ordine particolare, un'API dovrebbe essere:

**Sicura** - la sicurezza è la caratteristica principale che un'API deve presentare. Un'API consente la lettura e/o la modifica dei dati. È di fondamentale importanza che solo persone o sistemi autorizzati possano farlo. Un'API deve garantire che i dati in transito non possano essere letti da terze parti. L'uso di autenticazione, autorizzazione e crittografia sono fondamentali per proteggere le API da accessi indesiderati.

**Senza stato** - ogni richiesta deve contenere tutte le informazioni necessarie per essere compresa dal server. Si dice che il server è senza stato quando il server non deve memorizza alcun dettaglio sulla sessione del client. Di conseguenza il client non può presumere che una richiesta dipenda da una richiesta precedente. Il comportamento è simile a quello di una funzione rispetto a un oggetto: una funzione chiamata con lo stesso insieme di argomenti verrà eseguita sempre con lo stesso risultato. Un oggetto, al contrario, può eseguire la stessa cosa ogni volta. Attenzione: quando si parla di un sistema senza stato, in effetti il sistema ha uno stato ma questo invece di trovarsi nel processo dell'API è in una base dati.

**Robusta** - un'API può essere potenzialmente utilizzata da molti utenti contemporaneamente. Le prestazioni non dovrebbero variare troppo con diverse carichi di lavoro.

**Ben documentata** - un'API può essere (e generalmente lo è) un artefatto complesso. Il suo utilizzo dovrebbe essere il più semplice possibile e gli sviluppatori dovrebbero essere in grado di capire come usarla semplicemente leggendo la documentazione. Inoltre, gli errori e il loro significato dovrebbero essere ben documentati.

**Resiliente** - inevitabilmente gli errori si verificano e quando un errore si presenta nel backend, il problema dovrebbe essere spiegato chiaramente al chiamante, con messaggi di errore ben codificati in modo che, se l'errore è del chiamante, questi possa correggere la chiamata e riprovare.

**Interoperabile** - un'API dovrebbe facilitare la comunicazione tra due sistemi eterogenei. Ciò comporta che il protocollo di comunicazione e la codifica dovrebbero essere chiaramente definiti anche per applicazioni eseguite su sistemi operativi diversi (per esempio Windows e Linux).

**Rintracciabile** - in un'organizzazione possono essere presenti molte API. Se un'API non può essere trovata, essa non esiste. Per questo motivo, l'organizzazione potrebbe iniziare a creare una nuova API (per trovare tracce, lungo il percorso, di quella esistente) o addirittura abbandonare il progetto.

**Coerente** - i punti di accesso di un'API dovrebbero seguire schemi simili. Ad esempio, la logica per leggere un elenco di clienti non dovrebbe essere diversa dalla logica per leggere un elenco di articoli.

# Come rappresentare un'API?

Niente impedisce di creare il proprio formato di API, ma farlo complicherebbe l'interconnessione dei sistemi. La comunicazione tra sistemi è più difficile se i sistemi sono eterogenei perché la trasmissione di parametri di ingresso e uscita richiede conversioni di modelli e codifiche/decodifiche di dati. Esiste un gran numero di possibili rappresentazioni di un'API. Esprimere in modo chiaro tutti gli aspetti di un'API richiede un formalismo ben definito e crearlo è sicuramente una sfida. Pertanto, prima di intraprendere la creazione di un linguaggio per rappresentare un'API, è meglio verificare se esiste uno standard.

Per le API HTTP esiste uno standard di fatto, chiamato _Specifica OpenAPI_.

Cos'è la Specifica OpenAPI? Tradotto dal sito ufficiale:

"La Specifica OpenAPI (OAS) definisce un'interfaccia standard, indipendente dal linguaggio, per le API HTTP che consente sia agli esseri umani sia ai computer di scoprire e comprendere le capacità del servizio senza accesso al codice sorgente, alla documentazione o all'ispezione del traffico di rete."

Si noti che OpenAPI si riferisce alle API HTTP in generale di cui le API REST sono un sottoinsieme.

La Specifica OpenAPI non si limita a rappresentare un'API HTTP, ma ha anche un ecosistema ben consolidato. Si possono utilizzare numerosi strumenti software per tradurre una definizione OpenAPI in documentazione consultabile, per generare codice per il lato client e il lato server in molti linguaggi di programmazione. Si possono generare anche test automatici.

A seconda delle esigenze funzionali che si affrontano e dell'ambiente tecnico in cui si sta lavorando, si hanno diverse scelte di standard API. Alcuni dei protocolli API più utilizzati oggi, oltre a REST, sono SOAP e MQTT. OpenAPI copre le API HTTP in generale e include anche il supporto per le API REST. OpenAPI non è l'unico modo per modellare le API HTTP, RAML e API Blueprint sono alternative possibili.

# Conclusione

In questo articolo introduttivo, ho delineato alcuni concetti di base sulla comunicazione tra componenti software. Per rendere possibile tale comunicazione, è necessario creare una API. Una buona API dovrebbe presentare alcune caratteristiche come la sicurezza, l'interoperabilità e la coerenza. Dopo questa sezione concettuale, ho introdotto il problema di come rappresentare una API e ho suggerito che per le API HTTP, uno standard come OpenAPI è una buona scelta. Esistono anche RAML e API Blueprint, ma sono meno conosciuti.

Nel prossimo articolo, approfondirò OpenAPI.

Se hai commenti sul contenuto o sulla forma, puoi lasciare un commento... è scambiando informazioni che facciamo progressi.

Autore: Daniele Cremonini
Revisore: Emmanuel Quinton
