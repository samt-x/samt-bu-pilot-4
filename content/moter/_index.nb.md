---
id: 3945b641-0fb4-4ddd-bdb9-61b7a81acece
title: "Møter"
linkTitle: "Møter"
weight: 18
---
Arkitekturarbeidet i pilot 4 ble gjort i en serie arbeidsmøter fra 22. mai til 28. august 2026 (arbeidsmøte 1–8). [Arkitekturrapporten](../leveranser/arkitektur-og-informasjonsmodell/) bygger på dem. Her er referatene fra de to første møtene. De øvrige referatene ligger foreløpig bare i prosjektets område i Teams.

## Arbeidsmøte 1 – 22. mai 2026

*Første møte i arbeidsmøteserien om arkitektur.*

**Tid:** kl. 14.00–15.30\
**Deltakere:** Geir Magne Vangen, Sikt (møteleder) · Anne Kathrine Haugen, Sikt · Martin Skurtveit, Sikt · Christiane Frolich, Digdir · Kjersti Steien, Digdir · Anders Alstad, Novari

### Bakgrunn og mål

Dette var det første møtet i en serie arbeidsmøter om arkitektur for pilot 4 i SAMT-BU. Piloten skal legge grunnlaget for innrapportering av enkeltresultater og kvalifikasjonsoppnåelse (vitnemål) til semantisk og teknisk arkitektur. [Drømmereisen](../leveranser/drommereise/) er styrende for arbeidet.

Overgangen fra papirdokumentasjon til digitale bevis skal gi innbyggeren full kontroll over egen informasjon, gjøre det mulig for konsumenter (arbeidsgivere, offentlig sektor med flere) å digitalisere og automatisere prosesser, og motvirke svindel.

### Datamodell

Møtet gikk gjennom relevante informasjonsmodeller og standarder:

- CEN European Learner Mobility (ELM) og ELMO (XML-format) – med kjernebegreper som Learner, Issuer, Learning Opportunity Specification, Learning Opportunity Instance, samt støtte for flerspråklighet, utvidelser (extension) og vedlegg (attachment).
- European Learning Model (ELM) ble presentert som rammeverk for læringsdata på europeisk nivå.
- Øvrige standarder fra 1EdTech ble nevnt som referanse: OneRoster, Edu-API, Comprehensive Learner Record og Open Badges.

For Utdanningsregisteret ble disse forholdene drøftet:

- Definisjon av en utdanning basert på ELM-modellen.
- En utdanningsmulighet som del av en institusjons portefølje.
- En utdanningsinstans som tilbys på et gitt tidspunkt og sted.

Modellen for Kompetanseregisteret ble presentert, med den samme overordnede modellen for utdanning som i Utdanningsregisteret, med tillegg for vurdering.

Under denne gjennomgangen ble det så vidt diskutert hvordan innholdet i disse registrene kunne beskrives i Data.norge.no, og videre bruken av semantiske teknologier i denne infrastrukturen. Dette er punkter som er aktuelle å ta med i det videre arbeidet.

### Arkitektur

Martin presenterte teknisk arkitektur sentrert rundt FS GraphQL API. Skissen ble framstilt som Sikts foreløpige tanker på et veldig overordnet nivå, og det ble understreket at detaljene må avklares gjennom prosjektet. I skissen ser man:

- Skolesystemene, med hvilke data Sikt trenger: enkeltresultater, kvalifikasjoner, fødselsnummer for resultateier og fødselsnummer for brukeren som leverer data (for audit).
- Autentisering mot FS GraphQL API gjennom Maskinporten og Feide, eventuelt ID-porten via Feide.
- Registrene for informasjon om, oppbevaring av og deling av utdanning og kompetansebevis, henholdsvis Utdanningsregisteret, Kompetanseregisteret og Vitnemålsportalen.
- Koblinger til internasjonale integrasjoner som EMREX, OOTS/SDG og EUDI Wallet.

