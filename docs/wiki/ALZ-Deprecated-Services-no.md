# Azure Landing Zones Utdaterte retningslinjer og tjenester

## I denne seksjonen

- [Utdaterte retningslinjer](#utdaterte-retningslinjer)
- [Utdaterte tjenester](#utdaterte-tjenester)

## Oversikt

Ettersom Microsoft videreutvikler retningslinjer og tjenester, kan en eller flere komponenter i Azure Landing Zone (ALZ) bli erstattet og utdatert.

## Utdaterte retningslinjer

Nye Azure-retningslinjer utvikles og opprettes av produktgrupper som støtter deres tjenester, og de er vanligvis av typen "innebygd" (`built-in`). Disse nye retningslinjene erstatter ofte utdaterte retningslinjer og gir vanligvis veiledning om hvilken retningslinje som skal brukes i stedet. Azure Landing Zones (ALZ) retningslinjer er ikke unntatt fra dette, og over tid vil noen retningslinjer bli oppdatert for å dra nytte av nye "innebygde" retningslinjer i stedet for ALZs "tilpassede" retningslinjer. I løpet av denne prosessen vil "tilpassede" ALZ-retningslinjer bli utdatert når nye "innebygde" retningslinjer er tilgjengelige og gir samme funksjonalitet. Dette reduserer til slutt vedlikeholdsarbeidet for "tilpassede" retningslinjer.

Utdaterte retningslinjer:

| Utdatert ALZ-retningslinje                   | Erstattet av "innebygd" retningslinje<br>(inkluderer lenke til AzAdvertizer)                                                                                               | Begrunnelse                                                            |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Distribuerer NSG flytlogger og trafikkanalyse<br>ID: `Deploy-Nsg-FlowLogs`                  | [`e920df7f-9a64-4066-9b58-52684c02a091`](https://www.azadvertizer.net/azpolicyadvertizer/e920df7f-9a64-4066-9b58-52684c02a091-no.html) | Tilpasset retningslinje erstattet av "innebygd" retningslinje, krever mindre administrativt arbeid |
| Distribuerer NSG flytlogger og trafikkanalyse til Log Analytics<br>ID: `Deploy-Nsg-FlowLogs-to-LA`            | [`e920df7f-9a64-4066-9b58-52684c02a091`](https://www.azadvertizer.net/azpolicyadvertizer/e920df7f-9a64-4066-9b58-52684c02a091-no.html) | Tilpasset retningslinje erstattet av "innebygd" retningslinje, krever mindre administrativt arbeid |
| Forbud mot opprettelse av offentlig IP<br>ID: `Deny-PublicIP`                      | [`6c112d4e-5bc7-47ae-a041-ea2d9dccd749`](https://www.azadvertizer.net/azpolicyadvertizer/6c112d4e-

5bc7-47ae-a041-ea2d9dccd749-no.html) | Tilpasset retningslinje erstattet av "innebygd" retningslinje, krever mindre administrativt arbeid |
| Siste versjon av TLS skal brukes i API Appen<br>ID: `8cb6aa8b-9e41-4f4e-aa25-089a7ac2581e` | [`f0e6e85b-9b9f-4a4b-b67b-f730d42f1b0b`](https://www.azadvertizer.net/azpolicyadvertizer/f0e6e85b-9b9f-4a4b-b67b-f730d42f1b0b-no.html)  | Utdatert retningslinje i initiativet fjernet, da eksisterende retningslinje erstatter den |
| SQL-servere skal bruke kundestyrt nøkler for kryptering av data i hviletilstand<br>ID: `0d134df8-db83-46fb-ad72-fe0c9428c8dd` | [`0a370ff3-6cab-4e85-8995-295fd854c5b8`](https://www.azadvertizer.net/azpolicyadvertizer/0a370ff3-6cab-4e85-8995-295fd854c5b8-no.html)  | Utdatert retningslinje i initiativet erstattet med ny retningslinje                  |
| RDP-tilgang fra internett skal blokkeres<br>ID: `Deny-RDP-From-Internet` | [`Deny-MgmtPorts-From-Internet`](https://www.azadvertizer.net/azpolicyadvertizer/Deny-MgmtPorts-From-Internet-no.html)  | Utdatert retningslinje, da den erstattes av en mer fleksibel retningslinje                  |
| Distribuerer SQL Database Transparent Data Encryption<br>ID: [`Deploy SQL Database Transparent Data Encryption`](https://www.azadvertizer.net/azpolicyadvertizer/Deploy-Sql-Tde-no.html) |	`86a912f6-9a06-4e26-b447-11b16ba8659f` | Tilpasset retningslinje erstattet av "innebygd" retningslinje, krever mindre administrativt arbeid |

### Mer informasjon

- [Azure Policy - Forhåndsvisning og utdaterte retningslinjer](https://github.com/Azure/azure-policy/blob/master/built-in-policies/README.md#preview-and-deprecated-policies) - for å lære mer om utdatingsprosessen.
- [Migrer ALZ-retningslinjer til innebygde](https://github.com/Azure/Enterprise-Scale/wiki/Migrate-ALZ-Policies-to-Built%E2%80%90in) - for veiledning om hvordan du migrerer utdaterte ALZ-tilpassede retningslinjer til Azure innebygde retningslinjer.

## Utdaterte tjenester

- Fjernet alternativet for å distribuere "ActivityLog" løsningen til Log Analytics Workspace, da den er erstattet av Activity Log Insights Workbook, som er dokumentert [her](https://learn.microsoft.com/azure/azure-monitor/essentials/activity-log-insights).
- Fj

ernet alternativet for å distribuere "Service Map" løsningen, da den er erstattet av VM Insights, som er dokumentert [her](https://learn.microsoft.com/azure/azure-monitor/essentials/activity-log-insights). Veiledning om migrering og fjerning av "Service Map" løsningen finner du [her](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-migrate-from-service-map).