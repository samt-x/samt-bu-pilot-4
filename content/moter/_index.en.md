---
id: 3945b641-0fb4-4ddd-bdb9-61b7a81acece
title: "Meetings"
linkTitle: "Meetings"
weight: 18
lastmod: 2026-11-20T02:42:17+01:00
last_editor: Erik Hagen

---
The architecture work in pilot 4 was done in a series of working meetings from 22 May to 28 August 2026 (working meetings 1–8). The [architecture report](../leveranser/arkitektur-og-informasjonsmodell/) is based on them. Here are the minutes of the first two meetings. The other minutes are for now only in the project's area in Teams.

## Working meeting 1 – 22 May 2026

*First meeting in the series of working meetings on architecture.*

**Time:** 14:00–15:30\
**Participants:** Geir Magne Vangen, Sikt (chair) · Anne Kathrine Haugen, Sikt · Martin Skurtveit, Sikt · Christiane Frolich, Digdir · Kjersti Steien, Digdir · Anders Alstad, Novari

### Background and goal

This was the first in a series of working meetings on architecture for pilot 4 in SAMT-BU. The pilot shall lay the foundation for reporting individual results and achieved qualifications (diplomas), in terms of semantic and technical architecture. The [dream journey](../leveranser/drommereise/) guides the work.

The transition from paper documentation to digital evidence shall give citizens full control of their own information, make it possible for consumers (employers, the public sector and others) to digitalise and automate processes, and counter fraud.

### Data model

The meeting reviewed relevant information models and standards:

- CEN European Learner Mobility (ELM) and ELMO (XML format) – with core concepts such as Learner, Issuer, Learning Opportunity Specification, Learning Opportunity Instance, as well as support for multiple languages, extensions and attachments.
- The European Learning Model (ELM) was presented as a framework for learning data at European level.
- Other standards from 1EdTech were mentioned for reference: OneRoster, Edu-API, Comprehensive Learner Record and Open Badges.

For the Education Register (Utdanningsregisteret), the following were discussed:

- Definition of an education based on the ELM model.
- A learning opportunity as part of an institution's portfolio.
- An education instance offered at a given time and place.

The model for the Competence Register was presented, with the same overall model for education as in the Education Register, with additions for assessment.

During this review there was a brief discussion of how the content of these registers could be described in Data.norge.no, and of the further use of semantic technologies in this infrastructure. These are points worth taking into the further work.

### Architecture

Martin presented a technical architecture centred on the FS GraphQL API. The sketch was presented as Sikt's preliminary thinking at a very high level, and it was stressed that the details must be clarified during the project. The sketch shows:

- The school systems, with the data Sikt needs: individual results, qualifications, the national identity number of the result owner and the national identity number of the user delivering the data (for audit).
- Authentication against the FS GraphQL API through Maskinporten and Feide, possibly ID-porten via Feide.
- The registers for information about, storage of and sharing of education and certificates of competence: the Education Register, the Competence Register and Vitnemålsportalen (the Norwegian Diploma Registry) respectively.
- Links to international integrations such as EMREX, OOTS/SDG and the EUDI Wallet.

The discussion revealed that some of the data we want to store originates from the Grep and Vigo code lists, not from the school systems themselves. This must be followed up when we look more closely at the data model. There was also discussion of how the school systems deliver diplomas in municipalities and county authorities today (Vigo and FINT), and that this intermediary may affect how the architecture is finally designed. How these delivery chains are to be handled in the new architecture is an open question the project must address.

Sikt also briefly explained why they have invested so heavily in GraphQL and the FS GraphQL API. One consequence of this choice is that integrating parties must support and understand GraphQL, or alternatively that Sikt must offer a REST-to-FS-GraphQL bridge for those who cannot integrate directly. Which model applies to which parties must be clarified later in the project.

### Next steps

- Assess models used in primary, lower secondary and upper secondary schools.
- Continue work on proposals for how reporting should be implemented.
- The next meeting of the working group is Tuesday 2 June, 14:00–15:30.

## Working meeting 2 – 2 June 2026

*Second meeting in the series of working meetings on architecture.*

**Time:** 14:00–14:35\
**Participants:** Geir Magne Vangen, Sikt (chair) · Martin Skurtveit, Sikt · Christiane Frolich, Digdir · Kjersti Steien, Digdir · Håkon Søgnen, KS Digital · Truls Waage, Stavanger municipality · Jens Hansen, Stavanger municipality · Eivind Nilssen, Udir

### Agenda

The meeting was used for a high-level review of how Stavanger municipality handles diplomas and results for its primary and lower secondary schools. Novari's review of the solution for upper secondary education was moved to the next meeting.

### Stavanger municipality – diplomas

Truls Waage and Jens Hansen presented the solution.

The City Archives of Stavanger municipality have digitised everything received from the schools. This documentation has been scanned to PDFs and linked to the individual citizen. The documentation is presented in Min side, the citizen portal of Stavanger municipality (including data from Finnøy and Rennesøy, which merged with Stavanger municipality in 2020).

**Vigilo** (school administration system): At the end of schooling, diplomas are archived for long-term preservation. These are also PDFs. This has been done for the last three years.

**Vigo:** Diploma data is transferred to Vigo as structured data (CSV files), with the following content:

- national identity number, name
- subject code
- grade, participated, exempted
- the pupil's e-mail address
- (conduct/behaviour and absence are not part of this export to Vigo)

The information is delivered to Vigo in three rounds:

1. national identity number, class membership
2. grades for term 1 (New Year)
3. grades for term 2 (final result), which also include grades from year 9 (food and health)

The municipalities themselves commission development from Vigilo. There is a user council for Vigilo that collaborates on feedback to the vendor.

Vigilo is implementing the Norwegian profile of OneRoster 1.2 (1EdTech). Adjustments are still being made, but it is largely ready for use. More information about the profile is available under «OneRoster 1.2 Norwegian Profile» at IMS Global Learning Consortium (1EdTech).

### Next steps

- The topic for the next meeting will be the handling of results in upper secondary schools (school administration system, Vigo, possible reporting route to a national competence register).
- The minutes give «Friday 2 June, 13:00–14:30» as the time of the next meeting. That cannot be right, since this meeting itself was held on 2 June.
