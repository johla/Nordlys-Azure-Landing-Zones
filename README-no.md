# Nordlys arkitekturen - Referanseimplementering

[![Gjennomsnittlig tid for å løse et problem](http://isitmaintained.com/badge/resolution/azure/enterprise-scale.svg)](http://isitmaintained.com/project/azure/enterprise-scale "Gjennomsnittlig tid for å løse et problem")
[![Andel åpne problemer](http://isitmaintained.com/badge/open/azure/enterprise-scale.svg)](http://isitmaintained.com/project/azure/enterprise-scale "Andel åpne problemer")

## Brukerdokumentasjon

For å få mer informasjon om Azure landingssone-referanseimplementeringen, kan du se [dokumentasjonen på vår Wiki](https://github.com/Azure/Enterprise-Scale/wiki).

---

## Mål

Nordlys arkitekturen for Azure landingsoner gir forhåndsdefinert veiledning kombinert med Azure beste praksis, og den følger designprinsipper på tvers av viktige designområder, slik at organisasjoner kan definere sin Azure-arkitektur. Den vil fortsette å utvikle seg i takt med Azure-plattformen og blir til slutt definert av de ulike designavgjørelsene organisasjoner må ta for å definere sin Azure-reise.

Nordlys arkitekturen for Azure landingsoner er modulær i designet og lar organisasjoner starte med grunnleggende landingssoner som støtter deres applikasjonsportefølje. Arkitekturen gjør det mulig for organisasjoner å starte så lite som nødvendig og skaleres i takt med deres forretningsbehov, uavhengig av skaleringspunkt.

![Animert bilde som viser modulariteten til Azure landingssoner](./docs/wiki/media/ESLZ.gif)

---

_Nordlys arkitekturen representerer den strategiske designveien og målrettet teknisk tilstand for Azure-miljøet ditt._

---

Ikke alle virksomheter tar i bruk Azure på samme måte, så Nordlys arkitekturen kan variere mellom kunder. De tekniske vurderingene og designanbefalingene i Nordlys arkitekturen kan til slutt føre til forskjellige avveininger basert på kundens scenario. Variasjon er forventet, men hvis de grunnleggende anbefalingene følges, vil den resulterende målarkitekturen legge kunden på vei mot bærekraftig skala.

Referanseimplementeringene av Nordlys arkitekturen i dette repository er ment å støtte Azure-adoptering på bedriftsnivå og gir forhåndsdefinert veiledning basert på autoritativt design for hele Azure-plattformen.

| Viktig kundekrav for landingssone | Referanseimplementeringer for Nordlys arkitekturen |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tidsplan for å oppfylle sikkerhets- og overholdelseskrav for en arbeidsbelastning | Å aktivere alle anbefalinger under oppsettet vil sikre at ressursene er i samsvar med overvåkings- og sikkerhetsperspektivet |
| Gir en grunnleggende arkitektur ved hjelp av flersubskripsjonsdesign | Ja, for hele Azure-mandanten uavhengig av kundens skaleringspunkt |
| Beste praksis fra skytilbyderen | Ja, bevist og validert med kunder |
| Er i tråd med skytilbyderens plattformplan | Ja |
| Brukervennlig opplevelse og forenklet oppsett | Ja, Azure Portal |
| Alle viktige tjenester er til stede og konfigurert i samsvar med anbefalte beste praksiser for identitets- og tilgangsstyring, styring, sikkerhet, nettverk og logging | Ja, ved bruk av flersubskripsjonsdesign i tråd med Azure-plattformplanen |
| Automatiseringsfunksjoner (IaC/DevOps) | Ja: ARM, Policy, Bicep og Terraform-moduler |
| Gir langsiktig selvforsyningsevne | Ja, Nordlys arkitekturen -> 1:N landingssoner. Tilnærmingen og arkitekturen forbereder kunden på langsiktig selvforsyning, og referanseimplementeringene er der for å hjelpe deg i gang |
| Muliggjør migrasjonshastighet på tvers av organisasjonen | Ja, Nordlys arkitekturen -> 1:N landingssoner. Arkitekturen inkluderer design for segmentering og ansvarsseparasjon for å gi teamene muligheten til å handle innenfor passende landingssoner |
| Oppnår driftsmessig utmerkelse | Ja. Gir autonomi for plattform- og applikasjonsteamene med en policydrevet styring og ledelse |

## Suksesskriterier

For å utnytte denne referanseimplementasjonen fullt ut, må leserne samarbeide tett med sentrale interessenter hos kunden innen viktige tekniske områder, som identitet, sikkerhet og nettverk. Suksessen med å ta i bruk skyen avhenger i stor grad av tverrfaglig samarbeid innen organisasjonen, ettersom nødvendige designavgjørelser på bedriftsnivå krysser flere områder og må involvere eksperter (Subject Matter Expertise) og interessenter innenfor kundens domene. Det er avgjørende at organisasjonen har definert sin [Nordlys arkitektur](./docs/EnterpriseScale-Architecture-no.md) i tråd med designprinsippene og de viktige designområdene.

Det antas også at leserne har en bred forståelse av nøkkelbegreper og tjenester i Azure for å fullt ut kontekstualisere de anbefalte retningslinjene som er inkludert i Nordlys arkitekturen.
<!--
![Nordlys arkitektur](./docs/wiki/media/ES-process.png)
-->

## Driftsetting av Nordlys arkitekturen i ditt eget miljø

Nordlys arkitekturen er modulær i designet og gjør det mulig for kunder å starte med grunnleggende landingssoner som støtter deres applikasjonsporteføl
