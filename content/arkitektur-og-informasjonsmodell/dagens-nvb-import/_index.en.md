---
id: 70eb49eb-f182-4c2e-afe5-060d4e352084
title: "Today's process for NVB import"
linkTitle: "Today's NVB import"
weight: 10
---

## 5. Today's process for NVB import

To understand what the new architecture changes, today's process must be described. This chapter documents how results from upper secondary education currently enter the National Diploma Database (Nasjonal vitnemålsdatabase, NVB), and which risk factors lie in this process. The description builds on the format specification for the NVB control engine [5], the reviews in the series of working meetings [12] and NVB's own documentation [6].

### 5.1 Actors and data flow

NVB is managed by Sikt and is today the source of truth for diplomas (vitnemål) and certificates of competence (kompetansebevis) from upper secondary education, used primarily by Samordna opptak (the Norwegian Universities and Colleges Admission Service) for admission to higher education [6].

The data flow into NVB is as follows (figure 1):

1. **The school registers final assessments in the school administration system.** For public upper secondary schools this is InSchool; private schools use, among others, iSkole and IST Everyday [17]. The teacher sets the overall achievement grade, and examination grades are added after marking.
2. **The school administration system generates diploma and certificate of competence data** as a character-separated text file («soltegnfil», see 4.2) according to the NVB specification [5].
3. **Vocational training (fagopplæringen) goes via VIGO.** Trade examinations are registered in VIGO, which produces the final diploma for apprentices and candidates in vocational training. VIGO exports a file in .nvb format which is later loaded into NVB again [12: working meetings 3 and 7].
4. **The file is validated before submission** (see 5.3 on how validation is done today).
5. **NVB accepts documents that validate without errors.** Documents with error messages are rejected and returned for correction [5]. NVB does not accept diplomas that do not validate [12: working meeting 8].
6. **The school owner issues and signs the diploma on paper.** There is currently no digital signing of diplomas – the diploma is printed and signed by the head teacher, in addition to the data being submitted and validated [12: working meeting 8].

Primary and lower secondary school is not part of this flow. Primary school diplomas are stored as PDF in the pupil's file in the municipality's case management and archive system, and there is currently no way of extracting the data [12: working meetings 4 and 6]. See [chapter 10](../grunnskole/).

