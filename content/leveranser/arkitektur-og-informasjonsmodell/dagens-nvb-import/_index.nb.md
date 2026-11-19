---
id: 70eb49eb-f182-4c2e-afe5-060d4e352084
title: "Dagens prosess for NVB-import"
linkTitle: "Dagens NVB-import"
weight: 10
lastmod: 2026-11-20T00:30:48+01:00
last_editor: Erik Hagen

---

## Dagens prosess for NVB-import

For å forstå hva den nye arkitekturen endrer, må dagens prosess beskrives. Dette kapittelet dokumenterer hvordan resultater fra videregående opplæring i dag kommer inn i Nasjonal vitnemålsdatabase (NVB), og hvilke risikomomenter som ligger i denne prosessen. Beskrivelsen bygger på formatspesifikasjonen for NVB-kontrollmotoren [5], gjennomgangene i arbeidsmøteserien [12] og NVBs egen dokumentasjon [6].

### Aktører og dataflyt

NVB forvaltes av Sikt og er i dag kilden til sannhet for vitnemål og kompetansebevis fra videregående opplæring, primært brukt av Samordna opptak ved opptak til høyere utdanning [6].

Dataflyten inn til NVB går slik (figur 1):

1. **Skolen registrerer sluttvurderinger i det skoleadministrative systemet.** For offentlige videregående skoler er dette InSchool; private skoler bruker blant annet iSkole og IST Everyday [17]. Læreren setter standpunktkarakter, og eksamenskarakterer kommer til etter sensur.
2. **Det skoleadministrative systemet genererer vitnemåls- og kompetansebevisdata** som en tegnseparert tekstfil («soltegnfil», se [Filformatet – soltegnfilene](#filformatet--soltegnfilene)) etter NVB-spesifikasjonen [5].
3. **Fagopplæringen går via VIGO.** Fagprøver registreres i VIGO, som produserer endelig vitnemål for lærlinger og kandidater i fagopplæringen. VIGO eksporterer en fil i .nvb-format som senere lastes inn i NVB igjen [12: arbeidsmøte 3 og 7].
4. **Filen valideres før innsending** (se [Validering og kontroll i dag](#validering-og-kontroll-i-dag) om hvordan valideringen skjer i dag).
5. **NVB tar imot dokumenter som validerer uten feil.** Dokumenter med feilmeldinger avvises og returneres for korreksjon [5]. NVB tar ikke imot vitnemål som ikke validerer [12: arbeidsmøte 8].
6. **Skoleeier utsteder og signerer vitnemålet på papir.** Det finnes i dag ingen digital signering av vitnemål – vitnemålet skrives ut og signeres av rektor, i tillegg til at dataene sendes inn og valideres [12: arbeidsmøte 8].

Grunnskolen er ikke en del av denne flyten. Grunnskolevitnemål lagres som PDF i elevmappen i kommunens saks- og arkivsystem, og det finnes i dag ingen måte å hente ut dataene på [12: arbeidsmøte 4 og 6]. Se [Grunnskole](../grunnskole/).

![Dagens dataflyt inn til Nasjonal vitnemålsdatabase: videregående skole via InSchool og fagopplæring via Vigo leverer ¤-fil (.nvb) som valideres i valideringstjenesten (ODM) og sendes inn til NVB, som avviser dokumenter som ikke validerer; Samordna opptak bruker NVB. Grunnskolens vitnemål ligger som PDF i saks- og arkivsystemet og inngår ikke i flyten.](dagens-dataflyt-nvb.png)

*Figur 1: Dagens dataflyt inn til Nasjonal vitnemålsdatabase.*

### Filformatet – soltegnfilene

Utvekslingsformatet er en linjebasert tekstfil der feltene skilles med soltegn (¤), og der radtypen avgjøres av en typebokstav i starten av linjen [5]:

| Radtype | Innhold | Motsvar i NVBs datamodell |
|---|---|---|
| ¤A | Startlinje per filseksjon: organisasjonsnummer, skolenummer (VIGO), tellere | Filimport |
| ¤S | Skoleinformasjon (valgfri): navn, adresse, kontaktperson | Enhetsregister |
| ¤V | Dokumenthode for vitnemål eller kompetansebevis: dokumentidentifikator, dokumenttype (VGDOKTYPEKODE: VM = vitnemål, KB = kompetansebevis), fødselsnummer, navn som fritekst, avgangsår, reform (KL/R94) | VGDOK |
| ¤P | Én linje per programområde (normalt tre for et vitnemål): programområdekode, nivåkode (VG1–VG3), påstandskode (Bestått / Fullført / Fullført og bestått) | VGDOKPROMR |
| ¤F | Én linje per fag: fagkode (Grep), fagtype (FF/FP/VP), standpunkt- og eksamenskarakter, omfang, eksamenstermin, fagmerknad | VGDOKFAG |
| ¤M | Merknader til dokument eller enkeltfag, med VMM-merknadskoder fra Grep | VGDOKMERKNAD |
| ¤D | Annullering av tidligere utstedt dokument, med årsakskode | – |

Formatet er posisjonsbasert: feltenes betydning avgjøres av kolonneposisjon i linjen, ikke av feltnavn. Filen er i praksis fire ulike radskjemaer multiplekset inn i én strøm, der en delt nøkkel (vitnemålsnummeret) knytter fag- og merknadslinjene til dokumenthodet. Dokumenttypen styres av en kodeverdi (VGDOKTYPEKODE) satt av leverandøren – vitnemål og kompetansebevis er rader i samme tabell, skilt kun ved kodeverdi. Dette representasjonsvalget er kjernen i det semantiske skiftet til KREG, se [Semantisk arkitektur](../semantisk-arkitektur-og-begrepskatalog/#semantisk-arkitektur).

Sikt ønsker å fase ut soltegnfilene som format og å fjerne filinnsendingen til NVB i sin helhet; hvordan utfasingen skal skje, er beskrevet i [Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur) (sameksistens og utfasing). Digdirs arkitekturprinsipp 4, anbefaling 4.5, advarer nettopp mot innlåsing av data i systemer med egne formater [2].

### Validering og kontroll i dag

Valideringen kontrollerer at fagsammensetningen i et dokument oppfyller kravene til vitnemål, blant annet kravene til generell studiekompetanse, mot kodeverk fra Grep, Udirs føringsskriv og rundskrivet om fag- og timefordeling [5][10]. Kontrollen brukes også underveis: fra høsten i siste skoleår kjøres den for å sjekke at vitnemålet vil validere, noe som er viktig for å gi elevene trygghet for at de får vitnemålet sitt til slutt [12: arbeidsmøte 6].

Historisk ble dette gjort med kontroll.exe, en binær kommandolinjeapplikasjon distribuert per operativsystem, kjørt lokalt eller mot sentral server hos Samordna opptak [5]. Kontroll.exe er nå avviklet og erstattet av Sikts valideringstjeneste basert på IBM ODM. I InSchool er valideringen bygget inn i selve vitnemålsgenereringen (fag mot vitnemålsnummer) og brukes for alle vitnemål; I VIGO brukes kontrollmotor kun for fagprøvene når et vitnemål er klar. Kravene til sluttvurdering og dokumentasjon følger av forskrift til opplæringslova; Udir forvalter føringsskrivet som operasjonaliserer føringen av vitnemål og kompetansebevis (et utkast til føringsforskrift er til behandling i Kunnskapsdepartementet), og skoleeier har ansvaret for utstedelsen. Kompetansen på kontrollene sitter i dag likevel implisitt hos Sikt gjennom ODM-implementasjonen [12: arbeidsmøte 8].

### Risikomomenter i dagens prosess

Gjennomgangen i arbeidsmøteserien har avdekket følgende risikomomenter ved dagens NVB-import. Disse utgjør en vesentlig del av begrunnelsen for den nye arkitekturen.

**R1 – Innlåsing i proprietært format.** Det posisjonsbaserte ¤-formatet uten feltnavn er skjørt, krever spesialkompetanse hos hver leverandør, og låser data inne i et systemspesifikt format, i strid med Digdirs anbefaling 4.5 [2][5].

**R2 – Registeret er ufullstendig.** NVB tar kun imot dokumentasjon som validerer. Dokumentasjon som ikke validerer – blant annet fra modulstrukturert opplæring for voksne, som utgjør hundrevis til flere tusen bevis i året – finnes bare på papir eller som ikke-validerte dokumenter utenfor registeret [12: arbeidsmøte 4]. Elever som ikke fullfører får heller ikke resultatene sine inn.

**R3 – Manuelle feil i filene.** Når skolene sender filer kan karakterer mangle eller fagmerknader være feilført. Dagens format tillater dessuten kun én fagmerknad per fag [12: arbeidsmøte 3].

**R4 – Batch-forsinkelse i en tidskritisk kjede.** Det er svært kort tid mellom sensurfrist (rundt 19. juni) og kjøring av inntak i begynnelsen av juli. Filbasert overføring gjør at resultatene i praksis ikke kan hentes fra registeret i denne perioden, med manuelt etterarbeid som konsekvens [12: arbeidsmøte 3]. Merk at heller ikke KREG vil kunne betjene inntakskjeden i nær framtid – dagens direkteflyt til inntaket videreføres; risikoen gjelder alle andre konsumenter av resultatene.

**R5 – Rundtur i fagopplæringsflyten.** Hvert transformasjonsledd i rundturen VIGO → .nvb-fil → NVB (jf. [Aktører og dataflyt](#aktører-og-dataflyt)) er en feilkilde, og flyten er ikke fullt kartlagt [12: arbeidsmøte 7].

**R6 – Regelverkskompetanse og forvaltning er skjevfordelt.** Regelverkseier (Udir) og de som forstår og vedlikeholder de implementerte valideringsreglene (Sikt) er ikke samme part (jf. [Validering og kontroll i dag](#validering-og-kontroll-i-dag)), og valideringstjenesten fanger ikke nødvendigvis opp alle regelendringer [12: arbeidsmøte 8].

**R7 – Strukturerte grunnskoleresultater bevares ikke.** Strukturerte resultater finnes kun midlertidig i skolesystemet og går tapt som struktur når vitnemålet arkiveres som PDF; det finnes ingen nasjonal tilgang, og gjenbruk krever manuelle leveranser fra hver kommune [12: arbeidsmøte 2 og 4].

**R8 – Svak semantikk for person og innhold.** Personnavn føres som fritekst i dokumenthodet, og feltbetydning avgjøres av posisjon. Formatet gir ingen strukturell garanti for at innholdet tolkes likt av avsender og mottaker [5][15].

**R9 – Leverandør- og teknologibinding i kontrolleddet.** Kontroll.exe var en lukket binær; overgangen til ODM har redusert, men ikke fjernet, bindingen. Digdirs anbefaling 5.6 peker på å unngå bindinger til enkeltleverandører [3]. Se anbefalingen om maskinlesbare valideringsregler i [Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur).

**R10 – Papir er eneste juridisk gyldige form.** Skoleeiers papirsignatur er det som gjør vitnemålet gyldig. Digitale resultater er ikke juridisk likestilt, og dette begrenser verdien av registeret som kilde. Følges opp i arbeidspakke 3 [12: arbeidsmøte 8][13].

**R11 – Data forsvinner når virksomheter opphører.** For private fagskoler forsvinner dokumentasjon i dag når skolen går konkurs. Dette illustrerer risikoen ved desentral lagring av dokumentasjon generelt, og er en del av begrunnelsen for et sentralt register gjennom hele livsløpet. Private fagskoler ligger utenfor Novaris ansvar; oppfølgingen av disse må plasseres hos Sikt eller annen part [12: arbeidsmøte 8].

**R12 – Endring og annullering er svakt støttet.** Standpunkt- og eksamenskarakterer er enkeltvedtak med klagerett; ved medhold endres karakteren og nytt dokument utstedes. I dag håndteres dette gjennom annulleringslinjer (¤D) og ny innsending. Livssyklusen endret sluttvurdering → annullert/erstattet dokument må håndteres eksplisitt i KREG-modellen; dette står som uavklart punkt i mappingen i [Mapping NVB → KREG](../semantisk-arkitektur-og-begrepskatalog/#mapping-nvb--kreg).
