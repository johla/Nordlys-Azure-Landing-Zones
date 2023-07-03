# Retningslinjer inkludert i referanseimplementasjonene for Azure landingssoner

Azure Policy og deployIfNotExist muliggjør autonomi i plattformen og reduserer operasjonell byrde når du skalerer implementeringene og abonnementene dine i Azure landingssone-arkitekturen. Hovedformålet er å sikre at abonnementer og ressurser er i samsvar, samtidig som det gir applikasjonsteam muligheten til å bruke sine foretrukne verktøy/klienter for distribusjon.

> For mer informasjon, se [Policydrevet styring](https://learn.microsoft.com/nb-no/azure/cloud-adoption-framework/ready/landing-zone/design-principles#policy-driven-governance).

## Ofte stilte spørsmål og tips

Vi har lagt til en dedikert [FAQ og tips for ALZ-policyer](./ALZ-Policies-FAQ-no) basert på vanlige problemer eller spørsmål som kundene og partnerne våre stiller.

## Hvorfor er tilpassede retningslinjer en del av Azure landingssoner?

Vi samarbeider med kundene og partnerne våre for å sikre at vi utvikler og forbedrer referanseimplementasjonene for å møte kundenes behov. Den primære tilnærmingen for retningslinjene som en del av Azure landingssoner er å være proaktiv (deployIfNotExist og modify) og forebyggende (deny). Vi flytter kontinuerlig disse retningslinjene til innebygde policyer.

## Hvilke Azure-retningslinjer tilbyr Azure landingssone i tillegg til de som allerede er innebygd?

Det er omtrent 114 tilpassede Azure Policy-definisjoner inkludert og rundt 12 tilpassede Azure Policy-initiativ inkludert som en del av implementasjonen av Azure landingssoner. Disse legger seg til de som allerede er innebygd i hver kundes Azure-tenant.

Alle tilpassede Azure Policy-definisjoner og -initiativer er de samme for alle de tre implementeringsalternativene for Azure landingssoner: [Terraform-modul](https://aka.ms/alz/tf), [Bicep-moduler](https://aka.ms/alz/bicep) og [Azure landingssoneportalakselerator](https://aka.ms/alz#azure-landing-zone-accelerator).

Dette skyldes at kilden til sannheten er [`Enterprise-Scale`-repoet](https://github.com/Azure/Enterprise-Scale), som både Terraform- og Bicep-implementeringsalternativene trekker fra for å bygge sine respektive `lib`-mapper.

For en komplett liste over alle tilpassede og innebygde policyer som er distribuert i en implementering av Azure landingssone, kan du se følgende [del](https://github.com/Azure/Enterprise-Scale/wiki/ALZ-Policies#what-policy-definitions-are-assigned-within-the-azure-landing-zones-custom--built-in).

> Målet vårt er alltid å prøve å bruke innebygde policyer der de er tilgjengelige og samarbeide med produkteamene for å adoptere våre tilpassede policyer og gjøre dem inneby

gde, noe som tar tid. Dette betyr at det alltid vil være behov for tilpassede policyer.

## Hvorfor blir administrerte identiteter distribuert som en del av ALZ-policyene?

Administrerte identiteter gir en alternativ måte å få tilgang til Azure-ressurser uten å måtte administrere legitimasjon. De opprettes som en del av ALZ-policyene hovedsakelig for policyer som har deployIfNotExists (DINE)-effekten i dette initiativet. De administrerte identitetene brukes til å rette opp ressurser som ikke er i samsvar med policyen. For mer informasjon om hvordan retting fungerer med tilgangskontroll, kan du se følgende dokumentasjon: [Retting av ressurser som ikke er i samsvar - Azure Policy | Microsoft](https://learn.microsoft.com/nb-no/azure/governance/policy/how-to/remediate-resources?tabs=azure-portal#how-remediation-access-control-works)

## AzAdvertizer-integrasjon

Vi har samarbeidet med skaperen av [AzAdvertizer](https://www.azadvertizer.net) for å integrere alle tilpassede Azure Policy-definisjoner og -initiativ som en del av Azure landingssoner. Dette gjør det enklere for kundene å bruke verktøyet til å se nærmere på policyene på en brukervennlig måte som er populær i fellesskapet.

På enten [Policy](https://www.azadvertizer.net/azpolicyadvertizer_all.html#%7B%22col_10%22%3A%7B%22flt%22%3A%22ESLZ%22%7D%7D)- eller [Initiative](https://www.azadvertizer.net/azpolicyinitiativesadvertizer_all.html)-delen av nettstedet, velger du nedtrekksmenyen for 'Type'-kolonnen (den siste til høyre) til 'ALZ', og du vil se alle policyene som nevnt ovenfor i verktøyet, slik at du kan undersøke nærmere.

AzAdvertizer oppdateres også en gang per dag!

## Hvilke retningslinjer for policydefinisjoner er tildelt innenfor Azure Landing Zones (tilpassede og innebygde)?

Som en del av en standard utrullingskonfigurasjon, blir policydefinisjoner og policysettdefinisjoner distribuert på flere nivåer innenfor Azure Landing Zone Management Group-hierarkiet, slik det er vist i diagrammet nedenfor.

![image](./media/MgmtGroups_Policies_v0.1-no.svg)

De følgende delene vil gi en oppsummering av policysett og policysettdefinisjoner som blir brukt på hvert nivå i Management Group-hierarkiet.

> **MERK**: Selv om de følgende seksjonene vil definere hvilke policydefinisjoner/-sett som blir brukt på spesifikke områder, må du huske at policyen arves innenfor ditt Management Group-hierarki.

> <a href=./media/ALZ%20Policy%20Assignments%20v2.xlsx><img src=./media/ef73.jpg width=64 height=64 align=center></a> For enkelhets skyld er det tilgjengelig en Excel-versjon av informasjonen nedenfor [her](./media/ALZ%20Policy%20Assignments%20v2.xlsx) eller klikk på ikonet (sist oppdatert i april 2023).

### Mellomliggende rot

Denne administrasjonsgruppen er en overordnet gruppe for alle andre administrasjonsgrupper som er opprettet innenfor standard Azure Landing Zone-konfigurasjon. Policytildeling fokuserer hovedsakelig på tildeling av sikkerhets- og overvåkingsretningslinjer for å sikre samsvar og redusert driftsbyrde.

| Tildelingsnavn                                                 | Definisjonsnavn                                               | Policytype                         | Beskrivelse                                                                                                                                                                                            | Effekt(er)         |
| -------------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- |
| **Håndheve ALZ Sandbox-regler** | **Håndheve ALZ Sandbox-regler** | `Policysettdefinisjon`, **Tilpasset** | Dette initiativet vil bidra til å håndheve og styre abonnementer som er plassert innenfor Sandobx Management Group. Inkluderte retningslinjer: <ul><li>Forby vNET-peering på tvers av abonnementer<li>Forby distribusjon av vWAN/ER/VPN-gateways.</ul>                                                                              | Håndheve              |

### Versjonering

Hver policydefinisjon og -initiativ inneholder en versjon i metadataavsnittet:
```json
"metadata": {
   "version": "1.0.0",
   "category": "{categoryName}",
   "source": "https://github.com/Azure/Enterprise-Scale/",
   "alzCloudEnvironments": [
      "AzureCloud",
      "AzureChinaCloud",
      "AzureUSGovernment"
   ]
}
```
For å spore og gjennomgå versjonene av policyer og initiativer, kan du se [AzAdvertizer](https://www.azadvertizer.net/index.html).

Denne versjonen økes i samsvar med følgende regler (kan end

res):
   - **Hovedversjon** (**1**.0.0)
      - Policydefinisjoner
         - Endringer i regell ogikk
         - endringer i eksistensbetingelser for "ifNotExists"
         - Vesentlige endringer i policyens effekt (f.eks. legge til en ny ressurs i en distribusjon)
      - Policysettdefinisjoner
         - Tillegg eller fjerning av en policydefinisjon fra policysettet
   - **Delversjon** (1.**0**.0)
      - Policydefinisjoner
         - Endringer i detaljene om effekten som ikke oppfyller kriteriene for hovedversjonen
         - Legge til nye tillatte verdier for parametere
         - Legge til nye parametere (med standardverdier)
         - Andre mindre endringer i eksisterende parametere
      - Policysettdefinisjoner
         - Legge til nye tillatte verdier for parametere
         - Legge til nye parametere (med standardverdier)
         - Andre mindre endringer i eksisterende parametere
   - **Patchversjon** (1.0.**0**)
      - Policydefinisjoner
         - Endringer i strenger (visningsnavn, beskrivelse osv.)
         - Andre endringer i metadata
      - Policysettdefinisjoner
         - Endringer i strenger (visningsnavn, beskrivelse osv.)
         - Andre endringer i metadata
   - **Suffiks**
      - Legg til "-preview" på versjonen hvis policyen er i en forhåndsvisningstilstand  
         - Eksempel: 1.3.2-preview
      - Legg til "-deprecated" på versjonen hvis policyen er i en utdatert tilstand
         - Eksempel: 1.3.2-deprecated
 
## Forhåndsvisning og utdaterte policyer

Denne delen har som mål å forklare hva det betyr når en innebygd policy har statusen "forhåndsvisning" eller "utdatert".

Policyer kan være i forhåndsvisning fordi en egenskap (alias) som refereres til i policydefinisjonen, er i forhåndsvisning, eller fordi policyen er nylig introdusert og ønsker tilbakemeldinger fra kundene. En policy kan bli utdatert når egenskapen (alias) blir utdatert og ikke støttes i den nyeste API-versjonen for ressurstypen, eller når det er behov for manuell migrering fra kundene på grunn av en brytende endring i den nyeste API-versjonen for ressurstypen.

Når en policy blir utdatert eller går ut av forhåndsvisning, påvirker det ikke eksisterende tildelinger. Eksisterende tildelinger fortsetter å fungere som før. Policyen blir fortsatt evaluert og håndhevet som normalt og produserer fortsatt samsvarsresultater.

Her er endringene som skjer når en policy blir utdatert:
- Visningsnavnet får prefikset "[Utdatert]:" for å gi kundene bes

kjed om å migrere eller slette policyen.
- Beskrivelsen oppdateres for å gi ytterligere informasjon om utdatert status.
- Versjonsnummeret oppdateres med suffikset "-deprecated". (se [Policy Versioning](#versioning) ovenfor)

> **MERK:** Verdien for `name` må ikke endres i filen gjennom utdatert status eller forhåndsvisning.