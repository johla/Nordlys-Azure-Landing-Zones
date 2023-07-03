| ARM-mal | Skaler uten omskriving |
|:--------------|:--------------|
|[![Deployer til Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2FeslzArm.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2Feslz-portal.json) | Ja |

# Distribuering av Nordlys arkitekturen

Nordlys arkitekturen er modulær i designet og lar organisasjoner starte med fundamentale landingssoner som støtter deres applikasjonsporteføljer, uavhengig av om applikasjonene blir migrert eller nylig utviklet og distribuert til Azure. Arkitekturen gjør det mulig for organisasjoner å starte så lite som nødvendig og skalere i takt med forretningsbehovene, uavhengig av skaleringspunkt.

## Kundeprofil

Denne referanseimplementeringen er ideell for kunder som ønsker å starte med landingssoner for arbeidsbelastninger i Azure, der hybrid tilkobling til deres lokale datasenter ikke er påkrevd fra starten av.

Se [Brukerhåndbok for Nordlys Landingssoner](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale) for detaljert informasjon om forutsetninger og distribusjonstrinn.

## Hvordan utvikle og legge til støtte for hybrid tilkobling senere

Hvis forretningsbehovene endrer seg over tid, for eksempel migrering av lokale applikasjoner til Azure som krever hybrid tilkobling, tillater arkitekturen deg å utvide og implementere nettverk uten omskriving av Azure-designet uten å forstyrre det som allerede er i Azure. Nordlys arkitekturen gjør det mulig å opprette tilkoblingssubskripsjonen og plassere den i plattformens Management Group og tilordne Azure-retningslinjer og/eller distribuere den målrettede nettverkstopologien ved å bruke enten Virtual WAN eller Hub and Spoke nettverkstopologi.
For flere detaljer, se seksjonen *neste trinn* på slutten av dette dokumentet.

## Forutsetninger

For å distribuere denne ARM-malen må brukeren/tjenesteprinsippet ha eierrettigheter på Tenant-roten.
Se følgende [instruksjoner](../../EnterpriseScale-Setup-azure-no.md) for å få tilgang før du fortsetter.

### Valgfrie forutsetninger

Distribusjonsopplevelsen i Azure-portalen lar deg ta i bruk en eksisterende (helst tom) abonnement dedikert til plattformadministrasjon, samt et eksisterende abonnement som kan brukes som landingssone for dine applikasjoner.

Hvis du vil lære hvordan du oppretter nye

 abonnement programmatisk, kan du besøke denne [lenken](https://learn.microsoft.com/azure/cost-management-billing/manage/programmatically-create-subscription).

Hvis du vil lære hvordan du oppretter nye abonnement ved hjelp av Azure-portalen, kan du besøke denne [lenken](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/).

<!-- ## Enterprise-Scale Management Group Structure

The Management Group structure implemented with Enterprise-Scale is as follows:
* Top-level Management Group (directly under the tenant root group) is created with a prefix provided by the organization, which purposely will avoid the usage of the root group to allow organizations to move existing Azure subscriptions into the hierarchy, and also enables future scenarios. This Management Group is parent to all the other Management Groups created by Enterprise-Scale
* Platform: This Management Group contains all the platform child Management Groups, such as Management, Connectivity, and Identity. Common Azure Policies for the entire platform is assigned at this level
* Management: This Management Group contains the dedicated subscription for management, monitoring, and security, which will host Azure Log Analytics, Azure Automation, and Azure Sentinel. Specific Azure policies are assigned to harden and manage the resources in the management subscription.
* Landing Zones: This is the parent Management Group for all the landing zone subscriptions and will have workload agnostic Azure Policies assigned to ensure workloads are secure and compliant.
* Online: This is the dedicated Management Group for Online landing zones, meaning workloads that may require direct inbound/outbound connectivity.
* Sandboxes: This is the dedicated Management Group for subscriptions that will solely be used for testing and exploration by an organization’s application teams. These subscriptions will be securely disconnected from the Corp and Online landing zones.
* Decommissioned: This is the dedicated Management Group for landing zones that are being cancelled, which then will be moved to this Management Group before deleted by Azure after 30-60 days. -->

## Hvordan dirftsette og arbeide med denne referanseimplementeringen

Nordlys landingssoner tilbyr en enhetlig opplevelse for å driftsette og arbeide med de ulike referanseimplementeringene. For å distribuere Nordlys arkitekturen, klikk på "Deployer til Azure"-knappen øverst på denne siden og sørg for å velge følgende alternativer:

- I **Nordlys kjerneoppsett**-bladet velger du alternativet for **Dedikerte (anbefalte)** abonnementer for plattformressurser.
- I **Nettverkstopologi og tilkobling**-bladet, under **Deployer nettverkstopologi**, velger du **Nei**.

De resterende alternativene på de forskjellige bladene vil avhenge av miljøet ditt og de ønskede distribusjonsinnstillingene. For detaljerte instruksjoner for hver av distribusjonstrinnene, se [Brukerhåndboken for Nordlys Landingssoner](https://github.com/Azure/Enterprise-Scale/wiki).

## Hva vil bli distribuert?

Som standard er alle anbefalinger aktivert, og du må eksplisitt deaktivere dem hvis du ikke ønsker at de skal bli distribuert og konfigurert.

- En skalerbar Management Group-hierarki som er tilpasset kjerneplattformfunksjonaliteter, slik at du kan operasjonalisere i stor skala ved hjelp av sentralt administrert Azure RB