| ARM-mal | Skaler uten omskriving |
|:--------------|:--------------|
| [![Deployer til Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2FeslzArm.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2Feslz-portal.json)  | Ja |

# Driftsett Nordlys arkitekturen med hub- og spoke-arkitektur

Nordlys arkitekturen er modulær i sin design og tillater organisasjoner å starte med grunnleggende landingssoner som støtter deres applikasjonsporteføljer og legge til hybrid tilkobling med ExpressRoute eller VPN ved behov. Alternativt kan organisasjoner starte med en Nordlys arkitektur basert på den tradisjonelle hub- og spoke-nettverkstopologien hvis kundene krever hybrid tilkobling til lokasjoner lokalt fra begynnelsen av.

En hub- og spoke-nettverkstopologi lar deg opprette en sentral Hub-VNet som inneholder delte nettverkskomponenter (som Azure Firewall, ExpressRoute og VPN-portaler) som deretter kan brukes av spoke-VNet-er, som er koblet til Hub-VNet via VNET-peering, for å sentralisere tilkoblingen i miljøet ditt. Gateway-transit i VNet-peering lar spokes ha tilkobling til/fra lokasjoner lokalt via ExpressRoute eller VPN, og i tillegg kan [transitiv tilkobling](https://azure.microsoft.com/blog/create-a-transit-vnet-using-vnet-peering/) mellom spokes implementeres ved å distribuere brukerdefinerte ruter (UDR) på spokes og bruke Azure Firewall eller en NVA (nettverksvirtuell apparat) i hub-en som transittressurs. Designoverveielser og anbefalinger for hub- og spoke-nettverkstopologi finner du [her](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/traditional-azure-networking-topology).

![Hub- og Spoke-nettverkstopologi](./media/hub-and-spoke-topology.png)

*En hub- og spoke-nettverkstopologi*

Denne referanseimplementeringen tillater også driftsetting av plattformtjenester på tvers av tilgjengelighetssoner (som Azure Firewall, VPN- eller ExpressRoute-portaler) for å øke tilgjengeligheten og oppetiden til slike tjenester.

## Kundeprofil

Denne referanseimplementeringen er ideell for kunder som har startet sin Nordlys arkitekturreise med en implementering av grunnleggende landingssoner og som deretter har behov for å legge til tilkobling til lokasjoner på stedet og avdelingskontorer ved å bruke en tradisjonell hub- og spoke-nettverksarkitektur. Denne

 referanseimplementeringen passer også godt for kunder som ønsker å starte med landingssoner for sin nye utrulling/utvikling i Azure ved å implementere en nettverksarkitektur basert på den tradisjonelle hub- og spoke-nettverkstopologien.

Vennligst se [Brukerveiledningen for Nordlys Landingssoner](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale) for detaljert informasjon om forutsetninger og trinn for driftsetting.

## Hvordan utvikle fra en grunnleggende Nordlys-arkitektur

Hvis kunden startet med en grunnleggende Nordlys-arkitekturdrift og hvis forretningsbehovene endres over tid, for eksempel migrering av applikasjoner fra lokale systemer til Azure som krever hybrid tilkobling, kan du enkelt opprette **Tilkobling**-abonnementet, plassere det i **Platform > Tilkobling**-håndteringsgruppen og tildele Azure Policy for hub- og spoke-nettverkstopologien.

## Forutsetninger

For å kunne driftsette denne ARM-malen, må din bruker/servicekonto ha eierrettigheter på rotnivået i leietakeren.
Se følgende [instruksjoner](../../EnterpriseScale-Setup-azure-no.md) for hvordan du gir tilgang.

### Valgfrie forutsetninger

Driftsopplevelsen i Azure Portal lar deg ta i bruk eksisterende (helst tomme) abonnementer dedikert til plattformstyring, tilkobling og identitet. Det lar deg også ta i bruk eksisterende abonnementer som kan brukes som initielle landingssoner for applikasjonene dine.

Hvis du ønsker å lære hvordan du oppretter nye abonnementer programmatiske, kan du besøke denne [lenken](https://learn.microsoft.com/azure/cost-management-billing/manage/programmatically-create-subscription).

Hvis du ønsker å lære hvordan du oppretter nye abonnementer ved hjelp av Azure Portal, kan du besøke denne [lenken](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/).

## Hvordan driftsette denne referanseimplementeringen

Nordlys Landingssoner tilbyr en enkelt opplevelse for å driftsette de ulike referanseimplementeringene. For å driftsette Nordlys med hub- og spoke-arkitektur, klikk på "Driftsett til Azure"-knappen øverst på denne siden og sørg for å velge følgende alternativer:

- I **Kjerneoppsett for Nordlys**-bladet, velg alternativet for **Dedikerte (anbefalt)** abonnementer for plattformressurser.
- I **Nettverkstopologi og tilkobling**-bladet, velg enten **Hub og spoke med Azure Firewall** eller **Hub og spoke med ditt eget tredjeparts NVA (nettverksvirtuelt apparat)** nettverkstopologi-alternativer.

Resten av alternativene på de ulike bladene vil avhenge av miljøet ditt og ønskede innstillinger for driftsetting. For detaljerte instru

ksjoner for hver av driftssettingstrinnene, se [Brukerveiledningen for Nordlys Landingssoner](https://github.com/Azure/Enterprise-Scale/wiki).

## Hva vil bli driftsatt?

Som standard er alle anbefalinger aktivert, og du må deaktivere dem eksplisitt hvis du ikke ønsker å driftsette og konfigurere dem.

- En skalerbar hierarki av håndteringsgrupper som er tilpasset kjerneplattformfunksjonalitet, slik at du kan operasjonalisere i stor skala ved bruk av sentralt administrert Azure RBAC (rollebasert tilgangskontroll) og Azure Policy der plattformen og arbeidsbelastningene har tydelig adskillelse.
- Azure Policy-er som muliggjør autonomi for plattformen og landingssonene.
- Et Azure-abonnement dedikert for **styring**, som muliggjør kjerneplattformfunksjonalitet i stor skala ved bruk av Azure Policy, for eksempel:
  - En Log Analytics-arbeidsområde og en Automation-konto
  - Overvåking av Azure Security Center
  - Azure Security Center (Standard- eller Gratisnivå)
  - Azure Sentinel
  - Diagnostikkinnstillinger for aktivitetslogger, virtuelle maskiner og PaaS-ressurser som sendes til Log Analytics
- Et Azure-abonnement dedikert for **tilkobling**, som driftsetter kjerne-Azure-nettverksressurser som:
  - Et hub-virtuelt nettverk
  - Azure Firewall (valgfritt - driftsetting på tvers av tilgjengelighetssoner)
  - ExpressRoute Gateway (valgfritt - driftsetting på tvers av tilgjengelighetssoner)
  - VPN Gateway (valgfritt - driftsetting på tvers av tilgjengelighetssoner)
  - Azure Private DNS-soner for privat kobling (valgfritt)
  - Azure DDoS Network Protection (valgfritt)
- (Valgfritt) Et Azure-abonnement dedikert for **identitet** i tilfelle organisasjonen din krever Active Directory-domenekontrollere i et dedikert abonnement.
  - Et virtuelt nettverk vil bli driftsatt og koblet til hub-virtual network via VNet-peering.
- Håndteringsgruppe for landingssone for **bedrifts-tilkoblede** applikasjoner som krever tilkobling til steder på stedet, til andre landingssoner eller til internett via delte tjenester som tilbys i hub-virtual network.
  - Dette er hvor du vil opprette abonnementene som vil huse bedrifts-tilkoblede arbeidsbelastninger.
- Håndteringsgruppe for landingssone for **nettbaserte** applikasjoner som vil være internett-vendte, hvor et virtuelt nettverk er valgfritt og hybrid-tilkobling ikke er nødvendig.
  - Dette er hvor du vil opprette abonnementene som vil huse nettbaserte arbeidsbelastninger.
- Landingsson-abonnementer for Azure-native, nettbaserte **nettbaserte** applikasjoner og ressurser.
- Landingsson-abonnementer for **bedrifts-tilkoblede

** applikasjoner og ressurser, inkludert et virtuelt nettverk som vil bli koblet til hub-en via VNet-peering.
- Azure Policy-er for nettbaserte og bedrifts-tilkoblede landingssoner, som inkluderer:
  - Tving overvåking av virtuelle maskiner (Windows og Linux)
  - Tving overvåking av virtuelle maskinskaleringssett (Windows og Linux)
  - Tving overvåking av Azure Arc virtuelle maskiner (Windows og Linux)
  - Tving sikkerhetskopiering av virtuelle maskiner (Windows og Linux)
  - Tving sikker tilgang (HTTPS) til lagringskontoer
  - Tving revisjon for Azure SQL
  - Tving kryptering for Azure SQL
  - Forhindre IP-videresending
  - Forhindre innkommende RDP fra internett
  - Sørg for at undernett er tilknyttet NSG
  - Knytt private sluttpunkter til Azure Private DNS-soner for Azure PaaS-tjenester.

![Nordlys med tilkobling](./media/es-hubspoke.png)

> For et detaljert diagram over nettverkstopologien for denne referanseimplementeringen, klikk [her](../../wiki/media/es-hubspoke-nw.png). Dette er også tilgjengelig i Visio-format fra [her](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/enterprise-scale-architecture.vsdx).

## Neste trinn

### Fra et applikasjonsperspektiv

Når du har driftsatt referanseimplementeringen, kan du opprette nye abonnementer eller flytte eksisterende abonnementer til **Landingssoner** > **Nettbasert** eller **Bedrifts-tilkoblet**-håndteringsgruppen, og tilordne deretter RBAC til gruppene/brukerne som skal bruke landingssonene (abonnementene) slik at de kan begynne å driftsette arbeidsbelastningene sine.

Se [Opprett Landingssone(r)](../../EnterpriseScale-Deploy-landing-zones-no.md)-artikkelen for veiledning om å opprette Landingssoner.