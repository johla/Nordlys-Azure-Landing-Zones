# ALZ Policy FAQ og Tips

## Ofte stilte spørsmål om ALZ-policyer

Det skjer mye endringer for policyer i Azure, og dermed også for ALZ, og vi får en rekke vanlige spørsmål fra våre kunder og samarbeidspartnere. Denne siden er ment å adressere disse spørsmålene.

### Diagnostic Settings v2 (mai 2023)

Det er flere problemstillinger knyttet til Diagnostic Settings, og vi erkjenner at dette er et komplekst område som forårsaker mye frustrasjon.

For øyeblikket reviderer eierne av Azure-funksjoner/tjenester policyene sine for å være i samsvar med det nye diagnostic settings v2-skjemaet (som inkluderer loggekategorier som er et vanlig ønske). Det lanseres nye diagnoseinnstillingspolicyer for Azure-tjenester, med dedikerte policyer avhengig av den nødvendige loggmålretningen (Log Analytics, Event Hub eller lagringskontoer). Vi jobber sammen med produktgruppene for å sikre at policyene blir oppdatert så snart som mulig.

Sjekk tilbake her for oppdateringer, og sørg for å bokmerke [Hva er nytt](https://aka.ms/alz/whatsnew) for å se de siste oppdateringene om ALZ.

For å se den gjeldende listen over GitHub-problemer relatert til diagnoseinnstillinger, vennligst se [denne lenken](https://github.com/Azure/Enterprise-Scale/labels/Area:%20Diagnostic%20Settings).

### Azure Monitor Agent (mai 2023)

På samme måte som Microsoft Monitor Agent (MMA) er på en avskrivningssti, er Azure Monitor Agent (AMA) den anbefalte erstatningen, og det er en rekke forespørsler om å støtte AMA-spesifikke policyer. AMA er for øyeblikket i forhåndsvisning, og vi samarbeider med produktgruppen for å sikre at policyene blir oppdatert så snart som mulig. Noen policyer er klare, men initiativet med å aktivere alle komponenter er fortsatt under arbeid.

### Suverene skyer (US Gov og Kina)

Mange GitHub-problemer knyttet til suverene skyer er for øyeblikket innenfor vårt ansvarsområde, og teamet vårt arbeider aktivt med å løse disse bekymringene.

Beklageligvis, på grunn av strenge sikkerhetskrav som er innebygd i suverene skyomgivelser, har teamet vårt ikke tilgangsrettigheter til å validere policyer eller autentisere vellykket distribusjon av ALZ. For øyeblikket er tilgangstillatelsene våre begrenset utelukkende til den offentlige skyen.

Gitt begrensningene våre med å utføre direkte tester, er vi avhengige av den uvurderlige støtten fra det bredere samfunnet for å hjelpe oss med å identifisere potensielle problemer og tilby konstruktiv tilbakemelding. Vi har som mål å svare på problemstill