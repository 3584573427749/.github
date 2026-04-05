# Simning.ax – Mikrotjänstbaserat Ekosystem

Välkommen till organisationens GitHub!  
Här utvecklas en modern, skalbar och modulär plattform byggd på mikrotjänster, lösenordslös inloggning, kontraktsstyrd utveckling och PWA-baserade användargränssnitt.

Syftet är att skapa ett säkert, snabbt och flexibelt system för:
- tränare  
- aktiva  
- administratörer  
- tävlingsfunktionärer  
- ekonomi/utbetalning  

Plattformen är **API-först**, **modulär** och designad för att kunna utvecklas och driftsättas tjänst-för-tjänst utan beroende av monoliter.

---

## Arkitekturöversikt

Plattformen består av:

### Autentisering
- **Auth-service**  
  Passwordless login (magic-link, OTP, TOTP), JWT-tokenhantering, roller/behörigheter.

### Gateway
- **Gateway-service**  
  Enda exponerade API-ingången  
  → JWT-verifiering  
  → klientpolicy  
  → routing till interna tjänster  

### Tidsrapportering & Resor
- **Time-service**  
  Arbetspass, versionerad timlön, traktamente, milersättning, attestflöden, export.

### Träningsgrupper
- **Group-service**  
  Grupper, aktiva, ledare/assistenter – mjuk koppling mot time- och anmälningssystemet.

### Resultat
- **Results-service**  
  Aktiva, inläsning av resultat med mellantider

### Anmälningar
- **Entries-service**  
  Anmälningar till lokala tävlingar.

### Frontends
- **Admin UI** – administration & attest  
- **Main UI** – tävlingsresultat, föreningsstatistik, personlig statistik, kvaltider, utmanare mm
- **Entris UI** – anmälningar till lokala tävlingar för tränare
- **PWA Tränare** – Tidsrapportering och reseräkningar  
- **PWA Aktiva** – personliga resultat & statistik  

---

## Repository-struktur

Organisationen innehåller följande repos:

### Backend: Mikrotjänster
- `auth-service`
- `gateway-service`
- `time-service`
- `group-service`
- `results-service` (E3)
- `competition-service` (E5)

### Frontend
- `admin-ui`
- `main-ui`
- `entries-ui`
- `pwa-trainer`
- `pwa-active`

### Dokumentation & Infrastruktur
- `.github` – CI templates, workflows, PR-mallar (delas av alla repos)
- `platform-docs` – tekniska specifikationer & arkitekturdokument
- `infrastructure` – Docker Compose, Traefik, prod/stage/dev-miljö

---

## Utvecklingsprinciper

### Mikrotjänster
- En tjänst = en databas  
- Ingen delad SQL  
- HTTP-kommunikation via Gateway  
- Tjänster deployas och versioneras oberoende

### Slim-baserad Action-arkitektur
- En route = en Action-klass  
- Actions är tunna  
- All logik i Services  
- Repositories via Doctrine DBAL

### OpenAPI som kontrakt
- Varje tjänst har en egen `openapi.yaml`  
- CI validerar kontraktet  
- TypeScript-klienter genereras automatiskt  
- Breaking changes stoppas av openapi-diff  

### Versionshantering (Semver)
- Varje repo har en `VERSION`-fil  
- CI kräver bump vid ändringar  
- Docker-images taggas automatiskt

---

## CI/CD & Kvalitet

Alla tjänster använder:

- PHPUnit  
- PHPStan  
- PHPCS  
- PHP-CS-Fixer  
- Infection (mutation testing)  
- Rector (modernisering)  
- GrumPHP (pre-commit)  
- OpenAPI linter + schema-validering  
- Breaking-change detection (openapi-diff)  

Pipelines ligger i `.github`-repon och delas av hela organisationen.

---

## Roadmap (sammandrag)

| Etapp | Leverans |
|-------|-----------|
| **E1** | Auth-service + Gateway + magic-link |
| **E2** | Time-service + Group-service + Admin UI + PWA Tränare |
| **E3** | Results-service + resultatvisning + Main UI|
| **E4** | PWA Aktiva |
| **E5** | Entries-service |

---

## Vill du bidra?

Läs: **CONTRIBUTING.md** i `.github`-repo.  
PR-mallar, kodstandard och CI-regler gäller för alla repos.

---

## Licens

Alla repos i denna organisation är licensierade under MIT-licensen om inte annat anges.

---

## Kontakt & frågor

Skapa ett Issue i relevant repo eller använd Discussions-funktionen om det gäller arkitektur, roadmap eller förbättringsförslag.

---

**Tack för att du bidrar till en modern och hållbar plattform!**  
