# 🏅 Simning.ax – Modern mikrotjänstarkitektur för åländsk simning

Välkommen till organisationens GitHub‑profil!  
Här utvecklas en komplett, modulär och skalbar idrottsplattform byggd på:

- lösenordslös autentisering (magic‑link, OTP, TOTP)
- gateway‑styrd mikrotjänsarkitektur
- tidsrapportering & resehantering
- träningsgrupper & medlemslogik
- resultat- och tävlingsmoduler
- moderna frontends (Vue 3 + Vite)
- full Docker‑baserad drift

Plattformen är byggd för att vara:
* säker  
* modulär  
* lätt att underhålla  
* flexibel för framtida utbyggnad  
* enhetlig för både backend och frontend  

All kod är licensierad under **MIT‑licensen** om inget annat anges.

---

## Arkitekturöversikt

Plattformen består av flera fristående mikrotjänster.  
Varje tjänst är helt isolerad och deployas i egen Docker‑container.

###  Backend-tjänster
- **auth-service** – passwordless login, roller, behörigheter  
- **gateway-service** – enda exponerade ingressen, JWT‑verifiering & routing  
- **time-service** – tidrapporter, resor, traktamente, milersättning  
- **group-service** – träningsgrupper, ledare & aktiva  
- **results-service** *(kommande)* – aktiva , resultat, historik  
- **entries-service** *(kommande)* – anmälningar till lokala tävlingar  

### Frontend
- **admin-ui** – administrationsgränssnitt  
- **min-ui** – allmänt webbgränssnitt för resultat, statistik, kvaltider mm
- **entries-ui** – gränssnitt för anmälningar till lokala tävlingar  
- **pwa-trainer** – tidrapportering för tränare  
- **pwa-active** – resultat & statistik för aktiva  

Alla frontends är:
- byggda med Vue 3 + Vite  
- levererade som Nginx‑servade Docker‑images  
- API‑drivna med genererade DTO‑typer (OpenAPI → `.d.ts`)  

---

## Docker & Infrastruktur

Plattformen körs uteslutande i Docker‑containers.  
Ett separat repo, **infrastructure**, innehåller:

- `docker-compose.yml` för hela systemet  
- Traefik som API‑gateway/ingress  
- nätverksisolering (privata tjänster + publikt gateway‑lager)  
- miljövariabler / secrets  
- MariaDB (extern eller containerberoende setup)  

Varje tjänst:
- bygger en egen Docker‑image via CI  
- taggas med Semantic Versioning  
- deployas separat  

---

## CI/CD & Kodkvalitet

Alla repos använder samma centrala CI‑policy via `.github`‑organisationens workflow:

### Backend
- PHPUnit  
- PHPStan  
- PHPCS  
- PHP‑CS‑Fixer (dry‑run)  
- Infection (mutation testing)  
- Rector (modernisering)  
- OpenAPI‑validering  
- VERSION‑fil‑kontroll  

### Frontend
- NPM install (CI)  
- ESLint  
- enhetstester (Vitest/Jest)  
- DTO‑generering från OpenAPI → `.d.ts`  
- Vite‑build  
- VERSION‑fil‑kontroll  

### Release
- Docker Buildx  
- Push till GitHub Container Registry (GHCR)  
- Taggar: `X.Y.Z`, `X.Y`, `X`, `latest`  

---

## Repositories

```

backend/
auth-service
gateway-service
time-service
group-service
results-service
entries-service

frontend/
admin-ui
main-ui
pwa-trainer
pwa-active
entries-ui

platform-docs/
infrastructure/
.github/   ← centrala templates/workflows

```

Alla repos är självständiga projekt med egen CI/CD, egen VERSION‑fil och egen releaseprocess.

---

## Roadmap

| Etapp | Innehåll |
|-------|----------|
| **E1** | Auth‑service + Gateway + magic‑link login |
| **E2** | Time‑service + Group‑service + Admin UI + PWA Trainer + Main UI |
| **E3** | Results‑service + resultatvisning och statistik i MainUI |
| **E4** | PWA Aktiva |
| **E5** | Entries‑service + Entries UI |

---

## Licens

Alla projekt är licensierade under:

**MIT License**  
Fri användning, modifiering och distribution.

---

## Bidra

Se **CONTRIBUTING.md** för:
- Branch‑policy  
- PR‑rutiner  
- Versionshantering  
- Testkrav  
- Kvalitetspipelines  

PRs välkomnas! 

---

**Tack för att du är en del av plattformens utveckling!**
