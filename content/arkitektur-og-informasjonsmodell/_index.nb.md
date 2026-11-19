---
id: 4dc9e99d-9c6b-4913-9f9c-0ea66512f34e
title: "Arkitektur og informasjonsmodell"
linkTitle: "Arkitektur og informasjonsmodell"
weight: 30
lastmod: 2026-11-19T21:36:39+01:00
last_editor: Erik Hagen

---

*Siden gjengir pilotens rapport «Arkitektur og informasjonsmodell – SAMT-BU Pilot 4. Dataflyt av resultater fra videregående opplæring til KREG», 30. august 2026 (leveranse fra arbeidspakke 2).*

Rapporten er pilotens leveranse på arbeidspakke 2, datert 30. august 2026. Den er utarbeidet gjennom arbeidsmøteserien om arkitektur (arbeidsmøte 1–8, 22. mai – 28. august 2026), og er fordelt på denne siden og undersidene under.

## 1. Sammendrag

Piloten anbefaler at resultater fra videregående opplæring leveres gjennom direkte integrasjon mot Kompetanseregisteret (KREG), ikke via FINT (samarbeidsgruppas vedtak 28. august 2026; formell beslutning hos systemeierne). Validering og innsending skilles som to separate konsepter: KREG skal ta imot 100 % av dokumentasjonen – også den som ikke validerer – med eksplisitt kvalitetstilstand, mens valideringstjenesten tilbys som egen komponent. Dagens filinnsending til NVB, med det posisjonsbaserte soltegnformatet, ønskes faset ut i sin helhet.

Rapporten dokumenterer dagens NVB-importprosess med tolv identifiserte risikomomenter ([kapittel 5](dagens-nvb-import/)), en anbefalt arkitektur og informasjonsmodell orientert mot European Learning Model v3 ([kapittel 8](maalbilde-og-anbefalt-arkitektur/) og [11](semantisk-arkitektur-og-begrepskatalog/)), avdekkede behov for grunnskolen ([kapittel 10](grunnskole/)), en vurdering mot Digdirs arkitekturprinsipper ([kapittel 9](maalbilde-og-anbefalt-arkitektur/)) og en anbefaling med veien videre for å starte realiseringen med Novari ([kapittel 13](konklusjon-og-veien-videre/)). De tre viktigste forutsetningene for realisering er finansiering og prioritering hos Novari og fylkeskommunene (SAMT-BU-midler kan potensielt benyttes inn i 2028), at KREG gjøres klart for mottak (Sikts vurdering: et ettårsperspektiv), og juridiske avklaringer – hjemmel for grunnskoledata, behandlingsgrunnlag for ikke-beståtte resultater og likestilling av digitale vitnemål med papirvitnemål.

## 2. Innledning

Dette dokumentet er pilotens leveranse på arbeidspakke 2: arkitektur og informasjonsmodell som grunnlag for dataflyt av vitnemål og resultater fra videregående opplæring inn i Kompetanseregisteret (KREG). Navnet på Kompetanseregisteret er ennå ikke bestemt, og i dette dokumentet refererer vi til det som KREG.

Rapporten er utarbeidet gjennom arbeidsmøteserien om arkitektur (arbeidsmøte 1–8, 22. mai – 28. august 2026) [12] og utgjør pilotens milepæl «arkitektur og informasjonsmodell er ferdigstilt» [13]. Arbeidet er forankret i SAMT-BUs case «Tilgjengeliggjøring av resultater fra opplæring» [13], med de konkrete brukersituasjonene skolebytte, opptak, videre oppfølging av unge og voksnes dokumentasjonsbehov som bakteppe. Piloten leverer forarbeid, ikke tekniske produkter [13]; realiseringen av arkitekturen skjer etter pilotperioden, slik veien videre i [kapittel 13](konklusjon-og-veien-videre/) beskriver.

Rapporten dokumenterer dagens prosess for NVB-import med risikomomentene i den ([kapittel 5](dagens-nvb-import/)), målbildet arbeidet styrer mot ([kapittel 6](maalbilde-og-anbefalt-arkitektur/)), vurderingen av arkitekturalternativene med samarbeidsgruppas anbefaling om direkte integrasjon mot KREG ([kapittel 7–9](maalbilde-og-anbefalt-arkitektur/)), avdekkede behov for grunnskolen ([kapittel 10](grunnskole/)), den semantiske arkitekturen med overgangen fra NVBs modell til en ELM-orientert modell ([kapittel 11](semantisk-arkitektur-og-begrepskatalog/)), en begrepskatalog ([kapittel 12](semantisk-arkitektur-og-begrepskatalog/)) og konklusjon med veien videre ([kapittel 13](konklusjon-og-veien-videre/)).

