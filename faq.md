---
title: FAQ
layout: default
nav_order: 9
---
# FAQ

Her finder du vejledninger og svar på spørgsmål om hosting, brugeradgang, logning og integrationer i OS2compliance samt links til yderligere dokumentation.

## Hosting hos Digital Identity

Hosting, sikkerhedspraksis og erfaringer fra Digital Identity.

| Krav | Beskrivelse |
|---|---|
| DNSSEC skal være aktiveret for alle domænenavne, som kommunen ejer eller administrerer. | Domænet os2compliance.dk ejes og administreres ikke af kommunen. DNSSEC er aktiveret for os2compliance.dk. |
| Al kommunikation til webgrænseflader skal være krypteret via HTTPS med TLS 1.2 eller nyere. | OS2compliance anvender TLS 1.2 og TLS 1.3 på alle eksternt eksponerede endpoints. |
| Serversoftware på alle webservere skal holdes løbende og regelmæssigt opdateret. | Patchning og opdatering af servere indgår som en fast aktivitet i driftsårshjulet. |
| Løsningen skal være beskyttet mod de væsentligste webapplikationssårbarheder, herunder de risici, der er beskrevet i OWASP Top 10. | Der anvendes ikke en særskilt OWASP Top 10-kontrolramme. Risici håndteres som en del af den generelle risikostyring og risikobehandling. Ved gennemgang i august 2026 af den aktuelle OWASP Top 10 vurderes emnerne at være dækket af den eksisterende risikostyring. |
| HSTS (HTTP Strict Transport Security) skal være implementeret. | HSTS er implementeret via Strict-Transport-Security-headeren på alle OS2compliance-applikationer. |

| Spørgsmål | Svar |
|---|---|
| Er der kendte kunder, som har integration til fx Splunk eller Sentinel? | Digital Identity kender ikke til andre kunder, der har etableret en sådan integration. |

## Brugergrænseflade og adgang

Browsere, tilgængelighed og brugerstyring for alle hostingformer.


| Krav | Beskrivelse |
|---|---|
| Løsningen skal understøtte de to seneste versioner af gængse browsere. | OS2compliance testes i Chromium-baserede browsere og Firefox. Der er ikke fastlagt en særskilt testmatrice for samtlige browsere og versionskombinationer. |
| Løsningen skal understøtte responsivt design. | OS2compliance har et responsivt layout. Styregruppen har besluttet, at løsningen primært udvikles til brug på større skærme, og der gennemføres derfor ikke særskilt test på små skærmstørrelser. |
| Løsningen skal understøtte Single Sign-On (SSO), eksempelvis via fælleskommunal adgangsstyring, Entra ID eller AD FS. | Single Sign-On er standard og den eneste understøttede loginform i OS2compliance. |
| Bruger-id og adgangskoder skal kunne synkroniseres sikkert med AD/Entra ID for administrative medarbejdere. | OS2compliance synkroniserer ikke bruger-id eller adgangskoder med AD/Entra ID. Autentifikation sker i stedet via fødereret login, jf. ovenstående. |
| Brugerstyringen skal være enkel og understøtte effektiv oprettelse, nedlæggelse og afgrænsning af brugeres adgang. Løsningen skal så vidt muligt være IdM-kompatibel og basere rettighedsstyringen på roller. | Brugerstyringen er fødereret. Roller og rettigheder administreres via kommunens IdM (eksempelvis OS2rollekatalog) og anvendes i OS2compliance. |

| Spørgsmål | Svar |
|---|---|
| Er løsningen udviklet og testet med henblik på WCAG-standarden for webtilgængelighed? | OS2compliance er ikke udviklet eller testet specifikt med udgangspunkt i WCAG-standarden. |
| Hvordan håndterer løsningen cookies, herunder relevante sikkerhedsindstillinger? | OS2compliance anvender cookies. Relevante cookies sættes med attributterne HttpOnly og Secure. |
| Hvilke loginmuligheder understøtter OS2compliance? | OS2compliance understøtter generelt SAML-baseret login. Ved anvendelse af fælleskommunal adgangsstyring oprettes it-systemet i KOMBIT, hvorefter kommunen opretter og tildeler de relevante roller i FK Adgangsstyring eller Rollekataloget. |

## Logning og integrationer

Applikationslogs, ændringshistorik og integrationer for alle hostingformer.

