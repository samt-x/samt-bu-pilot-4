---
id: 85e2e51d-1d14-407f-845a-92d9ace9b00a
title: "Arbeidsmøte 1 – 22. mai 2026"
linkTitle: "Arbeidsmøte 1"
weight: 10
lastmod: 2026-11-19T21:36:39+01:00
last_editor: Erik Hagen

---
*Referat fra første møte i arbeidsmøteserien om arkitektur for pilot 4.*

**Dato:** 22. mai 2026, kl. 14.00–15.30\
**Prosjekt:** SAMT-BU, pilot 4

**Deltakere:**

- Geir Magne Vangen, Sikt (møteleder)
- Anne Kathrine Haugen, Sikt
- Martin Skurtveit, Sikt
- Christiane Frolich, Digdir
- Kjersti Steien, Digdir
- Anders Alstad, Novari

## Bakgrunn og mål

Dette var det første møtet i en serie arbeidsmøter om arkitektur for pilot 4 i SAMT-BU. Piloten skal legge grunnlaget for innrapportering av enkeltresultater og kvalifikasjonsoppnåelse (vitnemål) til semantisk og teknisk arkitektur.

«Drømmereisen» som er styrende for arbeidet:

- Alle vitnemål og enkeltresultater fra opplæring samles i Kompetanseregisteret og er tilgjengelige gjennom hele livsløpet.
- Data fra kommuners og fylkeskommuners systemer leveres til Kompetanseregisteret slik at resultater kan deles og ivaretas sikkert.
- Digitale resultater skal være juridisk likestilt med papirdokumentasjon.
- Offentlige etater og private virksomheter kan hente utdanningsresultater enkelt – med hjemmel eller samtykke fra innbyggeren.
- Innbyggeren får alle resultater og vitnemål samlet på ett sted, delbart via Vitnemålsportalen.

Overgangen fra papirdokumentasjon til digitale bevis skal gi innbyggeren full kontroll over egen informasjon, gjøre det mulig for konsumenter (arbeidsgivere, offentlig sektor med flere) å digitalisere og automatisere prosesser, og motvirke svindel.

## Datamodell

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

## Arkitektur

Martin presenterte teknisk arkitektur sentrert rundt FS GraphQL API. Skissen ble framstilt som Sikts foreløpige tanker på et veldig overordnet nivå, og det ble understreket at detaljene må avklares gjennom prosjektet. I skissen ser man:

- Skolesystemene, med hvilke data Sikt trenger: enkeltresultater, kvalifikasjoner, fødselsnummer for resultateier og fødselsnummer for brukeren som leverer data (for audit).
- Autentisering mot FS GraphQL API gjennom Maskinporten og Feide, eventuelt ID-porten via Feide.
- Registrene for informasjon om, oppbevaring av og deling av utdanning og kompetansebevis, henholdsvis Utdanningsregisteret, Kompetanseregisteret og Vitnemålsportalen.
- Koblinger til internasjonale integrasjoner som EMREX, OOTS/SDG og EUDI Wallet.

I diskusjonen kom det fram at en del av dataene vi ønsker å lagre, stammer fra Grep og Vigos kodeverk, og ikke fra skolesystemene selv. Dette må følges opp når vi går nærmere inn på datamodellen. Det ble også snakket om hvordan skolesystemene i dag leverer vitnemål i kommunene og fylkeskommunene (Vigo og FINT), og at dette mellomleddet kan påvirke hvordan arkitekturen til slutt utformes. Hvordan disse leveranseløpene skal håndteres i den nye arkitekturen, er et åpent spørsmål prosjektet må ta tak i.

Sikt ga også en kort gjennomgang av hvorfor de har satset så tungt på GraphQL og FS GraphQL API. En konsekvens av dette valget er at integrasjonsparter må støtte og forstå GraphQL, eller alternativt at Sikt må tilby en REST-til-FS-GraphQL-kobling for dem som ikke kan integrere direkte. Hvilken modell som er aktuell for hvilke parter, må avklares videre i prosjektet.

## Veien videre

- Vurdere modeller brukt i grunn- og videregående skoler.
- Arbeide videre med forslag til hvordan innrapportering bør implementeres.
- Neste møte i arbeidsgruppen er tirsdag 2. juni kl. 14.00–15.30.