I diskusjonen kom det fram at en del av dataene vi ønsker å lagre, stammer fra Grep og Vigos kodeverk, og ikke fra skolesystemene selv. Dette må følges opp når vi går nærmere inn på datamodellen. Det ble også snakket om hvordan skolesystemene i dag leverer vitnemål i kommunene og fylkeskommunene (Vigo og FINT), og at dette mellomleddet kan påvirke hvordan arkitekturen til slutt utformes. Hvordan disse leveranseløpene skal håndteres i den nye arkitekturen, er et åpent spørsmål prosjektet må ta tak i.

Sikt ga også en kort gjennomgang av hvorfor de har satset så tungt på GraphQL og FS GraphQL API. En konsekvens av dette valget er at integrasjonsparter må støtte og forstå GraphQL, eller alternativt at Sikt må tilby en REST-til-FS-GraphQL-kobling for dem som ikke kan integrere direkte. Hvilken modell som er aktuell for hvilke parter, må avklares videre i prosjektet.

### Veien videre

- Vurdere modeller brukt i grunn- og videregående skoler.
- Arbeide videre med forslag til hvordan innrapportering bør implementeres.
- Neste møte i arbeidsgruppen er tirsdag 2. juni kl. 14.00–15.30.

## Arbeidsmøte 2 – 2. juni 2026

*Andre møte i arbeidsmøteserien om arkitektur.*

**Tid:** kl. 14.00–14.35\
**Deltakere:** Geir Magne Vangen, Sikt (møteleder) · Martin Skurtveit, Sikt · Christiane Frolich, Digdir · Kjersti Steien, Digdir · Håkon Søgnen, KS Digital · Truls Waage, Stavanger kommune · Jens Hansen, Stavanger kommune · Eivind Nilssen, Udir

### Agenda

Møtet ble brukt til en overordnet gjennomgang av hvordan Stavanger kommune håndterer vitnemål og resultater for sine grunnskoler. Gjennomgangen fra Novari om løsningen for videregående opplæring ble flyttet til neste møte.

### Stavanger kommune – vitnemål

Truls Waage og Jens Hansen fortalte om løsningen.

Byarkivet i Stavanger kommune har digitalisert alt som er kommet inn fra skolene. Denne dokumentasjonen er skannet til PDF-er og knyttet til den enkelte innbygger. Dokumentasjonen er videre presentert i Min side, innbyggerportalen for Stavanger kommune (inklusive data fra Finnøy og Rennesøy, som ble slått sammen med Stavanger kommune i 2020).

**Vigilo** (skoleadministrativt system): Ved skoleslutt blir vitnemål arkivert for langtidsoppbevaring. Også dette er PDF-er. Dette har vært gjort de siste tre årene.

**Vigo:** Vitnemålsdata overføres til Vigo som strukturerte data (CSV-filer), med dette innholdet:

- fødselsnummer, navn
- fagkode
- karakter, deltatt, fritatt
- e-postadresse for eleven
- (orden/oppførsel og fravær er ikke en del av denne eksporten til Vigo)

Informasjonen leveres til Vigo i tre runder:

1. fødselsnummer, klassetilhørighet
2. karakterer termin 1 (nyttår)
3. karakterer termin 2 (sluttresultat), som også inkluderer karakterer fra trinn 9 (mat og helse)

Det er kommunene selv som bestiller utvikling fra Vigilo. Det finnes et brukerråd for Vigilo som samarbeider om tilbakemeldinger til leverandøren.

Vigilo er i ferd med å implementere den norske profilen av OneRoster 1.2 (1EdTech). Det jobbes fortsatt med justeringer av denne, men den er i stor grad klar til bruk. Mer informasjon om profilen finnes under «OneRoster 1.2 Norwegian Profile» hos IMS Global Learning Consortium (1EdTech).

### Veien videre

- Tema for neste møte blir håndtering av resultater i videregående skoler (skoleadministrativt system, Vigo, mulig rapporteringsvei til nasjonalt kompetanseregister).
- Referatet oppgir «fredag 2. juni kl. 13.00–14.30» som tidspunkt for neste møte. Det kan ikke stemme, siden dette møtet selv ble holdt 2. juni.
