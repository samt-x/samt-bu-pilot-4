---
id: eefc1491-0009-4f94-bede-fb970997c68c
# id: auto-generert – kopierte verdier overskrives automatisk ved push
title: Om pilot 4
weight: 10
lastmod: 2026-07-30T09:34:18+02:00
last_editor: Erik Hagen

---
## INNLEDNING

Formålet med SAMT-BU er å etablere et felles fundament for samarbeid og kunnskap om datadrevet tjenesteutvikling og sømløse brukerreiser på tvers av forvaltningsnivåer og sektorer, med pilotering ut fra området barn og unge – fra barnehage til høyere utdanning.  Gjennom praktisk utprøving og kompetansebygging skal prosjektet legge grunnlag for at felles fundamentet er modent og kjent, slik at det blir brukt og bygget videre på, også på andre områder.

Et av prosjektets mest sentrale produkter er å levere MVP-er og pilotering av informasjonsmodeller og tjenester. Prosjektet ønsker også å ha en smidig tilnærming til oppgavene basert på nysgjerrighet og læring gjennom utprøving i korte iterasjoner, slik at vi raskt kan vise reell verdi og også raskt finner ut hva som ikke fungerer (fail fast). Vi vil gjøre dette gjennom pilotprosjekter som dekker prioriterte use cases

Dette dokumentet inneholder en beskrivelse av plan for gjennomføring av pilot .  Selv om vi i dette dokumentet har beskrevet gjennomføring av pilot som en fasedreven prosess, forutsettes det at vi kommer til å arbeide iterativt der enkelte faser gjentas ved behov.

## \
OMFANG OG AVGRENSNING

### Bakgrunn

Arbeidet tar utgangspunkt i casebeskrivelsen 1. Tilgjengeliggjøring av resultater fra opplæring (<https://samt-bu-docs.pages.dev/behov/use-cases/01-resultater-vgo/>)

Caset tar for seg hvordan resultater fra grunnskole og videregående opplæring kan gjøres tilgjengelige på en bedre og mer sammenhengende måte for aktører som har behov for informasjonen i videre oppfølging av unge. Resultater kan for eksempel være status for gjennomført opplæring, fullføring, kompetanse, vurderinger eller dokumentasjon av oppnådd nivå.

### Pilotens målsetning

Målsettingen for piloten er å gjøre forarbeidet før en teknisk implementering. Det vil si få utarbeidet arkitektur, informasjonsmodell, oversikt over hvilke systemer som inneholder data om resultater fra utdanning og starte kartlegging av juridiske behov for å kunne dele disse dataene.

### Pilotens produkter

Piloten har følgende produkter:

- Arkitektur og informasjonsmodell som danner grunnlag for dataflyt av vitnemål og resultater fra utdanning.
- Oversikt over systemleverandører som har vitnemål og resultater fra grunn- og videregående opplæring. Den skal gi oss kontaktpunkter til alle aktører som skal avgi data til kompetanseregisteret.
- Oversikt over juridiske behov knyttet til både digitalisering av vitnemål og å avgi vitnemål og resultater fra grunn- og videregående opplæring.
- Forståelse av hva digitalisering av vitnemål innebærer. Utarbeidelse av informasjonspakke som skal benyttes overfor alle aktører berørt av digitalisering av vitnemål og overføring av data.

Selve gjennomføringen av piloten skal også danne grunnlag for oppdatering av prosjektets «dreiebok» for gjennomføring av denne typen prosjekter.

### Avgrensning

I denne piloten er det ingen tekniske produkter, kun forberedelser knyttet til senere utvikling av API, oppkobling til API-et og grensesnitt for innsyn i utdanningsresultatene.

## BEMANNING

### Pilotprosjektet

Anne Kathrine Haugen, Prosjektleder, [Anne.kathrine.haugen@sikt.no, ](mailto:Anne.kathrine.haugen@sikt.no)Sikt\
Geir Vangen, Arkitekt/Fagdirektør, [geir.vangen@sikt.no,](mailto:geir.vangen@sikt.no) Sikt\
Martin Skurtveit,Tech-leder, [martin.skurtveit@sikt.no,](mailto:martin.skurtveit@sikt.no) Sikt\
Sarah Renså-Skogesal, Jurist, [sarah.rensa.skogesal@sikt.no,](mailto:sarah.rensa.skogesal@sikt.no) Sikt\
Katarina Parteka Aarsnes, Jurist, [katarina.aarsnes@sikt.no,](mailto:katarina.aarsnes@sikt.no) Sikt\
Øystein Nilsen, [Oystein.Nilsen@udir.no, ](mailto:Oystein.Nilsen@udir.no)Udir

## GJENNOMFØRING

### Pilotens varighet

Piloten varer fra 02.05.2026 – 30.08.2026.

### Pilotens arbeidspakker

Det er definert følgende arbeidspakker:

1. Infopakke til alle involverte og systemleverandører. Forståelse av hva digitalisering av vitnemål innebærer.
2. Arkitektur og informasjonsmodell
3. Oversikt over juridiske behov
4. Oversikt over systemleverandører

### Viktige milepæler

Piloten har følgende viktige milepæler:

- Når informasjonspakken er klar
- Når arkitektur og informasjonsmodell er ferdigstilt
- Når det er utarbeidet en oversikt over de juridiske behovene
- Når systemleverandøroversikten er komplett

### Praktisk gjennomføring

Piloten gjennomføres iterativt etter smidige prinsipper og i sprinter på 2 eller 3 uker. Hver sprint starter med et planleggingsmøte og avsluttes med en demo. Det vil bli vurdert å kjøre felles demo i SAMT-BU regi for alle piloter.

I utgangspunktet er det ønskelig å gjennomføre standups for hele piloten samlet annenhver dag, men etter hvert kan det være tilstrekkelig at arbeidet koordineres i hver av arbeidspakkene.

Det bør gjennomføres minst en retrospektiv i løpet av pilotperioden.

Det settes opp en egen GitHub-løsning for prosjektet som skal benyttes i Piloten.

## Overordnet plan

![](bilde-1785396781497.png)## Mål (KPI/OKR)

![](bilde-1785396847637.png)##
