---
id: 4dc9e99d-9c6b-4913-9f9c-0ea66512f34e
title: "Architecture and information model"
linkTitle: "Architecture and information model"
weight: 30
lastmod: 2026-11-19T21:53:02+01:00
last_editor: Erik Hagen

---

*This page reproduces the pilot's report «Arkitektur og informasjonsmodell – SAMT-BU Pilot 4. Dataflyt av resultater fra videregående opplæring til KREG» (Architecture and information model – SAMT-BU Pilot 4. Data flow of results from upper secondary education to KREG), 30 August 2026 (deliverable from work package 2).*

The report is the pilot's deliverable for work package 2, dated 30 August 2026. It was produced through the series of working meetings on architecture (working meetings 1–8, 22 May – 28 August 2026), and is divided between this page and the subpages below.

## Summary

The pilot recommends that results from upper secondary education be delivered through direct integration with the competence register Kompetanseregisteret (KREG), not via FINT (decision of the collaboration group on 28 August 2026; the formal decision rests with the system owners). Validation and submission are separated into two distinct concepts: KREG is to receive 100 % of the documentation – including documentation that does not validate – with an explicit quality state, while the validation service is offered as a separate component. Today's file submission to NVB, with its position-based «soltegn» format (fields separated by the ¤ character), is to be phased out entirely.

