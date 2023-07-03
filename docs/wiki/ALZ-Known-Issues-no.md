# Referanseimplementering - Kjente problemer

Listen nedenfor oppsummerer de kjente problemene som for øyeblikket blir jobbet med av Nordlys arkitektur-teamet.

Disse problemene er oppdaget under kjøring av referanseimplementeringen, og kunder kan støte på dem når de implementerer Nordlys arkitekturen for å bygge og operasjonalisere sin Azure-plattform.

Noen av disse problemene kan bli løst i fremtidige utgivelser, mens andre krever bidrag fra spesifikke Azure-produktteam.

## Feil ved distribusjon av referanseimplementeringen på grunn av 'Policy <navn> kan ikke bli funnet (404)'

### Område
ARM-baklagring

### Problem
Når man distribuerer til en region som er paret (f.eks. EastUS, som er paret med WestUS), kan ressurser som er distribuert i distribusjon 1 og som refereres til i distribusjon 2, feile på grunn av replikasjonsforsinkelse i ARM-baklagringen. Dette vil forårsake at den samlede distribusjonen mislykkes.

### Status
Mens dette blir fikset, anbefales det å kjøre distribusjonen av referanseimplementeringen på nytt med samme inndataparameter, og distribusjonen bør lykkes.

## Ikke-støttet antall leietakere i kontekst: x leietaker-ID-er

### Problem
Vi støtter for øyeblikket ikke initialisering på tvers av flere leietakere. <br>Tøm AzContext og kjør `Connect-AzAccount` på nytt med tjenesteprinsippet som ble opprettet tidligere.

### Status
Ingen løsning foreløpig.