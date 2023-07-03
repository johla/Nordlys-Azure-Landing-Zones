# Hvordan hjelper Azure Policies i Nordlys arkitekturen?

Azure Policy gjør det mulig å *kodifisere* kravene til bedriftsstyring. Implementeringen av Nordlys arkitekturen bruker i stor grad Azure Policies for å gjøre det enkelt å implementere ulike sikkerhetsmekanismer som er nødvendige for å oppfylle drifts- og styringskrav. Nedenfor diskuteres de vanligste kravene til bedriftsstyring og hvordan de kan konfigureres ved hjelp av Azure Policies i Nordlys arkitekturen.

## Hindre opprettelse av tjenester med offentlig IP

De fleste Azure-plattform-tjenester (PaaS) opprettes med en offentlig IP-adresse tilordnet dem. Dette er et godt alternativ for utviklere som ønsker å komme raskt i gang med disse tjenestene. Offentlig endeakselererer læringskurven og er ideell når man utvikler piloter og Proof of Concept (PoC) i liten skala.

Imidlertid blir bruken av offentlige IP-adresser noen ganger oversett når disse pilotene/PoC-ene blir produksjonsklare bedriftsapplikasjoner.

Produksjonsarbeidsmengder som bruker offentlige IP-adresser uten tilstrekkelige sikkerhetstiltak kan øke sikkerhetsrisikoen. Ondsinnet aktører kan potensielt bruke offentlig IP som en inngangsport for å lansere et angrep. Mange bedriftsregler for samsvar tillater ikke bruk av offentlig IP bare for å unngå eksponering for slike sikkerhetsrisikoer.

Det finnes en tilpasset Nordlys arkitekturen-policy som nekter opprettelse av offentlig IP-adresse, noe som hindrer opprettelse av offentlig IP i et område som er målet for policyen. Bedrifter kan enkelt forhindre opprettelse av virtuelle maskiner (VM-er) med offentlig IP ved hjelp av denne policyen.

På samme måte finnes det en tilpasset Nordlys arkitekturen Policy-initiativ (også referert til som PolicySet) som hjelper bedrifter med å forhindre at Azure-tjenester opprettes med en offentlig IP-adresse i utgangspunktet.

## Pålegge innsamling av revisjon og logginformasjon

Mangelen på revisjons- og diagnoseinformasjon på granulært nivå kan påvirke operative praksiser. Ufullstendig revisjonsinformasjon gjør det vanskelig å korrelere logger fra flere Azure-tjenester og danne en sammenhengende feilsøkingsopplevelse.

Det er ønskelig at Azure-tjenester, når de er provisjonert, gir detaljert informasjon om Azure-plattformen de samhandler med. Denne informasjonen kan grovt sett deles inn i logger og metrikker. Hver Azure-tjeneste kan videre kategoriseres i sine underkomponenter (f.eks. har en Azure Public IP-ressurs "DDoSProtectionNotifications", "DDoSMitigationReports" og "DDoSMitigationFlowLogs" som under

komponenter). Innsamling av diagnostisk informasjon på disse underkategoriene kan i stor grad forbedre revisjons- og feilsøkingsopplevelsen.

Nordlys arkitekturen implementerer et tilpasset Policy-initiativ for å tvinge fram innsamling av logger og metrikker på et dypere nivå, noe som hjelper bedrifter med å samle inn logger og metrikker per Azure-tjeneste. Dette initiativet inkluderer en policy for hver Azure-tjeneste. Viktige loggkategorier for hver Azure-tjeneste og alle metrikker samles automatisk gjennom disse policyene.

## Gi omfattende sikkerhet for SQL-databaser

SQL-databaser er en utbredt Azure-tjeneste i de fleste Azure-implementeringer. Dessverre er de også et hovedmål for ondsinnede aktiviteter fra både interne og eksterne aktører i en bedrift.

Et tilpasset Nordlys arkitekturen Policy-initiativ spesifikt for SQL-databaser hjelper med å implementere følgende sentrale styringspraksiser.

### Krypter SQL-data i hvile

SQL-databasen og sikkerhetskopiene er utsatt for risikoen for at ondsinnede aktører får tak i dem. Det er veldig enkelt å gjenopprette SQL-databasen enten fra databasfiler eller sikkerhetskopier. Uten et ordentlig forsvarssystem på plass kan ondsinnede aktører få tilgang til all dataen.

Å sørge for at SQL-databasen er kryptert i hvile er et av de første trinnene for å bygge en forsvarsstrategi for SQL-databasen. Azure SQL-database Transparent Data Encryption (TDE) sikrer at dataen er kryptert i hvile uten behov for noen endring i applikasjonskoden.

En SQL-database med TDE aktivert gjør det vanskelig for ondsinnede aktører å få tilgang til dataen den inneholder, selv om den er kompromittert.

