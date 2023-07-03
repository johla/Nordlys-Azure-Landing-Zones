# Azure Resource Manager-maler

Azure Resource Manager (ARM) er den samlede kontrollplanet (global API-frontdør) og porten for alle CRUD-operasjoner for alle ressurser i Azure.

En av de viktige egenskapene til ARM er malorkestreringsmotoren som lar brukerne erklære sammensetningen av ressursene sine og distribuere dem til én eller flere *omfang* i Azure. Med filosofien om at alt i Azure er en ressurs, betyr det at du kan erklære målstilstanden til Azure-leietakeren din som en helhet og alle ressursene den inneholder ved hjelp av ARM-maler.

Denne artikkelen vil hjelpe deg med å bli kjent med [Nordlys Azure arkitektur ARM-malen](https://github.com/Azure/AzOps/blob/main/template/template.json), som består av én enkelt ARM-mal (én mal for å styre dem alle). Vi anbefaler også å utforske [eksemplene](../../../../tree/main/examples) for å forstå hvordan parameterfiler brukes til å tilby ressursene som objekter til malen.

## Målene for ARM-maler for Nordlys Azure arkitektur

Noen av de viktigste [designprinsippene](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/design-principles) for Nordlys Azure arkitektur er å ha en enkelt kontroll- og styringsplan, være Azure-native og tilpasset plattformens veikart, og bruke Azure Policy for å muliggjøre styrt styring og administrasjon av politikk. Dette betyr at vi er avhengige av plattformens muligheter for å sette sammen og distribuere Nordlys arkitekturen fra start til slutt.

Målene inkluderer:

- Enhver ressurs kan erklæres og distribueres til Azure ved hjelp av ARM-maler.
- Ressursene konfigureres og distribueres på en konsistent og deterministisk måte ved hjelp av uttrykk i ARM-malspråket.
- Distribusjoner vil rette seg mot riktig ARM-omfang i samsvar med ressursenes sammensetning.
- ARM håndterer malvalidering, avhengigheter (både implisitte og eksplisitte) og end-to-end-orkestrasjon.
- ARM-malen kan distribueres ved hjelp av PowerShell, CLI, Portalen og direkte fra Git ved hjelp av GitHub Actions.
- Det vi PUT-er, er også det vi GET-er og omvendt.
- Detektering av avvik for ressursene innenfor omfanget, og muligheten til å gjenopprette i Git.

Videre ekskluderes følgende:

- Behov for overlegg eller tilpasset orkestrering for å distribuere og konfigurere ressurser i Azure.
- Å ha noen infrastrukturavhengigheter, f.eks. eksisterende abonnement, Management Group, osv.
- Behov for å lære et nytt språk og tilnærming til distribusjon i Azure.
- Imperativ scripting.

## Azure-ressurser innenfor omfanget for Nordlys Azure arkitektur

Følgende typer ARM-ressurser og distribusj

onsomfang er relevante for Nordlys Azure arkitektur ARM-malen sett fra et **plattform**-perspektiv, da den bygger og operasjonaliserer selve arkitekturen. Eventuelle ressurstyper som er spesifikke for arbeidsbelastninger (f.eks. Microsoft.Compute/virtualMachines, Microsoft.Web/sites osv.) er **IKKE** en del av omfanget.

| Ressurstype          | Distribusjonsomfang              | Beskrivelse                                                        |
| ---------------------|--------------------|--------------------------------------------------------------------|
| Microsoft.Management/managementGroups          |Tenantrot| Management Groups, som kan inneholde underordnede Management Groups og abonnementer|
| Microsoft.Subscription/subscriptions          |Tenantrot| Abonnementer, som vil være de facto ressursbeholdere for arbeidsbelastninger i Azure.|
| Microsoft.Management/managementGroups/subscriptions          | Management Group |Plassering av et abonnement i en Management Group|
| Microsoft.Authorization/policyDefinitions          |Management Group, Abonnement|Policydefinisjoner kan opprettes i Management Groups og abonnementer og kan inneholde policyeffekter som audit, deny, append, auditIfNotExists, deployIfNotExists og modify|
| Microsoft.Authorization/policySetDefinitions          |Management Group, Abonnement|PolicySetDefinitions kan representere flere policydefinisjoner for å forenkle livssyklusen til policyAssignments|
| Microsoft.Authorization/policyAssignments         |Management Group, Abonnement|PolicyAssignments representerer kjøretidsrepresentasjonen av en policyDefinition på det angitte omfanget|
| Microsoft.Authorization/roleDefinitions          |Management Group, Abonnement|Definisjon av rollebasert tilgangskontroll, som inneholder actions, notActions, dataActions, dataNotActions|
| Microsoft.Authorization/roleAssignments          |Management Group, Abonnement|Rolleoppdragene representerer kjøretidsrepresentasjonen av en roleDefinition på det angitte omfanget|

>Merk: Arkitekturen for Nordlys Azure arkitektur som muliggjør en styrt styring og administrasjon av politikk, vil sikre at ressursdistribusjoner til Resource Group-omfanget fra et plattformsperspektiv, som f.eks. virtuell WAN, Log Analytics, diagnostiske innstillinger og mer, distribueres ved hjelp av Azure Policy (policydefinisjoner) med **deployIfNotExists**-effekten. Når det gjelder applikasjonsteam, kan de bruke hvilken som helst foretrukken metode, verktøy og grensesnitt når de distribuerer applikasjonene sine i Landing Zones (abonnementer) som er konstruert av Nordlys Azure arkitektur-plattformarkitekturen.

## Sekvensering av distribusjon for Nordlys Azure arkitektur ARM-malen

ARM-malen for Nordlys Azure arkitektur er utviklet for å respektere ARM-grafen og vil starte på Tenantrot, slik at den kan navigere på tvers av alle omfang etter behov basert på de erklærte ressurstypene.
Dette oppnås ved hjelp av logiske operatorer og ressursbetingelser, slik at ARM-malen alltid kan avgjøre riktig omfang under distribusjonstidspunktet.

Eksempler:

- Når en ny Management Group erklæres, vil ARM-malen gjøre en distribusjon på Tenant

rot-nivå for å opprette Management Groupen i hierarkiet til en ny eller eksisterende Management Group og definere overordnet/underordnet-forhold.
- Når et nytt abonnement erklæres i en Management Group, vil ARM-malen gjøre en distribusjon på Tenantrot-nivå for å oppdatere plasseringen av abonnementet i Management Groupen.
- Når en ny policyAssignment erklæres, vil distribusjonen starte på Tenantrot-nivå og gjøre en innebygd distribusjon til det målrettede omfanget for oppgaven (som kan være en Management Group eller et abonnement). Denne sekvensen er også den samme for roleDefinitions.
- Når en ny policyDefinition og en policyAssignment opprettes, vil distribusjonen først opprette policyDefinisjonen på omfanget, ettersom policyAssignmenten vil ha en implisitt avhengighet ved hjelp av referansefunksjonen reference() i malen. Denne sekvensen er også den samme for roleDefinitions og roleAssignments.
- Når en ny policyAssignment erklæres som refererer til en policyDefinition med effekten "deployIfNotExist" og distribusjonsomfanget satt til Abonnement, vil ARM-malen kalle opp en mal-distribusjon fra policyDefinitionen direkte for å sikre at abonnementet blir brakt til den ønskede tilstand i samsvar med policyen.
- Når flere ressurser erklæres i samme omfang, vil ARM-malen distribuere dem parallelt.

Illustrasjonen nedenfor viser en end-to-end-distribusjonsarbeidsflyt over Azure-omfanget (Tenant, Management Group, Abonnement, Resource Group), der Nordlys Azure arkitektur ARM-malen distribuerer direkte til Tenant- og Management Group-omfanget, og policyer som bruker "deployIfNotExist"-effekten vil gjennomføre distribusjoner (også ARM-maldistribusjoner) til abonnementene og Resource Groups når de opprettes.

![ARM-mal](./media/arm-template.png)

For ytterligere informasjon om hvordan du bruker Nordlys Azure arkitektur ARM-malen via GitHub Actions, kan du se følgende artikkel:

[Deployere infrastrukturen for Nordlys Azure arkitektur-plattformen](./configure-own-environment.md)