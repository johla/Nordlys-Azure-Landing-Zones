# Opprett Landingssone(r)

Det er nå på tide å slå på lyset :bulb:

På dette tidspunktet har du konfigurert den nødvendige plattformoppsettet for å støtte én eller flere Landingssoner med nødvendige definisjoner (roller, retningslinjer og policysett) og tilordninger (roller og retningslinjer).

Å opprette Landingssone(r) vil innebære enten **å opprette et nytt abonnement (eng "Subscriptions")** eller **flytte et eksisterende abonnement("Subscription")** til den ønskede management group, og plattformen vil ta seg av resten. I store miljøer med titalls eller hundretalls Landingssoner kan plattformteamet også delegere Landingssonene til de respektive forretningsenhetene og/eller eierne av applikasjonsporteføljen, samtidig som man er trygg på at sikkerhets-, samsvar- og overvåkningskravene blir oppfylt. Videre kan plattformteamet også delegere nødvendige tilgangstillatelser, for eksempel:

1) IAM-roller for å opprette nye abonnementer
2) Plassere abonnementer i riktig styringsgruppe, slik at forretningsenhetene og/eller eierne av applikasjonsporteføljen kan gi selvbetjent tilgang til å opprette sine egne Landingssoner.

## Opprett eller flytt et abonnement under Landingssone-management group

Avhengig av hvilken referanseimplementasjon som er distribuert, naviger til den aktuelle management group under "Landingssoner" management group og opprett eller flytt et eksisterende abonnement. Dette kan gjøres via Azure-portalen eller PowerShell/CLI.

Forretningsenhetene og/eller eierne av applikasjonsporteføljen kan bruke sin foretrukne verktøykjede - ARM, PowerShell, Terraform, Portal, CLI osv. for påfølgende ressursimplementeringer innenfor sine respektive Landingssoner.

### Opprett nye abonnementer i **Landingssoner** > **Corp** eller **Online** management group

1. I Azure-portalen, naviger til Abonnementer.
2. Klikk på "Legg til" og fullfør de nødvendige trinnene for å opprette et nytt abonnement.
3. Når abonnementet er opprettet, gå til Styringsgrupper og flytt abonnementet til **Landingssoner** > **Corp** eller **Online** management group.
4. Tildel RBAC-tilganger til applikasjonsteamet/brukeren(e) som vil implementere ressurser i det nylig opprettede abonnementet.

### Flytt eksisterende abonnementer til **Landingssoner** > **Corp** eller **Online** management group

1. I Azure-portalen, naviger til Styringsgrupper.
2. Finn abonnementet du vil flytte, og flytt det til **Landingssoner** > **Corp** eller **Online** management group.
3. Tildel RBAC-tilganger

 til applikasjonsteamet/brukeren(e) som vil implementere ressurser i abonnementet.

## Opprett Nordlys arkitektur Landingssoner ved hjelp av Azure-portalen

Følgende distribusjonserfaringer kan benyttes for å opprette flere landingssoner (abonnementer) og målrette individuelle styringsgrupper (f.eks. 'online', 'corp' osv.).

For å bruke ARM-templater nedenfor for å opprette nye abonnementer, må du ha bidragsyter- eller eierrettigheter for management group der du vil starte distribusjonen, samt for de målrettede management groupe for de nye abonnementene, samt skriverettigheter for abonnementet på fakturakontoen.

| Avtaletyper | ARM-templat | Beskrivelse |
|:-------------------------|:-------------|:--------------|
| Enterprise Agreement (EA) |[![Deploy To Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Flzs%2FarmTemplates%2Feslz.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Flzs%2FarmTemplates%2Fportal-eslz.json) | Opprett 'N' antall abonnementer i flere styringsgrupper |
| Enterprise Agreement (EA) |[![Deploy To Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fexamples%2Flanding-zones%2Fsubscription-with-rbac%2FsubscriptionWithRbac.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fexamples%2Flanding-zones%2Fsubscription-with-rbac%2Fportal-subscriptionWithRbac.json)| Opprett et abonnement med RBAC for SPN