Arkitekturvalgene er vurdert opp mot Digitaliseringsdirektoratets overordnede arkitekturprinsipper for digitalisering av offentlig sektor [1] og Rammeverk for digital samhandling [4]; vurderingen er samlet i [kapittel 9](maalbilde-og-anbefalt-arkitektur/), og prinsippene er i tillegg referert der de begrunner konkrete valg.

## 3. Avgrensning

Piloten dekker videregående opplæring først. Følgende er bevisst holdt utenfor piloten:

**Grunnskole:** Del av SAMT-BUs helhet, men inngår ikke i piloten. Avdekkede behov, ansvarsforhold og forutsetningene for et grunnskolespor er samlet i [kapittel 10](grunnskole/), slik at de kan tas videre selv om de ikke løses i piloten [12: arbeidsmøte 8].

**VIGO-modernisering:** Uplanlagt og ikke klar til piloten. Arkitekturen forholder seg til dagens filoverføring som alternativ, og moderniseringen tas inn i målbildet. Dersom SAMT-BU-midler gjøres tilgjengelige, kan en avgrenset modernisering av eksportleddet i VIGO likevel vurderes – dette krever egen prioriteringsbeslutning hos Novari og fylkeskommunene.

**Utdanningsregisteret:** Holder fortsatt på å onboarde fagskole og høyere utdanning. Ikke klart for å ta inn videregående opplæring. Tas inn i målbildet, ikke i piloten; rollen og det egne sporet er beskrevet i [kapittel 9](maalbilde-og-anbefalt-arkitektur/).

**Juridisk gyldighet av digitale vitnemål:** Egen arbeidsstrøm, hører til arbeidspakke 3 (juridiske behov).

**Modulstrukturert opplæring for voksne (MVOV):** Leveres i dag papirbasert/ikke-validert, med høyt volum. Prinsippet om at sluttvurdering er enheten (kapittel 4) gjelder også MVOV, men selve flyten løses ikke i piloten.

Sikt har erstattet valideringsmotoren «kontroll.exe» med en ny valideringsmotor basert på IBM Operational Decision Manager (ODM). Felles maskinlesbare valideringsregler basert på eller som input til ODM inngår ikke i piloten, men er dokumentert som anbefaling i [kapittel 9](maalbilde-og-anbefalt-arkitektur/).

## 4. Premisser for piloten

Følgende legges til grunn for arkitekturarbeidet og behandles ikke som åpne spørsmål videre:

1. **Sluttvurderingen i det enkelte fag er den atomære enheten som skal inn i KREG.** Sluttvurdering omfatter standpunktkarakter, eksamenskarakter og karakter til fag-, svenne- og kompetanseprøve, jf. forskrift til opplæringslova; også karakterer som ikke er bestått, er sluttvurderinger. I KREG er hver karakter én forekomst av vurdering.
2. **Kompetansebevis er en sammensetning av sluttvurderinger.** Vitnemål er en sammensetning av sluttvurderinger som representerer en fullført utdanning, som eksempelvis kvalifiserer til generell studiekompetanse (GSK). Presisering fra arbeidsmøte 6: et kompetansebevis fra videregående opplæring kan etter føringsskrivet [10] også inneholde halvårsvurderinger med karakter, og oppnådde kompetansemål kan dokumenteres i vedlegg. Premisset gjelder derfor det som skal inn i KREG i piloten, ikke alt som kan stå på et utstedt kompetansebevis [12: arbeidsmøte 6].
3. **Sluttvurderinger sendes inn i KREG fortløpende.** Arbeidshypotesen fra arbeidsmøte 4 var at kun beståtte sluttvurderinger skulle inn; arbeidsmøte 8 presiserte scopet: pilotens mål er teknisk overføring av dokumentasjonen uavhengig av om den er validert eller kontrollert manuelt – målet er å få inn 100 % av dokumentasjonen, også sluttvurderinger og kompetansebevis for elever som ikke har fullført eller bestått [12: arbeidsmøte 4 og 8].
4. **Modulkoder skal ikke inn i KREG – kun sluttvurderinger.** Modulkoder kan stå på kompetansebevis som blir utstedt, men ikke på vitnemål, jf. Udirs føringsskriv [10][12: arbeidsmøte 8].
5. **Læringsmål og oppnådde kompetansemål skal ikke inngå i selve piloten.** De kan tas med som uavklart del av målbildet [12: arbeidsmøte 6 og 8].

