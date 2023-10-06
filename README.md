# flex-github-actions-workflows

Dette repoet inneholder reusable GitHub Actions workflowss. Det er en forhåndsdefinert workflow som kan gjenbrukes på
tvers av flere repositorier
eller i forskjellige prosjekter innenfor samme repository. Den lar oss sentralisere og standardisere CI/CD-logikk,
slik at vi kan oppdatere, vedlikeholde og skalere denne logikken på ett sted. Dette reduserer duplisering av kode og
forenkler vedlikehold av GHA flyten vår.

For å bruke en slik workflow, se på en
av [eksemplene her på github](https://github.com/search?q=org%3Anavikt+%22navikt%2Fflex-github-actions-workflows%2F.github%2Fworkflows%2F%22+NOT+is%3Aarchived&type=code).

## Utvikling og endringer

For å teste endringer før de merges inn i master må du gjøre flere ting. 
1. Du må lage en branch i dette github actions repoet. 
2. Deretter må du finne et repo hvis normale fellesworkflow du vil gjøre endringer i. Lage en branch
for i repoet og i linjene det kalles felles github actions repoet skriver du f.eks. @branch-i-gha-repo istedenfor
@master.

```
felles-workflow:
   uses: navikt/flex-github-actions-workflows/.github/workflows/spring-boot.yml@dev-branch-i-gha-repo
```

3. Husk også på at alle "funksjoner" eller "undersworkflows" som kalles på med @master spesifisert i gha felles repoet
   må disse også forandres om du har endret på dem.

## Demo-delete

Denne skalerer ned demo apper til 0 podder når en demo branch er merget.

## Label dependabot PR

Denne workflowen legger til labels på pull requests fra Dependabot basert på typen versjonsoppdatering:

1. Den kjører kun hvis pull requesten er fra dependabot[bot] og den kommer ikke fra en forket repo.
2. Den henter metadata for Dependabot sin oppdatering.
3. Hvis oppdateringen er av typen "semver-patch", legges etiketten "automerge" til pull requesten.
4. Hvis oppdateringen er av typen "semver-minor", legges også etiketten "automerge" til pull requesten.

## Merge dependabot PR

Dennee workflowen håndterer automatisk merging av pull requests basert på spesifikke betingelser.
Denne kalles daglig med cronjobber fra alle de andre repoene.

1. **Ferieperioder**: Workflowet vil ikke kjøre i bestemte ferieperioder (sommer, jul og nyttår).
2. **Pull Request Betingelser**: PRer blir filtrert basert på:

- De må være åpne.
- De må ikke være fra en fork.
- De må ha en `automerge` etikett.
- De må ha blitt opprettet enten mer enn 2 dager siden (for PRer som inneholder 'navikt' eller 'aksel') eller mer enn en
  uke siden for andre PRer.

3. **Statussjekker**: Bare PRer som har bestått alle sine statussjekker blir vurdert for sammenslåing.
4. **Merging**: Den første sammenslåbare PRen blir sammenslått ved bruk av 'squash' metoden.
5. **Starter Master Workflow**: Etter en vellykket sammenslåing, vil en annen workflow, `workflow.yml`, bli trigget på '
   master'-grenen.

## NAIS Deploy til Dev og Prod Workflow

Dette workflowet styrer deploy-prosessen av en applikasjon til NAIS dev og prod klynger:

### Innstillinger:

- **Image**: Docker image som skal deployes.
- **Applikasjon**: Navnet på applikasjonen.
- **Klynger**: Standardverdier er `dev-gcp` for utviklingsmiljø og `prod-gcp` for produksjonsmiljø.
- **NAIS Template**: Stien til NAIS konfigurasjonsfilen (standard: `nais/app/naiserator.yaml`).
- **Variabler**: Separerte konfigurasjonsfiler for prod og dev (f.eks. `nais/app/prod.yaml` og `nais/app/dev.yaml`).

### Jobber:

1. **Deploy til Dev**:
  - Trigges på `master` eller brancher som starter med `dev-`.
  - Bruker NAIS deploy GitHub Action for å deploye til dev miljøet.

2. **Deploy til Prod**:
  - Kun trigges på `master` gren.
  - Bruker NAIS deploy GitHub Action for å deploye til produksjon.
  - Sender en Slack-melding ved feil under deploy.

## Merknad:

- `concurrency` sørger for at det kun er én deploy-jobb som kjører om gangen for hver app til en gitt miljø (dev eller prod).

## Next.js Workflow
Denne bruker av next.js apper som er eksponert på nav.no. Denne er omfattende og inkluderer cypress testing.

## Next.js lightweight Workflow
Brukes av lettvekts next.js apper som ikke er eksponert på nav.no. Denne er enklere enn den over, og har f.eks. ikke cypress.

## Spring Boot Workflow
Brukes av alle våre spring boot apper


# Henvendelser

Spørsmål knyttet til koden eller prosjektet kan stilles til flex@nav.no

## For NAV-ansatte

Interne henvendelser kan sendes via Slack i kanalen #flex.
