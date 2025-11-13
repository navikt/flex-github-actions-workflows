# flex-github-actions-workflows

Dette repoet inneholder reusable GitHub Actions workflowss. Det er en forhåndsdefinert workflow som kan gjenbrukes på
tvers av flere repositorier
eller i forskjellige prosjekter innenfor samme repository. Den lar oss sentralisere og standardisere CI/CD-logikk,
slik at vi kan oppdatere, vedlikeholde og skalere denne logikken på ett sted. Dette reduserer duplisering av kode og
forenkler vedlikehold av GHA flyten vår.

For å bruke en slik workflow, se på en
av [eksemplene her på github](https://github.com/search?q=org%3Anavikt+%22navikt%2Fflex-github-actions-workflows%2F.github%2Fworkflows%2F%22+NOT+is%3Aarchived&type=code).

## Utvikling og endringer

For å teste endringer før de merges inn i main må du gjøre flere ting.

1. Du må lage en branch i dette github actions repoet.
2. Deretter må du finne et repo hvis normale fellesworkflow du vil gjøre endringer i. Lage en branch
   for i repoet og i linjene det kalles felles github actions repoet skriver du f.eks. @branch-i-gha-repo istedenfor
   @main.

```
felles-workflow:
   uses: navikt/flex-github-actions-workflows/.github/workflows/spring-boot.yml@dev-branch-i-gha-repo
```

3. Husk også på at alle "funksjoner" eller "undersworkflows" som kalles på med @main spesifisert i gha felles repoet
   må disse også forandres om du har endret på dem.

## Demo-delete

Denne skalerer ned demo apper til 0 podder når en demo branch er merget.

## Label dependabot PR

Denne workflowen legger til labels på pull requests fra Dependabot basert på typen versjonsoppdatering. Den styrer at
patch og minor oppdateringer blir automatisk merged.

## Merge dependabot PR

Dennee workflowen håndterer automatisk merging av pull requests basert på spesifikke betingelser.
Denne kalles daglig med cronjobber fra alle de andre repoene. I ferieperioder kjøres den ikke.

## NAIS Deploy til Dev og Prod Workflow

Denne workflowet styrer deploy-prosessen av en applikasjon til NAIS dev og prod klynger. Brancher prefixet med `dev-`
vil deployes til dev-gcp. main branchen vil deployes til prod-gcp og dev-gcp.

## Next.js Workflow

Denne bruker av next.js apper som er eksponert på nav.no. Denne er omfattende og inkluderer playwright testing.

## Next.js lightweight Workflow

Brukes av lettvekts next.js apper som ikke er eksponert på nav.no. Denne er enklere enn den over, og har f.eks. ikke
playwright.

## Spring Boot Workflow

Brukes av alle våre spring boot apper

# Henvendelser

Spørsmål knyttet til koden eller prosjektet kan stilles til flex@nav.no

## For NAV-ansatte

Interne henvendelser kan sendes via Slack i kanalen #flex.
