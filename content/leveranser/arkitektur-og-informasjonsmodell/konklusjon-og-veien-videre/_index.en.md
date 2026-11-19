---
id: e4e17e3b-131f-444a-a42c-196bfeabf4cb
title: "Conclusion and the way forward"
linkTitle: "Conclusion and the way forward"
weight: 50
lastmod: 2026-11-19T23:11:48+01:00
last_editor: Erik Hagen

---

## Conclusion and the way forward

### The most important points

1. **Today's NVB import is a risk in itself.** A position-based, proprietary file format, a register that only contains what validates, batch delays in a time-critical chain, and regulatory expertise that sits implicitly in a single rule engine ([Risk factors in today's process](../dagens-nvb-import/#risk-factors-in-todays-process)). This justifies the transition – not only as modernisation, but as risk reduction.
2. **Direct integration with KREG is the recommended flow** ([Architecture sketches for the pilot](../maalbilde-og-anbefalt-arkitektur/#architecture-sketches-for-the-pilot)) – with a formal decision by the system owners as the first next step.
3. **Validation and submission are two separate concepts** ([Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture)) – the register reflects reality with an explicit quality state, instead of keeping deviations outside.
4. **The semantics change from type-coded rows to a recursive, ELM-oriented model** – for the vendors, a shift in representation more than a field mapping ([Semantic architecture](../semantisk-arkitektur-og-begrepskatalog/#semantic-architecture)).
5. **Primary and lower secondary school and Utdanningsregisteret are documented as separate tracks** ([Primary and lower secondary school](../grunnskole/) and [Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture)).
6. **Implementation is within reach** – a one-year horizon for KREG, start of development at Novari within one year, and possible use of SAMT-BU funds into 2028, subject to funding and prioritisation decisions by the parties.

### The way forward after 1 September

The report is delivered as the pilot's milestone «architecture and information model completed» and handed over to SAMT-BU.

Based on the series of working meetings, the following architecture is recommended for the implementation:

1. **Direct integration with KREG, not via FINT** – InSchool directly against KREG's API, VIGO via file transfer with possible middleware until VIGO is modernised ([Architecture sketches for the pilot](../maalbilde-og-anbefalt-arkitektur/#architecture-sketches-for-the-pilot)).
2. **Validation as a separate component, separated from submission,** based on Sikt's ODM service – whether it should be offered as a common validation component for several systems is to be subject to a cost/benefit assessment ([Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture)).
3. **File submission to NVB is phased out entirely,** together with the «soltegn» format, according to the prerequisites in [Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture) (coexistence and phase-out).
4. **Utdanningsregisteret is followed up in a separate track** ([Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture)), so that it neither blocks the pilot nor is designed out.
5. **Primary and lower secondary school is taken further as a separate track** according to the prerequisites under [Primary and lower secondary school](../grunnskole/).

The implementation can follow three phases. The division into phases is a proposal and has not been discussed in the working group; the content of each phase will be specified once the system owners have made their decision.

**Autumn 2026 – clarification.** The system owners decide on the integration pattern, and Sikt and Novari carry out the mapping, start the API dialogue and clarify funding and prioritisation.

**Up to summer 2027 – preparation.** KREG is made ready to receive data, and development starts at Novari and the vendors.

**From the 2027/2028 school year at the earliest – implementation pilot.** InSchool and VIGO deliver to KREG for a limited selection, with NVB in parallel on the conditions in [Technical target architecture](../maalbilde-og-anbefalt-arkitektur/#technical-target-architecture).

**Ongoing.** The legal clarifications in work package 3 are planned so that they do not block the implementation pilot, the primary school track is taken further according to the prerequisites under [Primary and lower secondary school](../grunnskole/), and the proposals for an education person register (utdanningspersonregister) and machine-readable validation are submitted as candidates for new SAMT-BU cases.
