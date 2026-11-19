---
id: 65c70080-6fbe-4a0e-8495-9f1eed4fde25
title: "Target picture and recommended architecture"
linkTitle: "Target picture and recommended architecture"
weight: 20
lastmod: 2026-11-19T21:36:39+01:00
last_editor: Erik Hagen

---

## 6. Target picture

All diplomas and individual results from primary and secondary education are collected in KREG, which is available throughout the citizen's whole life course.

Data from the systems of municipalities and county authorities is delivered to KREG, so that all results can be shared and safeguarded securely.

School systems can themselves validate diplomas and final assessments along the way during the pupil's education.

When schools and county authorities issue diplomas and certificates of competence from upper secondary education, the certificates of competence in KREG are linked to a higher-level certificate of competence that documents the link. The composition is carried out in the school system by the schools, with the school owner as issuer responsible for the signature [12: working meetings 6 and 8].

Digital results have equal legal standing with paper documentation.

Public agencies and private organisations can retrieve education results easily – with a legal basis or the citizen's consent.

Citizens have all their results and diplomas from education collected in one place, from which they can be shared through Vitnemålsportalen (the Norwegian Diploma Registry portal).

Citizens in Norway are able to use their education across borders through EMREX, the EU Digital Identity Wallet and the Once-Only Technical System.

Utdanningsregisteret (the education register) is part of the target picture as the master data register for education offerings, and in the target picture it is to receive data about upper secondary education from Grep, InSchool and VIGO [12: working meeting 8].

Migration of historical documentation from municipal archives is not part of the target picture as currently defined.

Datasets, APIs, services, models and concepts that are established are to be registered in the National Data Catalogue (Felles datakatalog) at data.norge.no (see [chapter 12](../semantisk-arkitektur-og-begrepskatalog/)).

### 6.1 Unresolved part of the target picture

Achieved learning outcomes and competence aims, as well as formative assessment, may be included in certificates of competence from upper secondary education. Today these are attached as appendices.

How the models fit into ELM: the main features are described in [chapter 11](../semantisk-arkitektur-og-begrepskatalog/), but full placement – including the choice of application profile and checking the composition relations (hasPart/isPartOf) against the profile's rules – remains [8].

## 7. What are we to solve in SAMT-BU?

The core of the pilot is the flow of one final assessment from source to KREG, illustrated in figure 2:

1. **The final assessment is set in the source system.** The teacher sets the overall achievement grade in InSchool, and examination grades are added after marking; trade and journeyman's examinations are registered in VIGO.
2. **The source system delivers the final assessment to KREG continuously.** InSchool delivers directly via API; VIGO delivers via file transfer, possibly through middleware.
3. **The validation service is used along the way** to check the combination of subjects during the course of education, and for diploma validation when all final assessments are available.
4. **KREG receives the documentation** with an explicit quality state.
5. **The school composes and issues the qualification;** the school system delivers the issuing of the qualification itself, while the final assessments it includes will already be in KREG.
6. **Vitnemålsportalen and the consumers retrieve the results from KREG.**