## Resten av rapporten

- [Dagens prosess for NVB-import](dagens-nvb-import/) – kapittel 5, med de tolv risikomomentene
- [Målbilde og anbefalt arkitektur](maalbilde-og-anbefalt-arkitektur/) – kapittel 6–9: målbilde, hva SAMT-BU skal løse, arkitekturskisser og teknisk målarkitektur
- [Grunnskole](grunnskole/) – kapittel 10
- [Semantisk arkitektur og begrepskatalog](semantisk-arkitektur-og-begrepskatalog/) – kapittel 11 og 12, og vedleggene med informasjonsmodellene for KREG og NVB2
- [Konklusjon og veien videre](konklusjon-og-veien-videre/) – kapittel 13
- [Arbeidsmøter](arbeidsmoter/) – arbeidsmøteserien om arkitektur som rapporten bygger på

## 14. Kilder

1. Digitaliseringsdirektoratet: Overordnede arkitekturprinsipper for digitalisering av offentlig sektor. <https://www.digdir.no/digital-samhandling/overordnede-arkitekturprinsipper/1065>
2. Digitaliseringsdirektoratet: Prinsipp 4 – Del og gjenbruk data. <https://www.digdir.no/digital-samhandling/prinsipp-4-del-og-gjenbruk-data/1061>
3. Digitaliseringsdirektoratet: Prinsipp 5 – Del og gjenbruk løsninger. <https://www.digdir.no/digital-samhandling/prinsipp-5-del-og-gjenbruk-losninger/1062>
4. Digitaliseringsdirektoratet: Rammeverk for digital samhandling. <https://www.digdir.no/digital-samhandling/rammeverk-digital-samhandling/2148>
5. Samordna opptak / Sikt: NVB – Kontrollmotor kontroll.exe og filformat (januar 2024). <https://nasjonal-vitnemalsdatabase.no/SO-NVB-kontrollmotor-202401.pdf>
6. Nasjonal vitnemålsdatabase. <https://nasjonal-vitnemalsdatabase.no/>
7. Novari IKS: FINT informasjonsmodell, pakken utdanning, v4.1.0. <https://informasjonsmodell.felleskomponent.no/docs/package_utdanning?v=v4.1.0>
8. European Commission: European Learning Model (ELM) v3. <https://europass.europa.eu/en/european-learning-model-stakeholders> og <https://github.com/european-commission-empl/European-Learning-Model>
9. Digitaliseringsdirektoratet: SKOS-AP-NO-Begrep – forvaltningsstandard for begrepsbeskrivelser. <https://data.norge.no/specification/skos-ap-no-begrep>
10. Utdanningsdirektoratet: Føring av vitnemål og kompetansebevis for videregående opplæring i Kunnskapsløftet – 2026. <https://www.udir.no/eksamen-og-prover/vitnemal-og-kompetansebevis/foring-vitnemal-kompetansebevis-vgs-26/>
11. 1EdTech: OneRoster 1.2 Norwegian Profile. <https://www.imsglobal.org/spec/or-nor/v1p0>
12. SAMT-BU Pilot 4: Referater fra arbeidsmøter om arkitektur 1–8 (22. mai, 2. juni, 12. juni, 19. juni, 26. juni, 14. august, 21. august og 28. august 2026). Prosjektdokumenter.
13. SAMT-BU: Plan for gjennomføring av pilot 4, v0.1, 15.04.2026. Prosjektdokument. Casebeskrivelse: <https://docs.samt-bu.no/behov/use-cases/01-resultater-vgo/>
14. Sikt: Informasjonsmodell KREG (ER-diagram). [Vedlegg A](semantisk-arkitektur-og-begrepskatalog/).
15. Sikt: Informasjonsmodell NVB2 (ER-diagram). [Vedlegg B](semantisk-arkitektur-og-begrepskatalog/).
16. Sikt: Informasjonsmodell Utdanningsregisteret (ER-diagram). Delt i arbeidsmøteserien.
17. SAMT-BU AP4: Oversikt skoleadministrative systemer. Prosjektdokument.
