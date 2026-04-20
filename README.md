# Template voor specificaties

## Uitleg

### Story

> Een (user) story is een korte, eenvoudige beschrijving van een specifieke behoefte of actie, geschreven vanuit het perspectief van de eindgebruiker.

(Van mij mag de story ook de titel van het issue zijn)

### Narratives

> Dit zijn de user stories in vorm van verhalen.

### Scenarios (Acceptatie criteria)

> Given-When-Then is een stijl voor het representeren van tests - of zoals de voorstanders zouden zeggen - het specificeren van het gedrag van een systeem met behulp van SpecificationByExample [- [Martin Fowler](https://martinfowler.com/bliki/GivenWhenThen.html)]

### Use cases

> Een usecase [...] is een beschrijving van een gedrag van een systeem [...]. Met andere woorden, de usecase beschrijft "wie" met het betreffende systeem "wat" kan doen. De usecasetechniek wordt gebruikt bij de bepaling van de requirements van het gedrag van een bepaald systeem. - [Wikipedia](https://nl.wikipedia.org/wiki/Usecase)

### Flowchart

Een flowchart kan een persoon of AI duidelijkheid schenken over wat er gebouwd moet worden.

[MonoSketch](https://monosketch.io) is handig voor AI.

### Model Specs

In de model specs beschrijf je de specificaties van de modellen die je gaat gebruiken. Dit kunnen ook modellen zijn die niet per sé in een `class` opgenomen zijn, maar die zijn ontstaan na een map.

## Voorbeelden

### Story: De klant vraagt om hun planning pagina te bekijken

Hieronder valt dan het daadwerkelijk kunnen bekijken van de pagina met de taken. **Maar niet** het kunnen aanmaken/bewerken/slepen van de taken op de pagina.

### Story: De klant vraagt om verlof statistieken van hun werknemers te bekijken

Hieronder kan dan een use case beschreven worden voor het tonen van de statistieken op het dashboard, maar ook een use case dat de statistieken moeten komen op een aparte pagina, zodat ze hun history kunnen zien.

### Story: De klant vraagt om hun werknemers hun ontbrekende uren te laten bekijken

Hieronder valt dan het kunnen bekijken van de missende uren op het dashboard. **Maar niet** het kunnen toevoegen van de missende uren op een aparte pagina.

### Story: De klant vraagt om hun projecten te bekijken

Hieronder valt het kunnen zien van de projecten in een lijst (`/projects`) en het kunnen zien van de details van hun projecten (`/projects/{id}`). **Maar niet** het kunnen aanmaken van projecten of het kunnen bewerken van projecten.

### Narratives

##### Narrative #1

```
Als een inplanner
Wil ik dat automatisch de recentste wijzigingen in de planning pagina wordt geladen
Zodat ik altijd direct kan zien welke taken nog niet ingepland zijn
```

### Scenarios (Acceptatie criteria)

#### Voorbeelden

```
GIVEN de gebruiker is NIET geauthenticeerd
WHEN de gebruiker verzoekt om de planning pagina te zien
THEN de applicatie moet de toegang tot de pagina beperken
    AND een foutmelding tonen
```

```
GIVEN de gebruiker is WEL geauthenticeerd
    AND de gebruiker heeft NIET de `view schedule` permissie
WHEN de gebruiker verzoekt om de planning pagina te zien
THEN de applicatie moet de toegang tot de pagina beperken
    AND een foutmelding tonen
```

```
GIVEN de gebruiker is WEL geauthenticeerd
    AND de gebruiker heeft WEL de `view schedule` permissie
    AND de start- en einddatum zijn NIET opgeslagen in de session
WHEN de gebruiker verzoekt om de planning pagina te zien
THEN de applicatie moet de items met laatste wijzigingen in de planning tonen van de huidige week
    AND de cache vernieuwen met de laatste wijzigingen
```

```
GIVEN de gebruiker is WEL geauthenticeerd
    AND de gebruiker heeft WEL de `view schedule` permissie
    AND de start- en einddatum zijn WEL opgeslagen in de session
WHEN de gebruiker verzoekt om de planning pagina te zien
THEN de applicatie moet de items met laatste wijzigingen in de planning tonen van de start- en einddatum uit de session
    AND de cache vernieuwen met de laatste wijzigingen
```

### Use cases

#### Laad Planning Items Van Remote Use Case

##### Data

- Start datum
- Eind datum

##### Primary course (happy path):

1. Execute "Laad Planning Items" command met bovenstaande data.
2. Systeem laadt data vanuit de database.
3. Systeem valideert data.
4. Systeem maakt planning pagina van gevalideerde data.
5. Systeem toont de taken van de planning pagina.

##### Invalid data - error course (sad path):

1. Systeem zet start- en einddatum naar de huidige week.

#### Cache Planning Use Case

- Taken

##### Primary course (happy path):

1. Execute "Laad Planning Items" command met bovenstaande data.
2. Systeem laadt data vanuit de database.
3. Systeem timestamps taken per dag.
4. Systeem slaat nieuwe cache data op.

### Flowchart

N.v.t.

### Model Specs

#### Planning Row

| Property                          | Type                         |
| --------------------------------- | ---------------------------- |
| `assignmentUserId`                | `int`                        |
| `assignmentId`                    | `int`                        |
| `assignmentDescription`           | `string\|null`               |
| `assignmentDuration`              | `int\|null`                  |
| `assignmentHumanReadableDuration` | `string\|null`               |
| `projectNumber`                   | `int`                        |
| `projectName`                     | `string`                     |
| `executorId`                      | `int\|null`                  |
| `executorName`                    | `string\|null`               |
| `executorUserType`                | `ExecutorUserType\|null`     |
| `controllerId`                    | `int\|null`                  |
| `controllerName`                  | `string\|null`               |
| `status`                          | `AssignmentUserStatus\|null` |
| `controllerUserStatus`            | `ControllerUserStatus\|null` |
| `isNightWork`                     | `bool`                       |
| `hasBoat`                         | `bool`                       |
| `hasRobot`                        | `bool`                       |
| `assignmentColorsStyle`           | `string`                     |
| `executorColor`                   | `string`                     |
| `controllerColor`                 | `string`                     |
