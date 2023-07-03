|# ARM-mal | Skaler uten omskriving |
|:--------------|:--------------|
|[![Deployer til Azure](https://learn.microsoft.com/azure/templates/media/deploy-to-azure.svg)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2FeslzArm.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2FeslzArm%2Feslz-portal.json) | Ja |

# Driftsett Nordlys arkitekturen med Azure VWAN

Nordlys arkitekturen er modulær og gir organisasjoner mulighet til å starte med grunnleggende landingssoner som støtter deres applikasjonsportefølje, og deretter legge til hybridtilkobling med ExpressRoute eller VPN når det er nødvendig. Alternativt kan organisasjoner starte med en Nordlys arkitektur basert på en Azure Virtual WAN nettverkstopologi hvis kundene trenger hybridtilkobling til lokasjoner lokalt og global transittkobling fra begynnelsen av.

Azure Virtual WAN er en administrert løsning fra Microsoft som gir ende-til-ende, global og dynamisk transittkobling som standard. Virtual WAN-naver eliminerer behovet for manuell konfigurasjon av nettverkstilkobling. For eksempel trenger du ikke å administrere brukerdefinerte ruter (UDR) eller nettverksbaserte virtuelle apparater (NVAs) for å muliggjøre global transittkobling. Virtual WAN forenkler ende-til-ende nettverkstilkobling i Azure og til Azure fra lokalt miljø ved å opprette en hub-og-spoke nettverksarkitektur. Arkitekturen kan enkelt skaleres for å støtte flere Azure-regioner og lokasjoner lokalt (enhver-til-en-kobling), som vist i følgende figur:

![Azure Virtual WAN nettverkstopologi](./media/global-transit.png)


## Kundeprofil

Denne referanseimplementeringen er ideell for kunder som har startet sin Nordlys arkitektur-reise med en grunnleggende implementering av Nordlys arkitekturen og deretter har behov for å legge til tilkobling til lokasjoner lokalt og avdelingskontorer ved hjelp av Azure VWAN, ExpressRoute og VPN. Denne referanseimplementeringen passer også godt for kunder som ønsker å starte med landingssoner for ny implementering/utvikling i Azure, der et globalt transitt-nettverk er nødvendig, inkludert hybridtilkobling til lokasjoner lokalt og avdelingskontorer via ExpressRoute og VPN.

Vennligst se [Brukerhåndbok for Nordlys arkitekturens landingssoner](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale) for detaljert informasjon om forutsetninger og distribusjonstrinn.

## Hvordan utvikle fra Nordlys arkitektur

grunnlaget

Hvis kunden startet med en grunnleggende implementering av Nordlys arkitekturen, og hvis forretningskravene endrer seg over tid, for eksempel migrering av lokalt kjørte applikasjoner til Azure som krever hybridtilkobling, kan du enkelt opprette abonnementet **Tilkobling** og plassere det i **Plattform > Tilkobling**-håndteringsgruppen og tilordne Azure Policy for VWAN nettverkstopologi.

## Forutsetninger

For å distribuere denne ARM-malen må brukeren/serviceprinsippet ha eierrettigheter på leietakerrotet.
Se følgende [instruksjoner](../../EnterpriseScale-Setup-azure.md) for å se hvordan du gir tilgang.

### Valgfrie forutsetninger

Distribusjonsopplevelsen i Azure Portal lar deg ta i bruk eksisterende (helst tomme) abonnementer dedikert for plattformadministrasjon, tilkobling og identitet. Den lar deg også ta i bruk eksisterende abonnementer som kan brukes som initielle landingssoner for applikasjonene dine.

For å lære hvordan du oppretter nye abonnementer programmatiske, vennligst besøk denne [lenken](https://learn.microsoft.com/azure/cost-management-billing/manage/programmatically-create-subscription).

For å lære hvordan du oppretter nye abonnementer ved hjelp av Azure Portal, vennligst besøk denne [lenken](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/).

## Hvordan distribuere denne referanseimplementeringen

Nordlys arkitekturens landingssoner tilbyr en enhetlig opplevelse for å distribuere de forskjellige referanseimplementeringene. For å distribuere Nordlys arkitekturen med Azure VWAN, klikk på **Deployer til Azure**-knappen øverst på denne siden og sørg for at du velger følgende alternativer:

- I **Oppsett for Nordlys arkitekturgrunnlaget**-bladet, velg alternativet for **Dedikerte (anbefalt)** abonnementer for plattformressurser.
- I **Nettverkstopologi og tilkobling**-bladet, velg alternativet **Virtual WAN (administrert av Microsoft)** for nettverkstopologi.

Resten av alternativene på de forskjellige bladene vil avhenge av miljøet ditt og ønskede distribusjonsinnstillinger. For detaljerte instruksjoner for hver av distribusjonstrinnene, se [Brukerhåndboken for Nordlys arkitekturens landingssoner](https://github.com/Azure/Enterprise-Scale/wiki).

## Hva vil bli driftsatt?

- En skalerbar hierarki av håndteringsgrupper som er justert med kjerneplattformfunksjoner, slik at du kan operasjonalisere i stor skala ved bruk av sentralt administrert Azure RBAC og Azure Policy, der plattformen og arbeidsbelastningene har tydelig separasjon.
- Azure Policyer som gir autonomi for plattformen og landingssonene.
- Et Azure

-abonnement dedikert til **administrasjon**, som muliggjør kjerneplattformfunksjoner i stor skala ved bruk av Azure Policy, for eksempel:
  - En Log Analytics-arbeidsområde og en Automation-konto
  - Overvåking av Azure Security Center
  - Azure Security Center (Standard eller Free-tier)
  - Azure Sentinel
  - Diagnostiske innstillinger for aktivitetslogger, virtuelle maskiner og PaaS-ressurser sendt til Log Analytics
- Et Azure-abonnement dedikert til **tilkobling**, som distribuerer kjerne-Azure-nettverksressurser, for eksempel:
  - Azure VWAN
  - VWAN-hub
  - ExpressRoute Gateway (valgfritt)
  - VPN Gateway (valgfritt)
  - Azure Firewall (valgfritt)
  - Brannmurpolicyer (valgfritt)
  - Azure DDoS-nettverksbeskyttelse (valgfritt)
- Et Azure-abonnement dedikert til **identitet**, der kunder kan distribuere Active Directory-domenekontrollere som kreves for deres miljø.
  - Et virtuelt nettverk vil bli driftsatt og koblet til hub VNet via VNet-peering.
- Håndteringsgruppe for landingssone for **bedrifts**-tilknyttede applikasjoner som krever tilkobling til lokasjoner lokalt, til andre landingssoner eller til internett via delte tjenester som tilbys i VWAN-hub.
  - Dette er hvor du oppretter abonnementene dine som vil huse bedrifts-tilknyttede arbeidsbelastninger.
- Håndteringsgruppe for landingssone for **nettbaserte** applikasjoner som vil være internett-tilgjengelige, der et virtuelt nettverk er valgfritt og hybridtilkobling ikke er nødvendig.
  - Dette er hvor du oppretter abonnementene dine som vil huse nettbaserte arbeidsbelastninger.
- Landingssoneabonnementer for Azure-native, nettbaserte **nettbaserte** applikasjoner og ressurser.
- Landingssoneabonnementer for **bedrifts**-tilknyttede applikasjoner og ressurser, inkludert et virtuelt nettverk.
- Azure Policyer for nettbaserte og bedrifts-tilknyttede landingssoner, som inkluderer:
  - Pålegge overvåking av virtuelle maskiner (Windows og Linux)
  - Pålegge overvåking av VMSS (Windows og Linux)
  - Pålegge overvåking av Azure Arc VM (Windows og Linux)
  - Pålegge sikkerhetskopiering av virtuelle maskiner (Windows og Linux)
  - Pålegge sikker tilgang (HTTPS) til lagringskontoer
  - Pålegge revisjon for Azure SQL
  - Pålegge kryptering for Azure SQL
  - Forhindre IP-videresending
  - Forhindre innkommende RDP fra internett
  - Sørg for at subnettene er tilknyttet NSG

![Nordlys arkitektur uten tilkobling](./media/ns-vwan.png)

## Neste trinn

### Fra et

 applikasjonsperspektiv

Når du har driftsatt referanseimplementeringen, kan du opprette nye abonnementer eller flytte eksisterende abonnementer til **Landingssoner** > **Nettbaserte** eller **Bedrifts**-håndteringsgruppen, og til slutt tilordne RBAC til gruppene/brukerne som skal bruke landingssonene (abonnementene), slik at de kan begynne å distribuere arbeidsbelastningene sine.

Se [Opprette landingssone(r)](../../EnterpriseScale-Deploy-landing-zones.md)-artikkelen for veiledning om hvordan du oppretter landingssoner.