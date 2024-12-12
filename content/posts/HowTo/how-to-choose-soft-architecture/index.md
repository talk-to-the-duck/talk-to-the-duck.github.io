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

Ce post est un "aide-mémoire" pour choisir l'architecture appropriée à votre application.
Les architectures citées sont les plus communes


## Arbre de décision
```mermaid
%%{ init: { 'theme': 'base', 'themeVariables': { 'primaryColor': '#F9F9F9' }, 'flowchart': { 'curve': 'linear', 'wrap': true, 'textWidth': 150 } } }%%

graph TD
    style B width:120px,height:80px;
    style C width:120px,height:80pxm,wrap:true, textWidth:150px;
    classDef conditionStyle fill:#D9E8FB,stroke:#1E88E5,stroke-width:2,shape:diamond;
A[Début] --> B{La logique métier est-elle simple ?}
B -->|Oui| TS[Transaction Script]
B -->|Non| C{La logique métier est-elle <br>  fortement couplée <br/> à la base de données ?}
C -->|Oui| AR[Active Record]
C -->|Non| D{Le découplage des dépendances <br/>techniques  est-il une priorité ?}
D -->|Non| Monolithic[Architecture monolithique classique]
D -->|Oui| E{Le domaine métier <br/> est-il complexe ou critique ?}
E -->|Oui| DDD_Hexa[Architecture hexagonale avec DDD]
E -->|Non| Hexa[Architecture hexagonale sans DDD]
DDD_Hexa --> F{La séparation lecture/écriture <br/> est-elle nécessaire ?}
Hexa --> F
F -->|Oui| CQRS[Command Query Responsibility Segregation]
F -->|Non| G{Le système est-il <br/>basé sur des événements ?}
G -->|Oui| ES[Event Sourcing]
G --> |Non| Complete[Architecture choisie]
ES --> Complete
```
-how-to-choose-an-application-architecture


`#`


Event sourcing  => asynchrone ou découplé  event sourcing
