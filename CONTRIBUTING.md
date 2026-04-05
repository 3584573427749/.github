# Contributing Guide

Tack för att du vill bidra till denna plattform!  
Det här dokumentet beskriver hur du arbetar med repositories, branch-policy, kodstandard, tester, versionering och PR-rutiner.

Plattformen bygger på:
- Mikrotjänster (separata repos)
- Strikta API-kontrakt (OpenAPI)
- Slim 4 + PHP-DI + Doctrine DBAL
- CI/CD med GitHub Actions
- Versionshantering per tjänst (semver)

Syftet är att säkerställa hög kodkvalitet och förutsägbara releaser.

---

## Repository-struktur

Varje tjänst finns i sitt eget repo:

- `auth-service`
- `gateway-service`
- `time-service`
- `group-service`
- `results-service`
- `competition-service`
- `main-ui`
- `pwa-trainer`
- `pwa-active`

Det finns också meta-repos:
- `.github` (centrala workflows & templates)
- `platform-docs`
- `infrastructure`

---

## Branch-policy

### Huvudbranch
- `main`  
  Måste alltid vara stabil och deploybar.

### **Feature branches**
Skapas från `main`:
```

feature/xyz
fix/xyz
refactor/xyz

````

### Regler
- **Ingen** får commit:a direkt till `main`.
- Alla ändringar måste ske via Pull Request.
- Minst **1 godkänd review** krävs.

---

## Krav innan du gör en PR

Innan du öppnar en Pull Request ska du:

### 1. Kör all lokal kvalitetssäkring
Följande verktyg måste passera:

```bash
vendor/bin/phpunit
vendor/bin/phpstan analyse src --level=max
vendor/bin/phpcs src
vendor/bin/php-cs-fixer fix --dry-run
vendor/bin/rector process src --dry-run
vendor/bin/infection
````

### 2. Uppdatera `VERSION`-filen

En bump krävs när:

*   **PATCH**: buggfixar, no API change
*   **MINOR**: nya endpoints/fält, bakåtkompatibelt
*   **MAJOR**: breaking API changes

### 3. Uppdatera OpenAPI (om tjänsten har API)

*   Uppdatera `openapi.yaml`
*   Säkerställ att DTO:er och kontrakt matchar implementationen.

### 4. Lägg till/uppdatera tester

*   Services och Domain **måste** vara testade.
*   Actions testas via integrationstester.

***

## Testkrav

### Enhetstester

*   Täcker Services + Domain.

### Integrationstester

*   Täcker Actions och Repositories.

### Mutationstester (Infection)

*   Måste passera miniminivå (konfigurerad i CI).

### Code coverage

*   Ska vara rimlig och motsvara ändringens omfattning.

***

## Kodstandard

### PHP

*   PSR-12 kodstil
*   PHPCS körs i CI
*   PHP-CS-Fixer ska köras (dry-run) innan commit
*   Använd DI (PHP-DI) — inga service locators
*   Actions ska vara tunna (endast I/O + validering)

### JavaScript/TypeScript (UI)

*   ESLint + Prettier (när UI-projektet är igång)
*   Typade API-klienter genererade från OpenAPI

***

## API-kontrakt (OpenAPI)

För tjänster med API gäller:

1.  **Alla ändringar i API kräver:**
    *   uppdaterad `openapi.yaml`
    *   uppdaterade DTO:er
    *   versionbump

2.  **CI kommer att stoppa PR:n** om:
    *   OpenAPI inte är validerat
    *   Breaking change saknar MAJOR bump

***

## Pull Request-process

Varje PR ska innehålla:

### PR-beskrivning

*   Sammanfattning av ändringen
*   Varför den behövs

### Ändringstyp

*   Bug fix
*   New feature
*   Breaking change
*   Documentation

### Checklista

*   CI passerar 
*   Tester uppdaterade
*   Kodstil ok 
*   OpenAPI uppdaterad (om relevant) 
*   VERSION uppdaterad 

### Review-policy

*   Minst 1 granskare måste godkänna
*   Samtliga CI-jobb måste vara gröna

***

## Release-process (översikt)

När en PR mergas i `main`:

1.  CI läser `VERSION`
2.  Docker-image byggs
3.  Taggar skapas (`X.Y.Z`, `X.Y`, `X`, `latest`)
4.  Deploy sker via `docker compose pull && docker compose up -d`
5.  Phinx migrations körs (per tjänst)

Rollback sker genom att deploya en tidigare versionstag.

***

## Tack!

Tack för att du bidrar och hjälper till att hålla plattformen stabil, modern och skalbar.  
Frågor? Skapa en **Discussion** eller ett **Issue** i relevant repo.