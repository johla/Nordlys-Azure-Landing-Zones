# Konfigurasjon av retningsstyrt ruting i hub-og-spoke-nettverk

Policyen "Deployer en rute-tabell med spesifikke brukerdefinerte ruter" gjør det mulig å bruke en kunde-definert rutingkonfigurasjon til VNet-er som er inkludert i omfanget. For hver VNet som er inkludert, sjekker policyen om det finnes en rute-tabell som inneholder en samling av kunde-definerte UDR-er (User-Defined Routes), og den blir driftsatt dersom den ikke eksisterer. Rute-tabellen blir driftsatt i samme ressursgruppe som VNet-en som blir evaluert av policyen. Rute-tabellen som blir driftsatt av policyen må manuelt tilknyttes subnettene.

Det primære bruksområdet for policyen er automatisert rutingkonfigurasjon i Nordlys arkitekturen med hub-og-spoke-topologier (referansearkitekturen for Nordlys arkitekturen med hub-og-spoke er dokumentert [her](https://github.com/Azure/Enterprise-Scale/tree/main/docs/reference/adventureworks)). Ved å tilordne policyen til abonnementer i landingssonen som inneholder spoke VNet-er, blir det mulig å håndheve rutingsregler som for eksempel:

- Ruter all trafikk som forlater en spoke VNet til en brannmurklynge i hub-en.

- Tillater eller hindrer direkte internett-tilgang fra spoke VNet-er.

- Tillater direkte tilgang fra spoke VNet-er til delte tjenester i hub-en.

- Ruter all trafikk fra spoke VNet til delte tjenester i hub-en via brannmurklyngen i hub-en.

Policyen støtter følgende parametere:

- **effect**: En `String` som definerer virkningen av policyen. Tillatte verdier er `DeployIfNotExist` (standard) og `Disabled`.

- **requiredRoutes**: En `Array` av `String` objekter. Hvert `String` objekt definerer en brukerdefinert rute (UDR) i den egendefinerte rute-tabellen som blir driftsatt av policyen. Formatet er `"adresse-prefix;next-hop-type;next-hop-ip-adresse"`. Neste-hop IP-adresse må oppgis når neste hop-type er "VirtualAppliance". Tillatte verdier for feltet neste hop-type er dokumentert [her](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#next-hop-types-across-azure-tools). Dette er et eksempel på en *requiredRoutes*-array som definerer fire UDR-er:

```json
[
"0.0.0.0/0;VirtualAppliance;192.168.1.100", 
"192.168.1.4/32;VirtualAppliance;192.168.1.4",
"192.168.1.5/32;VirtualAppliance;192.168.1.5",
"192.168.2.0/24;VnetLocal"
]
```

- **vnetRegion**: En `String` som definerer regionen til `Microsoft.Network/virtualNetworks`-ressursene som blir evaluert av policyen.

 Kun VNeter i den angitte regionen blir evaluert av policyen. Denne parameteren gjør det mulig å tildele policyen flere ganger for å håndheve forskjellige rutingspolicyer i ulike regioner.
- **routeTableName**: En `String` som definerer navnet på den egendefinerte rute-tabellen som blir automatisk driftsatt av policyen (når en rute-tabell som inneholder alle *requiredRoutes* blir funnet).
- **disableBgpPropagation**: En `Boolean` som definerer verdien til egenskapen *disableBgpRoutePropagation* til den driftsatte rute-tabellen. Standardverdien er `false`.