The report documents today's NVB import process with twelve identified risk factors ([Risk factors in today's process](dagens-nvb-import/#risk-factors-in-todays-process)), a recommended architecture and information model oriented towards European Learning Model v3 ([Architecture sketches for the pilot](maalbilde-og-anbefalt-arkitektur/#architecture-sketches-for-the-pilot) and [Semantic architecture](semantisk-arkitektur-og-begrepskatalog/#semantic-architecture)), identified needs for primary and lower secondary school ([Primary and lower secondary school](grunnskole/)), an assessment against Digdir's architecture principles ([Assessment against Digdir's overarching architecture principles](maalbilde-og-anbefalt-arkitektur/#assessment-against-digdirs-overarching-architecture-principles)) and a recommendation with the way forward for starting the implementation with Novari ([Conclusion and the way forward](konklusjon-og-veien-videre/)). The three most important prerequisites for implementation are funding and prioritisation at Novari and the county authorities (SAMT-BU funds could potentially be used into 2028), that KREG is made ready to receive data (Sikt's estimate: a one-year horizon), and legal clarifications – a legal basis for primary school data, a basis for processing results that are not passed, and legal equivalence between digital diplomas and paper diplomas.

## Introduction

This document is the pilot's deliverable for work package 2: architecture and information model as the basis for the data flow of diplomas (vitnemål) and results from upper secondary education into the competence register Kompetanseregisteret (KREG). The name of the register has not yet been decided, and in this document we refer to it as KREG.

The report was produced through the series of working meetings on architecture (working meetings 1–8, 22 May – 28 August 2026) [12] and constitutes the pilot's milestone «architecture and information model completed» [13]. The work is anchored in the SAMT-BU case «Making results from education available» [13], with the concrete user situations of changing schools, admission, further follow-up of young people, and adults' need for documentation as the backdrop. The pilot delivers preparatory work, not technical products [13]; the architecture is implemented after the pilot period, as [The way forward after 1 September](konklusjon-og-veien-videre/#the-way-forward-after-1-september) describes.

The report documents today's process for NVB import and its risk factors ([Today's process for NVB import](dagens-nvb-import/)), the target picture the work is aiming for ([Target picture](maalbilde-og-anbefalt-arkitektur/#target-picture)), the assessment of the architecture alternatives with the collaboration group's recommendation of direct integration with KREG ([What are we to solve in SAMT-BU?](maalbilde-og-anbefalt-arkitektur/#what-are-we-to-solve-in-samt-bu), [Architecture sketches for the pilot](maalbilde-og-anbefalt-arkitektur/#architecture-sketches-for-the-pilot) and [Technical target architecture](maalbilde-og-anbefalt-arkitektur/#technical-target-architecture)), identified needs for primary and lower secondary school ([Primary and lower secondary school](grunnskole/)), the semantic architecture with the transition from NVB's model to an ELM-oriented model ([Semantic architecture](semantisk-arkitektur-og-begrepskatalog/#semantic-architecture)), a concept catalogue ([Concept catalogue](semantisk-arkitektur-og-begrepskatalog/#concept-catalogue)) and a conclusion with the way forward ([Conclusion and the way forward](konklusjon-og-veien-videre/)).

The architecture choices have been assessed against the Norwegian Digitalisation Agency's (Digdir) overarching architecture principles for digitalisation of the public sector [1] and the Framework for Digital Interoperability (Rammeverk for digital samhandling) [4]; the assessment is collected in [Assessment against Digdir's overarching architecture principles](maalbilde-og-anbefalt-arkitektur/#assessment-against-digdirs-overarching-architecture-principles), and the principles are also referred to where they justify specific choices.

## Scope

The pilot covers upper secondary education first. The following has deliberately been kept outside the pilot:

**Primary and lower secondary school (grunnskole):** Part of SAMT-BU as a whole, but not included in the pilot. Identified needs, responsibilities and the prerequisites for a primary school track are collected under [Primary and lower secondary school](grunnskole/), so that they can be taken further even though they are not solved in the pilot [12: working meeting 8].

**Modernisation of VIGO:** Unplanned and not ready for the pilot. The architecture treats today's file transfer as an option, and the modernisation is included in the target picture. If SAMT-BU funds are made available, a limited modernisation of the export stage in VIGO may nevertheless be considered – this requires a separate prioritisation decision by Novari and the county authorities.

**Utdanningsregisteret (the education register):** Still onboarding tertiary vocational education (fagskole) and higher education. Not ready to take in upper secondary education. Included in the target picture, not in the pilot; its role and the separate track are described in [Technical target architecture](maalbilde-og-anbefalt-arkitektur/#technical-target-architecture).

**Legal validity of digital diplomas:** A separate workstream, belonging to work package 3 (legal needs).

**Module-structured education for adults (MVOV):** Delivered today on paper/unvalidated, in high volumes. The principle that the final assessment is the unit (see [Premises for the pilot](#premises-for-the-pilot)) also applies to MVOV, but the flow itself is not solved in the pilot.

Sikt has replaced the validation engine «kontroll.exe» with a new validation engine based on IBM Operational Decision Manager (ODM). Common machine-readable validation rules based on, or as input to, ODM are not part of the pilot, but are documented as a recommendation in [Technical target architecture](maalbilde-og-anbefalt-arkitektur/#technical-target-architecture).

## Premises for the pilot

The following is taken as given for the architecture work and is not treated as open questions going forward:

1. **The final assessment (sluttvurdering) in each subject is the atomic unit that is to go into KREG.** Final assessment comprises the overall achievement grade (standpunktkarakter), the examination grade (eksamenskarakter) and the grade for the trade, journeyman's and competence examination (fag-, svenne- og kompetanseprøve), cf. the regulations to the Education Act (forskrift til opplæringslova); grades that are not passed are also final assessments. In KREG each grade is one instance of assessment (vurdering).
2. **A certificate of competence (kompetansebevis) is a composition of final assessments.** A diploma (vitnemål) is a composition of final assessments that represents a completed education, which for example qualifies for general university admissions certification (GSK). Clarification from working meeting 6: according to the guidelines for completing diplomas (føringsskrivet) [10], a certificate of competence from upper secondary education may also contain half-year assessments with grades, and achieved competence aims may be documented in an attachment. The premise therefore applies to what is to go into KREG in the pilot, not to everything that may appear on an issued certificate of competence [12: working meeting 6].
3. **Final assessments are submitted to KREG continuously.** The working hypothesis from working meeting 4 was that only passed final assessments should go in; working meeting 8 clarified the scope: the pilot's goal is the technical transfer of the documentation regardless of whether it has been validated or checked manually – the goal is to get 100 % of the documentation in, including final assessments and certificates of competence for pupils who have not completed or passed [12: working meetings 4 and 8].
4. **Module codes are not to go into KREG – only final assessments.** Module codes may appear on certificates of competence that are issued, but not on diplomas, cf. Udir's guidelines [10][12: working meeting 8].
5. **Learning outcomes and achieved competence aims are not to be part of the pilot itself.** They may be included as an unresolved part of the target picture [12: working meetings 6 and 8].

## The rest of the report

- [Today's NVB import process](dagens-nvb-import/) – with the twelve risk factors
- [Target picture and recommended architecture](maalbilde-og-anbefalt-arkitektur/) – target picture, what SAMT-BU is to solve, architecture sketches and technical target architecture
- [Primary and lower secondary school](grunnskole/) – identified needs and prerequisites for a primary school track
- [Semantic architecture and concept catalogue](semantisk-arkitektur-og-begrepskatalog/) – semantic architecture, concept catalogue and the appendices with the information models for KREG and NVB2
- [Conclusion and the way forward](konklusjon-og-veien-videre/) – the most important points and the way forward
- [Working meetings](arbeidsmoter/) – the series of working meetings on architecture that the report builds on

## Sources

1. Digitaliseringsdirektoratet: Overordnede arkitekturprinsipper for digitalisering av offentlig sektor (Overarching architecture principles for digitalisation of the public sector). <https://www.digdir.no/digital-samhandling/overordnede-arkitekturprinsipper/1065>
2. Digitaliseringsdirektoratet: Prinsipp 4 – Del og gjenbruk data (Principle 4 – Share and reuse data). <https://www.digdir.no/digital-samhandling/prinsipp-4-del-og-gjenbruk-data/1061>
3. Digitaliseringsdirektoratet: Prinsipp 5 – Del og gjenbruk løsninger (Principle 5 – Share and reuse solutions). <https://www.digdir.no/digital-samhandling/prinsipp-5-del-og-gjenbruk-losninger/1062>
4. Digitaliseringsdirektoratet: Rammeverk for digital samhandling (Framework for Digital Interoperability). <https://www.digdir.no/digital-samhandling/rammeverk-digital-samhandling/2148>
5. Samordna opptak / Sikt: NVB – Kontrollmotor kontroll.exe og filformat (NVB – the kontroll.exe control engine and file format) (January 2024). <https://nasjonal-vitnemalsdatabase.no/SO-NVB-kontrollmotor-202401.pdf>
6. Nasjonal vitnemålsdatabase (National Diploma Database). <https://nasjonal-vitnemalsdatabase.no/>
7. Novari IKS: FINT informasjonsmodell, pakken utdanning (FINT information model, the education package), v4.1.0. <https://informasjonsmodell.felleskomponent.no/docs/package_utdanning?v=v4.1.0>
8. European Commission: European Learning Model (ELM) v3. <https://europass.europa.eu/en/european-learning-model-stakeholders> and <https://github.com/european-commission-empl/European-Learning-Model>
9. Digitaliseringsdirektoratet: SKOS-AP-NO-Begrep – forvaltningsstandard for begrepsbeskrivelser (administrative standard for concept descriptions). <https://data.norge.no/specification/skos-ap-no-begrep>
10. Utdanningsdirektoratet: Føring av vitnemål og kompetansebevis for videregående opplæring i Kunnskapsløftet – 2026 (Completing diplomas and certificates of competence for upper secondary education under the Knowledge Promotion reform – 2026). <https://www.udir.no/eksamen-og-prover/vitnemal-og-kompetansebevis/foring-vitnemal-kompetansebevis-vgs-26/>
11. 1EdTech: OneRoster 1.2 Norwegian Profile. <https://www.imsglobal.org/spec/or-nor/v1p0>
12. SAMT-BU Pilot 4: Minutes of the working meetings on architecture 1–8 (22 May, 2 June, 12 June, 19 June, 26 June, 14 August, 21 August and 28 August 2026). Project documents.
13. SAMT-BU: Plan for gjennomføring av pilot 4 (Plan for carrying out pilot 4), v0.1, 15.04.2026. Project document. Case description: <https://docs.samt-bu.no/en/behov/use-cases/01-resultater-vgo/>
14. Sikt: Informasjonsmodell KREG (KREG information model, ER diagram). [Appendix A](semantisk-arkitektur-og-begrepskatalog/#appendix-a--information-model-kreg).
15. Sikt: Informasjonsmodell NVB2 (NVB2 information model, ER diagram). [Appendix B](semantisk-arkitektur-og-begrepskatalog/#appendix-b--information-model-nvb2).
16. Sikt: Informasjonsmodell Utdanningsregisteret (Utdanningsregisteret information model, ER diagram). Shared in the series of working meetings.
17. SAMT-BU AP4: Oversikt skoleadministrative systemer (Overview of school administration systems). Project document.