| Spørgsmål | Svar |
|---|---|
| Hvilke logfiler genererer OS2compliance som standard? | OS2compliance er en Spring Boot-applikation. Som standard skriver applikationen til konsollen (stdout) og opretter ikke selv logfiler. I en Docker-installation håndteres loggen derfor af Docker eller den konfigurerede logdriver, fx journald. |
| Hvilke typer hændelser logges i applikationen? | Standardlogniveauet er INFO. Loggen indeholder bl.a. opstart, kørsel af planlagte job (fx synkronisering med KITOS og KLE), fejl i integrationer samt uventede fejl med stacktrace (ERROR). |
| Findes der dokumentation for logstruktur og logniveauer? | Logniveauerne er de almindelige SLF4J/Logback-niveauer. Der findes ikke særskilt dokumentation af logstrukturen eller en yderligere samlet teknisk dokumentation for logning og revisionsspor. |
| Kan man se, hvilke ændringer en API-klient har foretaget? | Ja. Ændringer foretaget via API registreres i handlingsloggen med API-klientens navn som udfører. Som ved brugerændringer vises før- og efterværdier på feltniveau. |
| Kan man identificere ændringer foretaget af de indbyggede integrationer? | Kun hvis ændringen foretages via API. De indbyggede planlagte synkroniseringer, fx KITOS-synkroniseringen, opretter ikke i dag en selvstændig linje i handlingsloggen. |
| Er handlingsloggen i version 2.8 den primære revisionslog? | Ja. Handlingsloggen er den primære revisionslog og indeholder også login og logout. |
| Hvilke objekter er omfattet af handlingsloggen? | Handlingsloggen dækker aktiver, risikovurderinger, DPIA'er, hændelser, opgaver, behandlingsaktiviteter, dokumenter, leverandører, standarder, brugere, valglister og mailskabeloner. Foranstaltninger og kontakter vises ikke i handlingsloggen i dag. |
| Kan handlingsloggen eksporteres? | Ikke fra brugergrænsefladen i dag. Data ligger i databasen i tabellen auditlog og kan hentes direkte derfra. En eksportfunktion kan udvikles som en mindre udvidelse. |
| Kan opbevaringsperioden for handlingsloggen på 13 måneder konfigureres? | Nej, de 13 måneder er faste i dag. Oprydning kører hver nat og sletter også den bagvedliggende historik med før- og efterværdier. Perioden kan gøres konfigurerbar som en mindre udviklingsopgave. |
| Kan man se, hvis en integration overskriver en brugers ændring? | Ja, hvis integrationen skriver via OS2compliance' API. Brugerens ændring og den efterfølgende API-opdatering vises som to poster i handlingsloggen med udfører, tidspunkt samt før- og efterværdi. Det gør det muligt efterfølgende at se, at en brugers værdi er blevet overskrevet af integrationen. |
| Kan felter, der synkroniseres fra anden løsning eksempelvis ServiceNow, gøres skrivebeskyttede og markeres som integrationsfelter? | Ikke i dag. ServiceNow-synkroniserede felter kan aktuelt ikke gøres skrivebeskyttede eller få et ServiceNow-ikon på samme måde som KITOS-felter. Det kan udvikles som en udvidelse. |

### Logopsætning ved egen hosting

Opsætning af loglagring, rotation og videresendelse ved egen drift.

| Spørgsmål | Svar |
|---|---|
| Hvor lagres applikationslogs i en selvhostet installation? | Som standard lagres der ikke en særskilt logfil ud over stdout. Filbaseret logning kan konfigureres med almindelige Spring Boot-indstillinger, fx logging.file.name, eller via en egen logback.xml i config-mappen. Det kræver ikke kodeændringer. |
| Logges API-kald til og fra OS2compliance? | Som standard logges der ikke en linje for hvert API-kald. Afviste kald, fx ved ugyldig eller manglende ApiKey, samt fejl logges. Ved DEBUG for org.springframework.web.filter.CommonsRequestLoggingFilter kan hvert kald logges med endpoint, query-string og payload, men dette kan give store logmængder, og klientnavn og statuskode indgår ikke i denne logning. |
| Hvordan håndteres logrotation og logoprydning for applikationslogs? | Applikationen roterer eller rydder ikke selv op i konsolloggen. Det håndteres af Docker, logdriveren eller den valgte logplatform. Ved filbaseret logning kan rotation styres med logging.logback.rollingpolicy.*. |
| Er der en anbefalet opbevaringstid for applikationslogs? | Der er ikke fastsat en særskilt anbefaling fra OS2compliance. Opbevaringstiden bør følge organisationens egen logpolitik. |
| Understøtter OS2compliance videresendelse af logs til SIEM- eller logmanagement-løsninger? | Der er ikke en indbygget log-forwarder. Stdout kan opsamles af organisationens egen agent eller logplatform, fx Splunk, Sentinel/AMA eller Fluent Bit. Konsolloggen kan konfigureres som struktureret JSON (ECS/Logstash) med logging.structured.format.console. Handlingsloggen ligger i databasen (auditlog) og kan hentes derfra, hvis den også skal indgå i SIEM. |

## Dokumentation og vejledninger

Links til roller, releases, arkitektur, API og integrationer.

| Spørgsmål | Svar |
|---|---|
| Hvilke roller findes i OS2compliance, og hvilke rettigheder har de? | Se [Roller i OS2compliance](roller.md) for en oversigt over roller og tilladelser. |
| Hvor kan jeg læse om ændringer i de enkelte releases? | Se [Releases](changelogs/changelogs.md) for releasebeskrivelser. |
| Hvor kan jeg finde en beskrivelse af integrationerne? | Se [Integrationer](integrations/integrations.md) for en oversigt over integrationerne. |
| Hvor er OS2compliance API'et dokumenteret? | Se [API](api/api.md) for dokumentation af OS2compliance REST API. |
| Hvor kan jeg læse om OS2compliance arkitektur? | Se [Arkitektur](architecture.md) for en beskrivelse af systemarkitektur, integrationer og deployment. |
| Hvor kan jeg finde oplysninger om KITOS-integrationen? | Se [KITOS-integration](integrations/kitos.md) for en beskrivelse af integrationen og feltoversigten. |
