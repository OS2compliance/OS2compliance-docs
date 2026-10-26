---
title: Kitos
layout: default
parent: Integrations
has_children: false
---
# Kitos

OS2compliance integrerer med [OS2kitos](https://os2.eu/produkt/os2kitos), kommunens systemoverblik. Integrationen går begge veje: IT-systemer, anvendelser og kontrakter hentes fra KITOS og bliver til aktiver i OS2compliance, og et mindre udvalg af felter skrives tilbage til KITOS, så compliance-arbejdet er synligt der.

Felter der styres fra KITOS er skrivebeskyttede i OS2compliance og markeret med KITOS-logoet på aktivet. Rettelser skal foretages i KITOS og slår igennem ved næste synkronisering.

Yderligere teknisk dokumentation af KITOS' API findes i [KITOS' egen tekniske dokumentation](https://os2web.atlassian.net/wiki/spaces/KITOS/pages/657391621/Teknisk+dokumentation).

## Fra KITOS til OS2compliance

Synkroniseringen kører automatisk efter et fast skema. Hvert IT-system i KITOS bliver til ét aktiv i OS2compliance, og aktivet holdes opdateret ved efterfølgende kørsler.

### IT-system

| KITOS | OS2compliance |
|---|---|
| Navn | Navn |
| Beskrivelse | Beskrivelse |
| Rettighedshaver (navn + CVR) | Leverandør — oprettes automatisk hvis den ikke findes i forvejen |
| Systemforside → Referencer vedr. systemet (URL) | Links på aktivet — kun hvis indstillingen *Aktiver → kilde til links* står på dette valg |

Når et aktiv oprettes første gang, sættes derudover en række standardværdier, som efterfølgende kan rettes i OS2compliance: aktivtype *IT-system*, status *Ikke påbegyndt* og kritikalitet *Ikke kritisk*. Databehandleraftale står blank.

### IT-systemanvendelse

| KITOS | OS2compliance |
|---|---|
| Generelt → Forretningskritisk | Kritikalitet (*Kritisk* / *Ikke kritisk*) |
| Generelt → Indeholder AI-teknologi | AI-status |
| Arkivering → Arkiveringspligt | Systemet skal arkiveres |
| Roller | Systemejer, Systemansvarlig og Driftsansvarlig — se [Opsætning](#opsætning) |
| Lokale referencer → Dokumenttitel (URL) | Links på aktivet — kun hvis indstillingen *Aktiver → kilde til links* står på dette valg |

Er en anvendelse ikke gyldig i KITOS, sættes aktivet inaktivt, og felterne ovenfor opdateres ikke. Se [Aktiv eller inaktiv](#aktiv-eller-inaktiv).

### IT-kontrakt

| KITOS | OS2compliance |
|---|---|
| Kontraktforside → Gyldig fra | Kontraktdato |
| Kontraktforside → Gyldig til | Kontraktophør |
| Opsigelse → Opsigelsesfrist (måneder) | Opsigelsesvarsel |

Kontraktdata hentes også fra kontrakter der er ophørt. Løber der en kontrakt på anvendelsen, er det dens værdier der vises. Er alle ophørt, vises den der udløb senest, og aktivet sættes inaktivt.

### Brugere og roller

Alle brugere i kommunens KITOS-organisation hentes og kobles til brugere i OS2compliance — først på e-mailadresse, og hvis den ikke giver et match, på navn. Findes der flere brugere med samme navn, springes koblingen over, og det fremgår af loggen. Uden en kobling kan brugeren ikke sættes som systemejer, systemansvarlig eller driftsansvarlig fra KITOS.

Kommunens rolleliste fra KITOS hentes samtidig, så den kan vælges i indstillingerne.

### Aktiv eller inaktiv

Om et aktiv er aktivt, styres fra KITOS og er skrivebeskyttet i OS2compliance. Aktivet sættes inaktivt når

* anvendelsen har livscyklusfasen *Ikke i brug* eller ikke er gyldig i KITOS. Fasen *Ubesluttet* eller en manglende fase inaktiverer ikke
* alle kontrakter på anvendelsen er udløbet eller opsagt. En kontrakt der endnu ikke er startet, eller som er spærret af en ugyldig hovedkontrakt, tæller ikke som ophørt
* IT-systemet eller anvendelsen slettes i KITOS. Den tidligere KITOS-nøgle gemmes på aktivet, men det synkroniseres ikke længere

Aktivet bevares med sine risikovurderinger og konsekvensanalyser. Ændres fasen, eller løber en kontrakt igen, bliver aktivet aktivt igen. Efter en ny kontrakt kan det tage op til et døgn.

Der kan sendes en mail når synkroniseringen inaktiverer et aktiv eller opretter et nyt. Modtagere og de to hændelser slås til under **Indstillinger**, og teksten rettes under **Mailskabeloner**.

## Fra OS2compliance til KITOS

OS2compliance skriver kun til de felter der er nævnt nedenfor. Øvrige oplysninger i KITOS berøres ikke.

### Automatisk ved ændring af et aktiv

| OS2compliance | KITOS |
|---|---|
| Kritikalitet | Generelt → Forretningskritisk (Ja/Nej) |
| Systemet skal arkiveres | Arkivering → Arkiveringspligt |

Der skrives kun til KITOS hvis værdien reelt er ændret, så samtidige rettelser i KITOS ikke overskrives.

### Risikovurdering

Sendes manuelt med knappen **Synkroniser til OS2kitos** på aktivets fane *Risikovurdering*. Tidspunktet for seneste synkronisering vises under knappen.

| OS2compliance | KITOS |
|---|---|
| Er der foretaget risikovurdering | GDPR → Risikovurdering gennemført |
| Dato for seneste risikovurdering | GDPR → Dato for seneste risikovurdering |
| Resultat | GDPR → Risikovurderingens resultat |
| Navn på og link til seneste risikovurdering | GDPR → Dokumentation for risikovurdering |
| Dato for næste revision | GDPR → Planlagt dato for næste risikovurdering |

I dialogen vælges enten **Udfyld automatisk**, hvor værdierne hentes fra aktivets nyeste risikovurdering, eller **Indtast**, hvor de tastes manuelt. Ved manuel indtastning sendes hverken link eller dato for næste revision.

### Konsekvensanalyse (DPIA)

Sendes manuelt med knappen **Synkroniser til OS2kitos** på aktivets fane *Konsekvensanalyse*.

| OS2compliance | KITOS |
|---|---|
| — (sættes altid til Ja) | GDPR → DPIA gennemført |
| Oprettelsesdato for nyeste konsekvensanalyse | GDPR → DPIA-dato |
| Navn på og link til konsekvensanalysen | GDPR → DPIA-dokumentation |

## Opsætning

Indstillingerne findes under **Indstillinger → Kitos** og vises kun når integrationen er slået til i konfigurationen.

| Indstilling | Beskrivelse |
|---|---|
| Skal OS2compliance synkronisere med KITOS? | Slår oprettelse og opdatering af aktiver til og fra. Brugere og roller synkroniseres uanset |
| Vælg *Systemejer*-rolle i Kitos | Hvilken KITOS-rolle der udpeger aktivets systemejer |
| Vælg *Systemansvarlig*-rolle i Kitos | Hvilken KITOS-rolle der udpeger aktivets systemansvarlige |
| Vælg *Driftsansvarlig*-rolle i Kitos | Hvilken KITOS-rolle der udpeger aktivets driftsansvarlige |
| Aktiver → kilde til links | Om links hentes fra systemets forside-referencer eller fra anvendelsens lokale referencer. Er der ikke valgt en kilde, synkroniseres links ikke |
| Kilde til kontraktdato | Hvilket KITOS-felt der bliver til Kontraktdato |
| Kilde til kontraktophør | Hvilket KITOS-felt der bliver til Kontraktophør |

Betegnelserne *Systemejer*, *Systemansvarlig* og *Driftsansvarlig* er de danske standardnavne. De kan omdøbes under Indstillinger, og de valgte navne bruges både på aktivet og i rollevalgene ovenfor.

Ændringer i opsætningen slår først igennem ved næste synkronisering, og der kan gå op til et døgn før alle aktiver er opdateret.

## Synkroniseringsskema

| Kørsel | Standard | Hvad den gør |
|---|---|---|
| Synkronisering | Hvert 30. minut | Henter ændrede anvendelser, IT-systemer og kontrakter |
| Sletninger | 10 og 40 minutter over hver time | Inaktiverer aktiver hvis systemet eller anvendelsen er slettet i KITOS |
| Nulstilling af delta | Kl. 03:00 | Tvinger næste kørsel til at gennemgå alle systemer på ny |

Synkroniseringen er inkrementel: KITOS spørges kun om det der er ændret siden sidste kørsel. Den natlige nulstilling sikrer at ændringer der ikke er markeret korrekt i KITOS, alligevel kommer med.

## Konfigurationsproperties

| Variabel | Standardværdi | Beskrivelse |
|---|---|---|
| `INTEGRATION_KITOS_ENABLED` | `false` | Slår KITOS-integrationen til |
| `INTEGRATION_KITOS_BASE_PATH` | `https://kitos.dk` | URL til KITOS |
| `INTEGRATION_KITOS_USER_EMAIL` | | E-mail på KITOS API-brugeren |
| `INTEGRATION_KITOS_PASSWORD` | | Adgangskode til KITOS API-brugeren |
| `INTEGRATION_KITOS_CRON` | `0 */30 * * * ?` | Hvor ofte der synkroniseres |
| `INTEGRATION_KITOS_DELETION_CRON` | `0 10-59/30 * * * ?` | Hvor ofte der synkroniseres sletninger |
| `INTEGRATION_KITOS_FULL_SYNC_CRON` | `0 0 3 * * ?` | Hvornår delta-markeringen nulstilles |

Kommunens CVR-nummer i instansens konfiguration bestemmer hvilken KITOS-organisation der synkroniseres fra.

## Kendte begrænsninger

* Valgene **Systemer → Kontrakt → Indgået** og **Systemer → Kontrakt → Udløber** kan vælges som kilde til henholdsvis kontraktdato og kontraktophør, men er ikke implementeret. Vælges de, synkroniseres den pågældende dato ikke. Brug i stedet de to *Kontraktforside*-valg.
