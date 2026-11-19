---
id: 65c70080-6fbe-4a0e-8495-9f1eed4fde25
title: "Målbilde og anbefalt arkitektur"
linkTitle: "Målbilde og anbefalt arkitektur"
weight: 20
lastmod: 2026-11-19T21:53:02+01:00
last_editor: Erik Hagen

---

## Målbilde

Alle vitnemål og enkeltresultater fra grunnopplæringen er samlet i KREG, som er tilgjengelig gjennom hele livsløpet til borgeren.

Data fra kommuners og fylkeskommuners systemer leveres til KREG, slik at alle resultater kan deles og ivaretas på en sikker måte.

Skolesystemer kan selv validere vitnemål og sluttvurderinger underveis i elevens løp.

Når skole og fylkeskommunene skal utstede vitnemål og kompetansebevis fra videregående opplæring, kobles kompetansebevis i KREG sammen mot et overordnet kompetansebevis som dokumenterer koblingen. Sammensetningen utføres i skolesystemet av skolene, der skoleeier står som utsteder med signaturansvar [12: arbeidsmøte 6 og 8].

Digitale resultater er juridisk likestilt med papirdokumentasjon.

Offentlige etater og private virksomheter kan hente utdanningsresultater enkelt – med hjemmel eller samtykke fra innbyggeren.

Innbyggeren har alle resultater og vitnemål fra utdanning samlet på ett sted som kan deles i Vitnemålsportalen.

Borgere i Norge blir i stand til å bruke utdanningen sin på tvers av landegrenser gjennom EMREX, EU Digital Identity Wallet og Once-Only Technical System.

Utdanningsregisteret inngår i målbildet som grunndataregister for utdanningstilbud, og skal i målbildet få data om videregående opplæring fra Grep, InSchool og VIGO [12: arbeidsmøte 8].

Migrering av historisk dokumentasjon fra kommunale arkiver inngår ikke i målbildet slik det nå er definert.