![Today's data flow into the National Diploma Database: upper secondary schools via InSchool and vocational training via Vigo deliver a ¤ file (.nvb), which is validated in the validation service (ODM) and submitted to NVB, which rejects documents that do not validate; Samordna opptak uses NVB. Primary school diplomas are stored as PDF in the case management and archive system and are not part of the flow (figure in Norwegian)](dagens-dataflyt-nvb.png)

*Figure 1: Today's data flow into the National Diploma Database.*

### 5.2 The file format – the «soltegn» files

The exchange format is a line-based text file in which the fields are separated by the «soltegn» character (¤, the currency sign), and in which the row type is determined by a type letter at the start of the line [5]:

| Row type | Content | Counterpart in NVB's data model |
|---|---|---|
| ¤A | Start line per file section: organisation number, school number (VIGO), counters | Filimport (file import) |
| ¤S | School information (optional): name, address, contact person | Enhetsregister (entity register) |
| ¤V | Document header for diploma or certificate of competence: document identifier, document type (VGDOKTYPEKODE: VM = vitnemål/diploma, KB = kompetansebevis/certificate of competence), national identity number, name as free text, year of completion, reform (KL/R94) | VGDOK |
| ¤P | One line per programme area (normally three for a diploma): programme area code, level code (VG1–VG3), assertion code (Bestått / Fullført / Fullført og bestått – passed / completed / completed and passed) | VGDOKPROMR |
| ¤F | One line per subject: subject code (Grep), subject type (FF/FP/VP), overall achievement and examination grade, scope, examination term, subject remark | VGDOKFAG |
| ¤M | Remarks on the document or individual subjects, with VMM remark codes from Grep | VGDOKMERKNAD |
| ¤D | Cancellation of a previously issued document, with reason code | – |

The format is position-based: the meaning of a field is determined by its column position in the line, not by field names. In practice the file is four different row schemas multiplexed into one stream, where a shared key (the diploma number) links the subject and remark lines to the document header. The document type is controlled by a code value (VGDOKTYPEKODE) set by the vendor – diplomas and certificates of competence are rows in the same table, distinguished only by the code value. This choice of representation is the core of the semantic shift to KREG, see [chapter 11](../semantisk-arkitektur-og-begrepskatalog/).

Sikt wants to phase out the «soltegn» files as a format and to remove file submission to NVB entirely; how the phase-out is to take place is described in [chapter 9](../maalbilde-og-anbefalt-arkitektur/) (coexistence and phase-out). Digdir's architecture principle 4, recommendation 4.5, warns precisely against locking data into systems with their own formats [2].

### 5.3 Validation and control today

Validation checks that the combination of subjects in a document meets the requirements for a diploma, including the requirements for general university admissions certification (GSK), against code lists from Grep, Udir's guidelines for completing diplomas (føringsskrivet) and the circular on the distribution of subjects and teaching hours [5][10]. The check is also used along the way: from the autumn of the final school year it is run to check that the diploma will validate, which is important for giving pupils confidence that they will get their diploma in the end [12: working meeting 6].

Historically this was done with kontroll.exe, a binary command-line application distributed per operating system, run locally or against a central server at Samordna opptak [5]. Kontroll.exe has now been discontinued and replaced by Sikt's validation service based on IBM ODM. In InSchool, validation is built into the diploma generation itself (subjects against diploma number) and is used for all diplomas; in VIGO the control engine is used only for the trade examinations when a diploma is ready. The requirements for final assessment and documentation follow from the regulations to the Education Act (forskrift til opplæringslova); Udir manages the guidelines that operationalise how diplomas and certificates of competence are completed (a draft regulation on completing them, føringsforskrift, is under consideration in the Ministry of Education and Research), and the school owner is responsible for issuing them. The expertise on the checks nevertheless sits implicitly with Sikt today, through the ODM implementation [12: working meeting 8].

### 5.4 Risk factors in today's process

The review in the series of working meetings has identified the following risk factors in today's NVB import. They form a substantial part of the justification for the new architecture.

**R1 – Lock-in to a proprietary format.** The position-based ¤ format without field names is fragile, requires specialist expertise at each vendor, and locks data into a system-specific format, contrary to Digdir's recommendation 4.5 [2][5].

**R2 – The register is incomplete.** NVB only accepts documentation that validates. Documentation that does not validate – including from module-structured education for adults, which amounts to hundreds to several thousand certificates a year – exists only on paper or as unvalidated documents outside the register [12: working meeting 4]. Pupils who do not complete do not get their results in either.

**R3 – Manual errors in the files.** When schools send files, grades may be missing or subject remarks may be entered incorrectly. The current format also allows only one subject remark per subject [12: working meeting 3].

**R4 – Batch delay in a time-critical chain.** There is very little time between the marking deadline (around 19 June) and the admission run at the beginning of July. File-based transfer means that in practice the results cannot be retrieved from the register during this period, resulting in manual follow-up work [12: working meeting 3]. Note that KREG will not be able to serve the admission chain in the near future either – today's direct flow to admission continues; the risk applies to all other consumers of the results.

**R5 – Round trip in the vocational training flow.** Each transformation step in the round trip VIGO → .nvb file → NVB (cf. 5.1) is a source of errors, and the flow has not been fully mapped [12: working meeting 7].

**R6 – Regulatory expertise and management are unevenly distributed.** The owner of the regulations (Udir) and those who understand and maintain the implemented validation rules (Sikt) are not the same party (cf. 5.3), and the validation service does not necessarily pick up all changes to the rules [12: working meeting 8].

**R7 – Structured primary school results are not preserved.** Structured results exist only temporarily in the school system and are lost as structure when the diploma is archived as PDF; there is no national access, and reuse requires manual deliveries from each municipality [12: working meetings 2 and 4].

**R8 – Weak semantics for person and content.** Personal names are entered as free text in the document header, and the meaning of fields is determined by position. The format gives no structural guarantee that the content is interpreted the same way by sender and recipient [5][15].

**R9 – Vendor and technology lock-in in the control step.** Kontroll.exe was a closed binary; the transition to ODM has reduced, but not removed, the lock-in. Digdir's recommendation 5.6 points to avoiding dependence on individual vendors [3]. See the recommendation on machine-readable validation rules in [chapter 9](../maalbilde-og-anbefalt-arkitektur/).

**R10 – Paper is the only legally valid form.** The school owner's signature on paper is what makes the diploma valid. Digital results do not have equal legal standing, and this limits the value of the register as a source. Followed up in work package 3 [12: working meeting 8][13].

**R11 – Data disappears when organisations cease to exist.** For private tertiary vocational schools (fagskoler), documentation currently disappears when the school goes bankrupt. This illustrates the risk of decentralised storage of documentation in general, and is part of the justification for a central register throughout the whole life course. Private tertiary vocational schools are outside Novari's responsibility; follow-up of these must be placed with Sikt or another party [12: working meeting 8].

**R12 – Changes and cancellations are poorly supported.** Overall achievement and examination grades are individual administrative decisions with a right of appeal; if an appeal is upheld, the grade is changed and a new document is issued. Today this is handled through cancellation lines (¤D) and a new submission. The life cycle changed final assessment → cancelled/replaced document must be handled explicitly in the KREG model; this is listed as an unresolved point in the mapping in [chapter 11](../semantisk-arkitektur-og-begrepskatalog/).
