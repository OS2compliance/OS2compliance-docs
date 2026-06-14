---
title: Roller
layout: default
parent: Anvendelse
nav_order: 1
---
# Roller i OS2compliance
OS2compliance har et antal roller, der styrer hvad en bruger kan se og redigere i løsningen. Nedenfor beskrives rollerne og deres tilladelser, efterfulgt af en samlet oversigt over funktioner pr. rolle.

## Roller og tilladelser

| Rolle | Tilladelse |
|---|---|
| Læseadgang | Rollen skal kunne læse i hele løsningen, men skal ikke have adgang til Administrativt og tandhjulet<br>Rollen skal ikke kunne vælges som ansvarlig |
| Begrænset adgang | Kan vælges som ansvarlig<br>Har adgang til at se og opdatere opgaver, systemer og behandlingsaktiviteter, hvor brugeren er angivet i et af rollefelterne<br>Har adgang til at oprette opgaver til sig selv<br>Kan underskrive risikovurderinger og DPIA'er<br>Brugeren kan ikke ændre ansvarlig eller ændre opgavens deadline |
| Bruger | Kan vælges som ansvarlig<br>Har læseadgang<br>Kan redigere og slette i Behandlingsaktiviteter, Aktiver, Risikovurderinger, Dokumenter og Opgaver, som brugeren er angivet som ejer eller ansvarlig for<br>Kan redigere og slette i Standarder og Leverandører |
| Superbruger | Kan vælges som ansvarlig<br>Har fuld adgang til at oprette, redigere og slette i Standarder, Behandlingsaktiviteter, Aktiver, Leverandører, Risikovurderinger, Dokumenter og Opgaver |
| Administrator | Har samme rettigheder som en superbruger<br>Har mulighed for at justere systemindstillinger (via tandhjulet i øverste højre hjørne)<br>Administratoren har også adgang til at konfigurere trusselskatalog, konsekvensanalyse og hændelser |

## Oversigt over funktioner pr. rolle

| Modul | Læseadgang | Begrænset adgang | Bruger | Superbruger | Administrator |
|---|---|---|---|---|---|
| Dashboard | Se | Se/Rediger | Se/Rediger/Slette | Se/Rediger/Slette | Se/Rediger/Slette |
| Standarder | Se | ❌ | Se/Rediger/Slette | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Fortegnelse | Se | Se/Rediger<br>(kun dem man er ansvarlig for) | Se/Rediger/Slette<br>(kun hvis ansvarlig) | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Aktiver | Se | Se/Rediger<br>(kun dem man er ansvarlig for) | Se/Rediger/Slette<br>(kun hvis ansvarlig) | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| DBS-aktiver | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/Rediger |
| DBS-tilsyn | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/Rediger |
| Leverandører | Se | ❌ | Se/Rediger/Slette | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Risikovurderinger | Se | Se/Rediger<br>(kun risikovurderinger for de systemer man er systemansvarlig for, systemejer, risikoejer eller sat på til signering) | Se/Rediger/Slette<br>(kun hvis ansvarlig) | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Konsekvensanalyser | Se | Se/Rediger<br>(kun for de systemer man er systemansvarlig, systemejer eller risikoejer for eller sat på til signering) | Se/Rediger/Slette<br>(kun hvis ansvarlig) | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Foranstaltninger | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Trusselskataloger | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Skabelon til konsekvensanalyse | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Dokumenter | Se | ❌ | Se/Rediger/Slette<br>(kun hvis ansvarlig) | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Opgavecenter | Se | Se/Rediger<br>(kun dem man er ansvarlig for) | Se/Rediger/Udfør/Slette<br>(kun hvis ansvarlig) | Se/oprette/Udfør/Rediger/Slet | Se/oprette/Udfør/Rediger/Slet |
| Hændelseslog | Se | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Opsætning | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet | Se/oprette/Rediger/Slet |
| Rapporter | ❌ | ❌ | Se/udskriv | Se/udskriv | Se/udskriv |
| Administrativt | ❌ | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet |
| Tandhjul | ❌ | ❌ | ❌ | ❌ | Se/oprette/Rediger/Slet |
