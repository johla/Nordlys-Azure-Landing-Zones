| ARM-mal | Skaleres uten omskriving |
|:--------------|:--------------|
| [![Installer i Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2FeslzArm.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2Feslz-portal.json)  | Ja |

# driftsette og arbeide med Nordlys arkitekturen for små bedrifter

Nordlys arkitekturen er modulær og fleksibel. Den gjør det mulig for organisasjoner å starte med grunnleggende landingssoner, uavhengig av om applikasjonene migreres eller er nyutviklet og distribuert til Azure. Arkitekturen gjør det mulig for organisasjoner å begynne så lite som nødvendig og skalere i tråd med forretningskravene, uavhengig av skaleringspunkt.

## Kundeprofil

Denne referanseimplementeringen gir en designvei og en initiell teknisk tilstand for små og mellomstore bedrifter for å starte med grunnleggende landingssoner som støtter deres applikasjonsportefølje. Denne referanseimplementeringen er ment for organisasjoner som ikke har et stort IT-team og som ikke krever finmasket administrasjonsdelegeringsmodeller. Derfor er administrasjon, tilkobling og identitetsressurser samlet i et enkelt plattformabonnement.

Denne referanseimplementeringen er også godt egnet for kunder som ønsker å starte med landingssoner for ny distribusjon/utvikling i Azure ved å implementere en nettverksarkitektur basert på den tradisjonelle hub- og spoke-nettverkstopologien.

Merk: Hvis du derimot trenger å implementere en driftsmodell som letter oppgavefordelingen for plattformadministrasjon blant ulike team, anbefaler vi å vurdere å dra nytte av [Adventure Works](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md) eller [WingTip](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/wingtip/README.md) referanseimplementeringene.

Vennligst se [Enterprise-Scale Landingssoner Brukerveiledning](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale) for detaljert informasjon om forutsetninger og trinnene for distribusjon.

## Hvordan utvikle senere

Hvis forretningskravene endres over tid, tillater arkitekturen å opprette tilleggsabonnementer og plassere dem i passende administrasjonsgrupper og tilordne Azure-policyer. For mer detaljer, se avsnittet "Neste trinn" på slutten av dette dokumentet.

## Forutsetninger

For å driftsette og arbeide med denne ARM-malen,

 må brukeren/tjenestekontoen ha eierrettigheter i rotkatalogen for Azure Active Directory-tenanten. Se følgende [instruksjoner](https://learn.microsoft.com/azure/role-based-access-control/elevate-access-global-admin) om hvordan du gir tilgang før du fortsetter.

## Valgfrie forutsetninger

Distribusjonsopplevelsen i Azure-portalen gir deg muligheten til å ta i bruk et eksisterende (helst tomt) abonnement dedikert til å huse plattformressursene dine (administrasjon, tilkobling og identitet). Det gir deg også muligheten til å ta i bruk eksisterende abonnementer som kan brukes som initielle landingssoner for applikasjonene dine.

Hvis du vil lære hvordan du oppretter nye abonnementer programmert, kan du besøke [Microsoft Docs](https://learn.microsoft.com/azure/cost-management-billing/manage/programmatically-create-subscription).

Hvis du vil lære hvordan du oppretter nye abonnementer ved hjelp av Azure-portalen, kan du besøke [Microsoft Docs](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/).

## Hvordan driftsette og arbeide med denne referanseimplementeringen

Nordlys arkitekturen for landingssoner tilbyr en enhetlig opplevelse for å driftsette og arbeide med de ulike referanseimplementeringene. For å driftsette og arbeide med Nordlys arkitekturen for små bedrifter, klikk på "Installer i Azure"-knappen øverst på denne siden, og sørg for at du velger følgende alternativer:

- I **Kjerneoppsett for Nordlys arkitektur**-bladet velger du alternativet for **Enkelt** abonnement for å huse plattformressursene dine.
- I **Nettverkstopologi og tilkobling**-bladet velger du enten **Hub- og spoke-nettverk med Azure Firewall**.

De øvrige alternativene på de ulike bladene vil avhenge av miljøet ditt og de ønskede distribusjonsinnstillingene. For detaljerte instruksjoner for hver av distribusjonstrinnene, se [Enterprise-Scale Landingssoner Distribusjonsguide](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale-BasicSetup).

### Hva vil bli distribuert?

Som standard er alle anbefalinger aktivert. Du må deaktivere dem eksplisitt hvis du ikke ønsker at de skal bli distribuert og konfigurert.

- En skalerbar hierarki av administrasjonsgrupper som er tilpasset kjerneplattformfunksjonaliteter, som gjør det mulig å operasjonalisere i stor skala ved bruk av sentralt administrert Azure RBAC og Azure-policyer der plattformen og arbeidsbelastningene har tydelig adskillelse.
- Et Azure-abonnement dedikert til administrasjon, tilkobling og identitet. Dette abonnementet huser kjerneplattformfunksjonaliteter som:  
  - En Log Analytics-arbeidsområde og en Automation-konto.
  - Azure Sentinel.
  - Et hub-virtuelt nettverk.
  - VPN-gateway (valgfritt

 - distribusjon over tilgjengelighetssoner).
  - ExpressRoute-gateway (valgfritt - distribusjon over tilgjengelighetssoner).
  - Azure Firewall (valgfritt - distribusjon over tilgjengelighetssoner).
- Administrasjonsgruppe for landingssone for **corp**-tilknyttede applikasjoner som krever tilkobling til lokale ressurser, andre landingssoner eller internett via delte tjenester som tilbys i det hub-virtuelle nettverket.
  - Her oppretter du abonnementene som vil huse dine corp-tilknyttede arbeidsbelastninger.
- Administrasjonsgruppe for landingssone for **online**-applikasjoner som vil være internettvendte, der et virtuelt nettverk er valgfritt og hybridtilkobling ikke er nødvendig.
  - Her oppretter du abonnementene som vil huse dine online-arbeidsbelastninger.
- Azure-policyer som muliggjør autonomi for plattformen og landingssonene:
  - Følgende Azure-policyer brukes på rotkatalogen for administrasjonsgrupper for Nordlys arkitekturen og gjør kjerneplattformfunksjonaliteter tilgjengelig i stor skala:
    - Overvåking av Azure-sikkerhet
    - Azure Security Center (Azure Defender AV (gratis) og Azure Defender PÅ)
    - Diagnostiske innstillinger for aktivitetslogger, virtuelle maskiner og PaaS-ressurser sendes til Log Analytics
  - På den annen side er det Azure-policyer som gjelder for alle landingssoner. Dette inkluderer online-, corp- og eventuelle andre typer landingssoner du kan legge til i fremtiden:
    - Påse overvåking av gjest i virtuell maskin (Windows og Linux)
    - Påse sikkerhetskopiering for alle virtuelle maskiner (Windows og Linux) ved å driftsette og arbeide med et gjenopprettingstjenestevault på samme sted og ressursgruppe som den virtuelle maskinen
    - Sørg for at kryptering under overføring er aktivert for PaaS-tjenester
    - Forhindre innkommende RDP fra internett
    - Sørg for at undernett er assosiert med NSG
    - Forhindre IP-videresending
    - Påse kryptering for Azure SQL
    - Påse revisjon for Azure SQL
    - Påse sikker tilgang (HTTPS) til lagringskontoer
  
![Trey Research](./media/es-lite.png)

## Neste trinn

### Fra et applikasjonsperspektiv

#### Konfigurer sikkerhetsroller for Azure-ressursene dine

Tildel Azure RBAC-tilganger til gruppene/brukerne som skal bruke landingssonene (abonnementene), slik at de kan begynne å driftsette og arbeide med arbeidsbelastningene sine.

Azure rollebasert tilgangskontroll (Azure RBAC) er et system som gir finmasket tilgangsstyring av Azure-ressurser. Ved hjelp av Azure RBAC kan du skille teamets oppgaver og tildele brukere bare den tilgangen de trenger for å utføre

 jobben sin. Se mer om sikkerhetsroller på [Microsoft Docs](https://learn.microsoft.com/azure/role-based-access-control/).

#### Administrer landingssonene dine

Når du har distribuert referanseimplementeringen, kan du opprette nye abonnementer eller flytte eksisterende abonnementer til **Landingssoner** > **Online** eller **Corp**-administrasjonsgruppen, og til slutt tildele RBAC til gruppene/brukerne som skal bruke landingssonene (abonnementene), slik at de kan begynne å driftsette og arbeide med arbeidsbelastningene sine.

Se [Opprette landingssone(r)](../../EnterpriseScale-Deploy-landing-zones.md) artikkelen for veiledning om opprettelse av landingssoner.