Et Nordlys arkitekturen-tilpasset policy implementeres for å sikre at Azure SQL-databaser har TDE aktivert.

### Pålegge varsler for mistenkelig aktivitet

Ondsinnet aktører er stadig på utkikk etter å få tilgang til og utnytte forretningskritiske Azure SQL-databaser. Risikoen for at slike forsøk går ubemerket hen kan redusere en bedrifts evne til å oppdage og reagere på dem. I verste fall kan en bedrift aldri få vite om SQL-databasen er kompromittert.

Azure SQL-database gir mulighet for å sette opp sikkerhetsvarsler som kan rapportere mistenkelig aktivitet på SQL-serveren. Slike varsler sender en e-post til forhåndskonfigurerte e-postadresser, og kan valgfritt sende til Azure-abonnementsadministratorer og -eiere.

Nordlys arkitekturen implementerer en tilpasset policy som tvinger aktivering av sikkerhetsvarsler på Azure SQL-databaser. Bedriften kan dra nytte av å identifisere ondsinnede aktiviteter som SQL-injeksjonsangrep, brute force-angrep, os

v. gjennom disse varslingene. Sikkerhetsvarsler gir detaljert informasjon om hver hendelse, og denne informasjonen vises både i Azure-portalen og i e-postmeldinger.

### Pålegge revisjonsspor av operasjoner

En forretningskritisk Azure SQL-database kan være gjenstand for et stort antall DML (Data Manipulation Language), DCL (Data Control Language) og DDL (Data Definition Language) kommandoer som en del av daglige operasjoner. Uten klar kontroll og innsikt i disse operasjonelle aktivitetene, blir det utfordrende å skille mellom legitime og mistenkelige operasjoner.

Å aktivere SQL-revisjon kan bidra til å samle viktig informasjon om alle databaseaktiviteter. Det er også et krav for mange bransje-/regionale samsvarskrav. SQL-revisjon hjelper til med å generere og rapportere en revisjonssporing av databasehendelser.

Bedrifter kan bruke en tilpasset Nordlys arkitekturen-policy for å tvinge revisjon av Azure SQL Database. Denne policyen revisjonerer og rapporterer viktige databasehendelser som endringer i eierskap, vellykkede/mislukkede pålogginger, endringer i rollemedlemskap, endringer i skjema, osv. Bedrifter kan bruke denne policyen og revisjonssporingen den genererer for å få verdifulle innsikter i databaseoperasjoner og overholde bransje-/regionale samsvarskrav.

### Pålegge evaluering i henhold til anerkjente beste praksiser

Gjennom hele livssyklusen til Azure SQL-databasen gjennomgår den et stort antall endringer i skjema, tillatelser og konfigurasjon. Det er alltid en risiko for at slike endringer kan føre til avvik fra beste praksiser. Overflødige tillatelser, forlatte roller og mange slike konfigurasjonsavvik kan utnyttes av ondsinnede aktører.

Azure SQL-database har innebygd sårbarhetsvurderingstjeneste. Tilstanden til Azure SQL-database sett gjennom Microsofts beste praksis for SQL-database kan evalueres ved hjelp av sårbarhetsvurdering. En sårbarhetsvurderingsskanning identifiserer sikkerhetsrisikoer på database- og serversnivå. En rettede oppgave genereres også ved behov for å rette opp sårbarheten.

En tilpasset policy implementert i Nordlys arkitekturen sikrer at Azure SQL-databaser er konfigurert med sårbarhetsvurdering. Sårbarhetsvurderingsskanningene utføres regelmessig, og rapportene lagres i Azure-lagringskontoen. For periodiske skanningresultater for rapporteringsformål brukes en forhåndsdefinert e-postadresse.

## Beskytt mot bevisst/ubevisst sletting av hemmeligheter

Azure Key Vault er en tjeneste for å lagre konfidensiell informasjon, for eksempel nøkler, sertifikater, passord os

v. Azure Key Vault spiller en viktig rolle i sikkerhetslandskapet og brukes ofte til å lagre og administrere hemmeligheter for Azure-applikasjoner.

Det er viktig å sikre at ingen utilsiktet eller bevisst sletting av hemmeligheter skjer. Nordlys arkitekturen har en tilpasset policy for Azure Key Vault som tvinger en sikkerhetskopi av hemmeligheter i en annen Azure-region for beskyttelse mot utilsiktet sletting. Dette bidrar til å sikre at selv om en Key Vault eller hemmeligheter blir slettet, er det en sikkerhetskopi i en annen region som kan gjenopprettes.

Azure Policy i Nordlys-arkitekturen gir dermed et sett med retningslinjer og kontroller som kan håndheves på tvers av Azure-abonnementet for å oppfylle drifts- og styringskrav. Det hjelper til med å sikre at bedriftene opprettholder høy sikkerhet og samsvarsnivå for deres Azure-implementeringer.