Datasett, API-er, tjenester, modeller og begreper som etableres, skal registreres i Felles datakatalog på data.norge.no (se [Begrepskatalog](../semantisk-arkitektur-og-begrepskatalog/#begrepskatalog)).

### Uavklart del av målbildet

Oppnådde læringsmål og kompetansemål, samt underveisvurdering, kan inkluderes i kompetansebevis fra videregående opplæring. Disse legges med som vedlegg i dag.

Hvordan modellene passer inn i ELM: hovedtrekkene er beskrevet i [Innplassering i ELM v3](../semantisk-arkitektur-og-begrepskatalog/#innplassering-i-elm-v3), men full innplassering – herunder valg av applikasjonsprofil og kontroll av sammensetningsrelasjonene (hasPart/isPartOf) mot profilens regler – gjenstår [8].

## Hva skal vi løse i SAMT-BU?

Kjernen i piloten er flyten av én sluttvurdering fra kilde til KREG, illustrert i figur 2:

1. **Sluttvurderingen settes i kildesystemet.** Læreren setter standpunktkarakter i InSchool, og eksamenskarakterer kommer til etter sensur; fag- og svenneprøver registreres i VIGO.
2. **Kildesystemet leverer sluttvurderingen til KREG fortløpende.** InSchool leverer direkte via API; VIGO leverer via filoverføring, eventuelt gjennom en mellomvare.
3. **Valideringstjenesten brukes underveis** for å kontrollere fagsammensetningen gjennom opplæringsløpet, og til vitnemålsvalidering når alle sluttvurderinger foreligger.
4. **KREG tar imot dokumentasjonen** med eksplisitt kvalitetstilstand.
5. **Skolen setter sammen og utsteder kvalifikasjonen;** skolesystemet leverer selve kvalifikasjonsutstedelsen, sluttvurderingene som inngår vil allerede være i KREG.
6. **Vitnemålsportalen og konsumentene henter resultatene fra KREG.**

![Flyten av én sluttvurdering: læreren setter sluttvurdering i InSchool, fag- og svenneprøver i Vigo; InSchool leverer fortløpende via API og Vigo via filoverføring eller mellomvare til KREG, som tar imot sluttvurdering og kvalifikasjon; valideringstjenesten (ODM) brukes til fagsammensetning underveis og vitnemålsvalidering; Vitnemålsportalen og konsumenter henter fra KREG.](flyt-sluttvurdering-til-kreg.png)

*Figur 2: Flyten av én sluttvurdering fra kilde til KREG. Stiplede linjer er valideringsstøtte.*

## Arkitekturskisser for piloten

Felles for begge løsninger:

- Validering skjer mot Sikts ODM-baserte valideringstjeneste, som allerede har erstattet kontroll.exe i dagens flyt ([Validering og kontroll i dag](../dagens-nvb-import/#validering-og-kontroll-i-dag)).
- En flyt av typen VIGO/InSchool → FINT → KREG → FINT → VIGO/InSchool kan skape avstand fra sammenhengende tjenester og kan påvirke læreren som sluttvurderer i VIGO/InSchool [12: arbeidsmøte 7].
- Valget må vurderes opp mot Digdirs arkitekturrammeverk for sammenhengende tjenester [4]. Beslutningen ligger hos de som har ansvar for InSchool og VIGO, i tråd med Digdirs prinsipp 2 om å ta arkitekturbeslutninger på rett nivå [1].

### Alternativ 1, levere sluttvurderinger via FINT

Nøkkelpunkt:

- VIGO og InSchool bygges om til å sende sluttvurderinger til FINT fortløpende.
- FINT vil kunne motta data fortløpende på sluttvurderinger. Når noe er sluttvurdert i FINT, overføres det til KREG [12: arbeidsmøte 6].
- Dette mønsteret kan trolig gjenbrukes for grunnskolen.

### Alternativ 2, levere sluttvurderinger direkte i KREG

Nøkkelpunkt:

- InSchool sender sluttvurderinger direkte til KREGs GraphQL-API.
- For VIGO er det to muligheter: bygge mellomvare for å transformere filer fra VIGO til GraphQL API-kall med sluttvurderinger, eller midler til å modernisere denne delen av VIGO for å levere direkte.
- VIGO leverer sluttvurderinger og bestått fagprøve alt-i-ett.

### Vurdering og anbefaling

Arbeidsmøte 7 og 8 gjennomgikk FINTs informasjonsmodell [7] og vurderte de to alternativene. Arbeidsmøtets vurdering var følgende [12: arbeidsmøte 8]:

- FINT-informasjonsmodellen er laget for statistikk og analyse. FINT informasjonsmodell kan per i dag ikke uttrykke alle konsepter nødvendig for å overføre langtidslevende bevis. VIGO og InSchool er kilden til resultatinformasjonen.
- VIGO og InSchool benytter i dag tjenester fra Sikt for validering av vitnemål. Ansvaret for validering av korrektheten i vitnemålene ligger hos skolene.
- Dataflyt gjennom FINT kan potensielt ha noen forsinkelser, noe som kan skape utfordringer i noen tidskritiske situasjoner.
- Slik FINT og FINT-informasjonsmodellen er designet i dag, kan sluttvurderinger potensielt gå via FINT, mens kompetansebevis og vitnemål må komme fra de skoleadministrative systemene.
- Hvis FINT informasjonsmodell skal benyttes som et steg i overføringen til KREG må det antagelig gjøres en jobb for at konseptene i FINT skal matche konseptene i KREG og ELM.

**Vedtak (arbeidsmøte 8, 28. august 2026): Samarbeidsgruppa anbefaler at VIGO og InSchool overfører direkte til KREG, tilsvarende dagens overføringer til NVB.** Den formelle beslutningen ligger hos systemeierne – Novari og fylkeskommunene [12: arbeidsmøte 7].

Autentisering og autorisering i de ulike leddene må avklares videre, jf. Digdirs prinsipp 7 om tillit til oppgaveløsningen [1]. Utgangspunktet fra skissearbeidet er Maskinporten for virksomhetsautentisering og Feide der brukerkontekst trengs, med fødselsnummer for brukeren som leverer data, for sporbarhet [12: arbeidsmøte 1].

![Anbefalt arkitektur: InSchool leverer sluttvurderinger og vitnemålsdata direkte til KREGs GraphQL-API (anbefaling 28.08.2026); Vigo leverer via filoverføring og en mellomvare med uavklart eierskap; valideringstjenesten (ODM) er egen komponent; NVB kjøres parallelt med soltegnfil til den fases ut; Utdanningsregisteret er målbilde og eget spor; Vitnemålsportalen og konsumenter som SO, SSB og Lånekassen henter fra KREG.](anbefalt-arkitektur-direkte-kreg.jpeg)

*Figur 3: Anbefalt arkitektur for piloten – direkte integrasjon mot KREG. Stiplede elementer er uavklart eller målbilde.*

## Teknisk målarkitektur

Arkitekturen for denne piloten er ett steg mot det bredere målbildet for integrasjonsarkitekturen på tvers av skoleadministrativt system – VIGO – FINT – KREG:

Utdanningsregisteret får en sentral rolle i målbildet, men er ikke en del av det som realiseres i pilotperioden. Rollen bør likevel beskrives i arkitekturen nå, slik at den anbefalte arkitekturen ikke bygger seg inn i en blindvei. I målbildet skal Utdanningsregisteret få data om videregående opplæring fra GREP, InSchool og VIGO, og det etableres et eget spor knyttet til koder som opprettes i VIGO og deretter i InSchool, og som må henge sammen med Utdanningsregisteret [12: arbeidsmøte 6 og 8]. Sikt og Udir bør se nærmere på hvilken rolle registeret skal ha. I KREG er entitetene utdanningsspesifikasjon og utdanningsmulighet forberedt for kobling til Utdanningsregisteret (se [Innplassering i ELM v3](../semantisk-arkitektur-og-begrepskatalog/#innplassering-i-elm-v3) og [Vedlegg A – Informasjonsmodell, KREG](../semantisk-arkitektur-og-begrepskatalog/#vedlegg-a--informasjonsmodell-kreg)).

Grunnskole skal etter hvert inn i samme modell – forutsetningene er beskrevet under [Grunnskole](../grunnskole/).

**Validering og innsending brytes opp til to separate konsepter** [12: arbeidsmøte 7 og 8]. Valideringstjenesten skilles ut som egen komponent i arkitekturen. Der kan man sjekke om fagsammensetningen er riktig underveis i opplæringsløpet, og – når alle sluttvurderinger er på plass – validere om det foreligger et gyldig vitnemål. Konsekvensene av skillet:

- KREG skal kunne ta imot dokumentasjon som ikke validerer, så lenge dette er en akseptert tilstand. Skolen skal ha mulighet til å overstyre gyldigheten slik at dokumentasjonen står som gyldig med skolens signatur, selv om den ikke validerer automatisk.
- Ansvaret for validering i VIGO og InSchool ligger hos skolen, ikke hos Sikt. VIGO og InSchool må tilby funksjonalitet for skoleeiers manuelle kontroll av vitnemål som ikke valideres automatisk, men som likevel er gyldige.
- Udir forvalter føringsregelverket (føringsskrivet [10]); det som mangler, er en forvaltningsmodell som sikrer at valideringsreglene holdes i takt med regelverket – valideringstjenesten fanger ikke nødvendigvis opp alle regelendringer.
- Bindingen til IBM ODM som regelmotor aksepteres i piloten fordi implementasjonen allerede finnes og dekker om lag 90 % av tilfellene.

**Maskinlesbare valideringsregler.** Valideringen i ODM er ikke-proprietær i regeluttrykket, og i målarkitekturen er det ønskelig at regelverket – Grep, fag- og timefordelingen og føringsskrivet – kan uttrykkes og kontrolleres på en måte som kan gjenbrukes i flere systemer, for eksempel med SHACL. Behovet for maskinlesbar validering som kan brukes av Sikt, Udir, InSchool og VIGO ble foreslått som egen SAMT-BU-case [12: arbeidsmøte 7]. Antatt innsats og gevinst må vurderes redelig dersom dette meldes inn som behov [12: arbeidsmøte 8]. En felles, systemuavhengig beskrivelse av informasjonsmodellen for vitnemål, som kan brukes av både fagpersoner og valideringsløsninger, vil gjøre det lettere å implementere nye regelmotorer, oppdatere validering ved regelendringer og gi fagpersoner innsikt – i dag er fasit i praksis regelmotoren.

**Erstatte eksisterende prosesser med KREG:** KREG muliggjør nye integrasjonsmønstre som gjør at vi kan løse ut behov på en ny måte som kan spare kostnader i offentlig sektor. Det er behov for hendelser (events) i arkitekturen, slik at systemer kan reagere på endringer fortløpende [12: arbeidsmøte 6].

Utvikling som pågår på KREG gjelder fagskole, grunnstrukturen anses som god, og fagskolens behov antas ikke å legge uheldige føringer for API-ene for videregående opplæring [12: arbeidsmøte 8]. Sikt vurderer at KREG er klar til å ta imot data i et ettårsperspektiv, med forbehold om finansiering og prioritering.

**API-grensesnitt:** KREG eksponerer et GraphQL-API. For parter som ikke støtter GraphQL, kan det vurderes om Sikt skal tilby en REST-kobling [12: arbeidsmøte 1]; endelig grensesnittvalg avklares i API-dialogen mellom Sikt, Novari og systemleverandørene. KREG er også avhengig av kodeverk fra Grep og VIGO-kodeverket [12: arbeidsmøte 1] – sistnevnte er en løpende leveranse fra Novari som må inngå i avtalene.

**Sameksistens og utfasing NVB → KREG:** I parallellperioden innebærer løsningen dobbel leveranse for leverandørene (soltegnfil til NVB og API-leveranse til KREG). Varighet, utfasingskriterier og finansiering av dobbeltdriften må avklares mellom Sikt, Novari og systemleverandørene. Utfasing av filinnsendingen forutsetter at behovene bak dagens løsning er kartlagt av Sikt og Novari, at NVBs konsumenter – primært Samordna opptak – får dekket sine behov av KREG, og en avklaring av om historiske NVB-data skal migreres til KREG eller forvaltes videre som historisk register.

![Komponentskisse for teknisk målarkitektur: skolesystemene (Visma InSchool, Visma Flyt Skole, IST, Vigilo) leverer sluttvurderinger, kvalifikasjoner, fødselsnummer for resultateier og fødselsnummer til bruker som leverer data (for audit) til FS GraphQL API, med autentisering via Maskinporten, Feide og ID-porten; frontendene Min kompetanse og FS Admin bruker samme API; Kompetanseregisteret er koblet til Utdanningsregisteret, til validering av vitnemål (Grep API og ODM) og til Vitnemålsportalen, som har internasjonale integrasjoner mot Emrex, Single Digital Gateway / Once only technical system og EUDI Wallet.](teknisk-maalarkitektur.png)

*Figuren har ikke nummer eller bildetekst i rapporten.*

### Vurdering mot Digdirs overordnede arkitekturprinsipper

Digitaliseringsdirektoratets syv overordnede arkitekturprinsipper er obligatoriske for statlige virksomheter og anbefalt for kommunal sektor [1]. Tabellen oppsummerer hvordan arkitekturen i dette dokumentet svarer på hvert prinsipp.

| Prinsipp | Hvordan arkitekturen svarer |
|---|---|
| 1. Ta utgangspunkt i brukernes behov | Flytvalget er vurdert ut fra konsekvensen for eleven, lærlingen og læreren som sluttvurderer i InSchool/VIGO – en indirekte flyt via FINT ble blant annet forkastet fordi den skaper avstand til brukeren (se [Vurdering og anbefaling](#vurdering-og-anbefaling)). Målbildet gir innbyggeren all dokumentasjon samlet og delbar gjennom hele livsløpet, også for grupper som i dag faller utenfor registeret. |
| 2. Ta arkitekturbeslutninger på rett nivå | Beslutningen om integrasjonsmønster ligger hos dem som har ansvar for InSchool og VIGO; for grunnskolen er kommunene som skoleeiere beslutningsnivået; regelverksansvar ligger hos Udir. Samarbeidsgruppa gir anbefalinger. |
| 3. Bidra til digitaliseringsvennlig regelverk | Piloten skal levere forslag til hva som må hjemles for innsamling av grunnskoledata, der det i dag mangler lovhjemmel (se [Grunnskole](../grunnskole/)), og peker på behovet for leverings-/hentehjemler som følger dataflyten. Juridisk likestilling av digitale vitnemål følges opp i arbeidspakke 3. |
| 4. Del og gjenbruk data | KREG etableres som felles register med definert mottaksmodell, i stedet for innlåsing i soltegnfiler og PDF-arkiver (anbefaling 4.5 [2]). Datasett, API-er, modeller og begreper registreres i Felles datakatalog. |
| 5. Del og gjenbruk løsninger | Utfasing av filinnsendingen reduserer kompleksitet og teknisk gjeld (5.9), og mottaksmønsteret utvikles nå først for fagskole, med gjenbruk for videregående opplæring og på sikt grunnskole som intensjon [3]. Gap: bindingen til ODM som regelmotor består (5.6, jf. [Teknisk målarkitektur](#teknisk-målarkitektur)), og hendelsesbasert arkitektur (5.7) er identifisert som behov, ikke besluttet. |
| 6. Lag digitale løsninger som støtter samhandling | Informasjonsmodellen orienteres mot ELM v3 og dermed europeisk samhandling (EMREX, EU Digital Identity Wallet, Once-Only). FINT følger etter i samme retning [12: arbeidsmøte 7]. Semantisk samhandlingsevne er behandlet som eget arkitekturområde (se [Semantisk arkitektur](../semantisk-arkitektur-og-begrepskatalog/#semantisk-arkitektur)), jf. Rammeverk for digital samhandling [4]. |
| 7. Sørg for tillit til oppgaveløsningen | Autentisering/autorisering avklares per ledd (Maskinporten/Feide), innsendinger spores til bruker, skoleeiers utsteder- og signaturansvar er eksplisitt, og skillet validering/innsending gjør kvalitetstilstanden til hvert dokument synlig i stedet for at avvik holdes utenfor registeret. |

Sett mot Rammeverk for digital samhandlings fire dimensjoner [4] er status:

- semantisk samhandlingsevne dekt i [Semantisk arkitektur](../semantisk-arkitektur-og-begrepskatalog/#semantisk-arkitektur).
- teknisk samhandlingsevne dekkes av arkitekturvalgene i [Arkitekturskisser for piloten](#arkitekturskisser-for-piloten) og [Teknisk målarkitektur](#teknisk-målarkitektur).
- juridisk samhandlingsevne har tre identifiserte gap – hjemmel for grunnskoledata, likestilling av digitale vitnemål som må avklares i AP3.
- organisatorisk samhandlingsevne – mangler forvaltningsmodell for innspill til KREG fra videregående opplæring og grunnskole.