![The flow of one final assessment: the teacher sets the final assessment in InSchool, trade and journeyman's examinations in Vigo; InSchool delivers continuously via API and Vigo via file transfer or middleware to KREG, which receives final assessments and qualifications; the validation service (ODM) is used for checking the subject combination along the way and for diploma validation; Vitnemålsportalen and consumers retrieve from KREG (figure in Norwegian)](flyt-sluttvurdering-til-kreg.png)

*Figure 2: The flow of one final assessment from source to KREG. Dotted lines are validation support.*

## 8. Architecture sketches for the pilot

Common to both solutions:

- Validation is done against Sikt's ODM-based validation service, which has already replaced kontroll.exe in today's flow ([chapter 5](../dagens-nvb-import/)).
- A flow of the type VIGO/InSchool → FINT → KREG → FINT → VIGO/InSchool may create distance from seamless services and may affect the teacher who sets final assessments in VIGO/InSchool [12: working meeting 7].
- The choice must be assessed against Digdir's architecture framework for seamless services [4]. The decision rests with those responsible for InSchool and VIGO, in line with Digdir's principle 2 on taking architecture decisions at the right level [1].

### 8.1 Option 1, delivering final assessments via FINT

Key points:

- VIGO and InSchool are rebuilt to send final assessments to FINT continuously.
- FINT will be able to receive data on final assessments continuously. When something has received a final assessment in FINT, it is transferred to KREG [12: working meeting 6].
- This pattern can probably be reused for primary and lower secondary school.

### 8.2 Option 2, delivering final assessments directly to KREG

Key points:

- InSchool sends final assessments directly to KREG's GraphQL API.
- For VIGO there are two options: build middleware to transform files from VIGO into GraphQL API calls with final assessments, or funds to modernise this part of VIGO so that it delivers directly.
- VIGO delivers final assessments and passed trade examinations all in one.

### 8.3 Assessment and recommendation

Working meetings 7 and 8 reviewed FINT's information model [7] and assessed the two options. The working meeting's assessment was as follows [12: working meeting 8]:

- The FINT information model is designed for statistics and analysis. As of today the FINT information model cannot express all the concepts needed to transfer long-lived credentials. VIGO and InSchool are the source of the result information.
- VIGO and InSchool currently use services from Sikt to validate diplomas. Responsibility for validating the correctness of the diplomas lies with the schools.
- Data flow through FINT may potentially involve some delays, which may create challenges in some time-critical situations.
- As FINT and the FINT information model are designed today, final assessments could potentially go via FINT, while certificates of competence and diplomas must come from the school administration systems.
- If the FINT information model is to be used as a step in the transfer to KREG, work will probably have to be done to make the concepts in FINT match the concepts in KREG and ELM.

**Decision (working meeting 8, 28 August 2026): The collaboration group recommends that VIGO and InSchool transfer directly to KREG, in the same way as today's transfers to NVB.** The formal decision rests with the system owners – Novari and the county authorities [12: working meeting 7].

Authentication and authorisation in the various steps must be clarified further, cf. Digdir's principle 7 on trust in how tasks are carried out [1]. The starting point from the sketch work is Maskinporten for organisation authentication and Feide where user context is needed, with the national identity number of the user delivering data, for traceability [12: working meeting 1].

![Recommended architecture: InSchool delivers final assessments and diploma data directly to KREG's GraphQL API (recommendation of 28.08.2026); Vigo delivers via file transfer and middleware with unresolved ownership; the validation service (ODM) is a separate component; NVB runs in parallel with «soltegn» files until it is phased out; Utdanningsregisteret is target picture and a separate track; Vitnemålsportalen and consumers such as Samordna opptak, Statistics Norway and Lånekassen retrieve from KREG (figure in Norwegian)](anbefalt-arkitektur-direkte-kreg.jpeg)

*Figure 3: Recommended architecture for the pilot – direct integration with KREG. Dotted elements are unresolved or target picture.*

## 9. Technical target architecture

The architecture for this pilot is one step towards the broader target picture for the integration architecture across school administration system – VIGO – FINT – KREG:

Utdanningsregisteret gets a central role in the target picture, but is not part of what is implemented during the pilot period. Its role should nevertheless be described in the architecture now, so that the recommended architecture does not build itself into a dead end. In the target picture, Utdanningsregisteret is to receive data about upper secondary education from GREP, InSchool and VIGO, and a separate track is established for codes that are created in VIGO and then in InSchool, and which must be consistent with Utdanningsregisteret [12: working meetings 6 and 8]. Sikt and Udir should look more closely at what role the register is to have. In KREG, the entities utdanningsspesifikasjon (education specification) and utdanningsmulighet (education opportunity) are prepared for linking to Utdanningsregisteret (see [chapter 11 and appendix A](../semantisk-arkitektur-og-begrepskatalog/)).

Primary and lower secondary school is eventually to be included in the same model – the prerequisites are described in [chapter 10](../grunnskole/).

**Validation and submission are split into two separate concepts** [12: working meetings 7 and 8]. The validation service is separated out as its own component in the architecture. There one can check whether the combination of subjects is correct during the course of education, and – when all final assessments are in place – validate whether there is a valid diploma. The consequences of the separation:

- KREG must be able to receive documentation that does not validate, as long as this is an accepted state. The school must be able to override the validity, so that the documentation stands as valid with the school's signature even though it does not validate automatically.
- Responsibility for validation in VIGO and InSchool lies with the school, not with Sikt. VIGO and InSchool must offer functionality for the school owner's manual checking of diplomas that are not validated automatically but are nevertheless valid.
- Udir manages the rules for completing diplomas (the guidelines, føringsskrivet [10]); what is missing is a governance model ensuring that the validation rules are kept in step with the regulations – the validation service does not necessarily pick up all changes to the rules.
- The dependence on IBM ODM as rule engine is accepted in the pilot because the implementation already exists and covers about 90 % of the cases.

**Machine-readable validation rules.** Validation in ODM is non-proprietary in how the rules are expressed, and in the target architecture it is desirable that the regulations – Grep, the distribution of subjects and teaching hours, and the guidelines for completing diplomas – can be expressed and checked in a way that can be reused across several systems, for example with SHACL. The need for machine-readable validation that can be used by Sikt, Udir, InSchool and VIGO was proposed as a separate SAMT-BU case [12: working meeting 7]. The expected effort and benefit must be assessed honestly if this is submitted as a need [12: working meeting 8]. A common, system-independent description of the information model for diplomas, which can be used both by subject-matter experts and by validation solutions, will make it easier to implement new rule engines, update validation when rules change and give subject-matter experts insight – today the rule engine is in practice the answer key.

**Replacing existing processes with KREG:** KREG enables new integration patterns that allow us to meet needs in a new way that can save costs in the public sector. There is a need for events in the architecture, so that systems can react to changes continuously [12: working meeting 6].

Ongoing development on KREG concerns tertiary vocational education (fagskole); the basic structure is considered good, and the needs of tertiary vocational education are not expected to impose unfortunate constraints on the APIs for upper secondary education [12: working meeting 8]. Sikt estimates that KREG will be ready to receive data within a one-year horizon, subject to funding and prioritisation.

**API interface:** KREG exposes a GraphQL API. For parties that do not support GraphQL, it may be considered whether Sikt should offer a REST connection [12: working meeting 1]; the final choice of interface will be settled in the API dialogue between Sikt, Novari and the system vendors. KREG also depends on code lists from Grep and on the VIGO code list [12: working meeting 1] – the latter is an ongoing delivery from Novari that must be included in the agreements.

**Coexistence and phase-out NVB → KREG:** During the parallel period, the solution means double delivery for the vendors («soltegn» file to NVB and API delivery to KREG). Duration, phase-out criteria and funding of the parallel operation must be clarified between Sikt, Novari and the system vendors. Phasing out file submission requires that the needs behind today's solution have been mapped by Sikt and Novari, that NVB's consumers – primarily Samordna opptak – have their needs met by KREG, and a clarification of whether historical NVB data is to be migrated to KREG or maintained as a historical register.

![Component sketch of the technical target architecture: the school systems (Visma InSchool, Visma Flyt Skole, IST, Vigilo) deliver final assessments, qualifications, the national identity number of the result owner and the national identity number of the user delivering data (for audit) to the FS GraphQL API, with authentication via Maskinporten, Feide and ID-porten; the front ends Min kompetanse and FS Admin use the same API; Kompetanseregisteret is linked to Utdanningsregisteret, to diploma validation (Grep API and ODM) and to Vitnemålsportalen, which has international integrations with Emrex, the Single Digital Gateway / Once-Only Technical System and the EUDI Wallet (figure in Norwegian)](teknisk-maalarkitektur.png)

*The figure has no number or caption in the report.*

### 9.1 Assessment against Digdir's overarching architecture principles

The Norwegian Digitalisation Agency's seven overarching architecture principles are mandatory for central government agencies and recommended for the municipal sector [1]. The table summarises how the architecture in this document responds to each principle.

| Principle | How the architecture responds |
|---|---|
| 1. Start from the users' needs | The choice of flow has been assessed on the basis of the consequences for the pupil, the apprentice and the teacher who sets final assessments in InSchool/VIGO – an indirect flow via FINT was rejected partly because it creates distance to the user (chapter 8). The target picture gives citizens all their documentation collected and shareable throughout their life course, also for groups that currently fall outside the register. |
| 2. Take architecture decisions at the right level | The decision on the integration pattern rests with those responsible for InSchool and VIGO; for primary and lower secondary school, the municipalities as school owners are the decision level; responsibility for the regulations lies with Udir. The collaboration group makes recommendations. |
| 3. Contribute to digitalisation-friendly regulations | The pilot is to deliver a proposal for what needs a legal basis for collecting primary school data, where a statutory basis is currently lacking (chapter 10), and points to the need for legal bases for delivery/retrieval that follow the data flow. Legal equivalence of digital diplomas is followed up in work package 3. |
| 4. Share and reuse data | KREG is established as a common register with a defined receiving model, instead of lock-in to «soltegn» files and PDF archives (recommendation 4.5 [2]). Datasets, APIs, models and concepts are registered in the National Data Catalogue. |
| 5. Share and reuse solutions | Phasing out file submission reduces complexity and technical debt (5.9), and the receiving pattern is now being developed first for tertiary vocational education, with the intention of reuse for upper secondary education and eventually primary and lower secondary school [3]. Gap: the dependence on ODM as rule engine remains (5.6, cf. chapter 9), and event-driven architecture (5.7) has been identified as a need, not decided. |
| 6. Create digital solutions that support interoperability | The information model is oriented towards ELM v3 and thus European interoperability (EMREX, EU Digital Identity Wallet, Once-Only). FINT is following in the same direction [12: working meeting 7]. Semantic interoperability is treated as a separate architecture area (chapter 11), cf. the Framework for Digital Interoperability [4]. |
| 7. Ensure trust in how tasks are carried out | Authentication/authorisation is clarified per step (Maskinporten/Feide), submissions are traced to a user, the school owner's responsibility as issuer and signatory is explicit, and the separation of validation and submission makes the quality state of each document visible instead of keeping deviations outside the register. |

Measured against the four dimensions of the Framework for Digital Interoperability [4], the status is:

- semantic interoperability is covered in [chapter 11](../semantisk-arkitektur-og-begrepskatalog/).
- technical interoperability is covered by the architecture choices in chapters 8 and 9.
- legal interoperability has three identified gaps – legal basis for primary school data, legal equivalence of digital diplomas, which must be clarified in work package 3 (AP3).
- organisational interoperability – a governance model for input to KREG from upper secondary education and primary and lower secondary school is lacking.
