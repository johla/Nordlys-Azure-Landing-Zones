## Fil -> Nytt -> Region

Bedrifter ønsker å dra nytte av nye Azure-regioner og distribuere tjenester nærmere brukeren. De vil legge til nye Azure-regioner etter hvert som etterspørselen øker. Som en del av Nordlys arkitekturen med prinsippet om policybasert styring, vil de tildele retningslinjer i miljøet sitt med et antall regioner de ønsker å bruke, og retningslinjer vil sikre at Azure-miljøet deres er riktig konfigurert:

### Styring

Alle referansekunder har bestemt seg for å bruke et enkelt Log Analytics-arbeidsområde. Når den første regionen aktiveres, vil de distribuere Log Analytics-arbeidsområdet i administrasjonssubskripsjonen sin. Ingen handling vil være nødvendig når påfølgende Azure-regioner aktiveres, da Azure Policy vil sikre at all plattformlogging blir rutet til arbeidsområdet. Azure Policy brukes omfattende for ulike styringsoperasjoner. Se [Hvordan hjelper Azure-retningslinjer i Nordlys-landingsområdet?](./azpol-no.md) for å lære mer om ulike styrings- og distribusjonsoperasjoner som er aktivert i Nordlys-landingsområdet via Azure Policy.

### Nettverk

Her bruker kundene ulike arkitekturdesign. Følgende eksempler er for Contoso-referanseimplementeringen:

En policy vil kontinuerlig sjekke om en Virtual WAN VHub allerede eksisterer i "Connectivity"-subskripsjonen for alle aktive regioner, og opprette en hvis den ikke gjør det. Konfigurer Virtual WAN VHub for å sikre internett-trafikk fra sikrede tilkoblinger (spoke VNets inne i landingsområdet) til internett via Azure Firewall.

For alle Azure Virtual WAN VHubs vil retningslinjer sørge for at Azure Firewall blir distribuert og koblet til den eksisterende globale Azure Firewall-retningslinjen, samt opprettelsen av en regional Firewall-retningslinje, hvis nødvendig.

En Azure Policy vil også distribuere standard NSGer (nettverkssikkerhetsgrupper) og UDRer (User Defined Routes) i landingsområder, og mens NSG vil bli koblet til alle undernett, vil UDR kun bli koblet til VNet-injiserte PaaS-tjenesters undernett. Azure Policy vil sikre at riktige NSG- og UDR-regler er konfigurert for å tillate kontrolltrafikk for VNet-injiserte tjenester til å fortsette å fungere, men kun for de Azure PaaS-tjenestene som er godkjent i henhold til [Service Enablement Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/security-governance-and-compliance#whitelist-the-service-framework) beskrevet i dette dokumentet. Dette er nødvendig fordi når landingsområdets VNeter blir koblet til Virtual WAN VHub, vil de få standardruten (0.0.0.0/0) konfigurert for å pe