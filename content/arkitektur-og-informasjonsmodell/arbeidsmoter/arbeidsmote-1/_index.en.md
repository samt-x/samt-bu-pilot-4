---
id: 85e2e51d-1d14-407f-845a-92d9ace9b00a
title: "Working meeting 1 – 22 May 2026"
linkTitle: "Working meeting 1"
weight: 10
lastmod: 2026-11-19T21:36:39+01:00
last_editor: Erik Hagen

---
*Minutes of the first meeting in the series of working meetings on architecture for pilot 4.*

**Date:** 22 May 2026, 14:00–15:30\
**Project:** SAMT-BU, pilot 4

**Participants:**

- Geir Magne Vangen, Sikt (chair)
- Anne Kathrine Haugen, Sikt
- Martin Skurtveit, Sikt
- Christiane Frolich, Digdir
- Kjersti Steien, Digdir
- Anders Alstad, Novari

## Background and goal

This was the first in a series of working meetings on architecture for pilot 4 in SAMT-BU. The pilot shall lay the foundation for reporting individual results and achieved qualifications (diplomas), in terms of semantic and technical architecture.

The «dream journey» that guides the work:

- All diplomas and individual results from education are gathered in the Competence Register and are available throughout life.
- Data from the systems of municipalities and county authorities is delivered to the Competence Register so that results can be shared and kept securely.
- Digital results shall be legally equivalent to paper documentation.
- Public agencies and private businesses can retrieve education results easily – with a legal basis or the citizen's consent.
- Citizens have all results and diplomas gathered in one place, shareable via the Diploma Portal (Vitnemålsportalen).

The transition from paper documentation to digital evidence shall give citizens full control of their own information, make it possible for consumers (employers, the public sector and others) to digitalise and automate processes, and counter fraud.

## Data model

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

## Architecture

Martin presented a technical architecture centred on the FS GraphQL API. The sketch was presented as Sikt's preliminary thinking at a very high level, and it was stressed that the details must be clarified during the project. The sketch shows:

- The school systems, with the data Sikt needs: individual results, qualifications, the national identity number of the result owner and the national identity number of the user delivering the data (for audit).
- Authentication against the FS GraphQL API through Maskinporten and Feide, possibly ID-porten via Feide.
- The registers for information about, storage of and sharing of education and certificates of competence: the Education Register, the Competence Register and the Diploma Portal respectively.
- Links to international integrations such as EMREX, OOTS/SDG and the EUDI Wallet.

The discussion revealed that some of the data we want to store originates from the Grep and Vigo code lists, not from the school systems themselves. This must be followed up when we look more closely at the data model. There was also discussion of how the school systems deliver diplomas in municipalities and county authorities today (Vigo and FINT), and that this intermediary may affect how the architecture is finally designed. How these delivery chains are to be handled in the new architecture is an open question the project must address.

Sikt also briefly explained why they have invested so heavily in GraphQL and the FS GraphQL API. One consequence of this choice is that integrating parties must support and understand GraphQL, or alternatively that Sikt must offer a REST-to-FS-GraphQL bridge for those who cannot integrate directly. Which model applies to which parties must be clarified later in the project.

## Next steps

- Assess models used in primary, lower secondary and upper secondary schools.
- Continue work on proposals for how reporting should be implemented.
- The next meeting of the working group is Tuesday 2 June, 14:00–15:30.
