---
id: e4e17e3b-131f-444a-a42c-196bfeabf4cb
title: "Konklusjon og veien videre"
linkTitle: "Konklusjon og veien videre"
weight: 50
lastmod: 2026-11-19T23:11:48+01:00
last_editor: Erik Hagen

---

## Konklusjon og veien videre

### De viktigste momentene

1. **Dagens NVB-import er en risiko i seg selv.** Et posisjonsbasert, proprietært filformat, et register som bare inneholder det som validerer, batch-forsinkelser i en tidskritisk kjede, og regelverkskompetanse som sitter implisitt i én regelmotor ([Risikomomenter i dagens prosess](../dagens-nvb-import/#risikomomenter-i-dagens-prosess)). Dette begrunner overgangen – ikke bare som modernisering, men som risikoreduksjon.
2. **Direkte integrasjon mot KREG er anbefalt flyt** ([Arkitekturskisser for piloten](../maalbilde-og-anbefalt-arkitektur/#arkitekturskisser-for-piloten)) – med formell beslutning hos systemeierne som første steg videre.
3. **Validering og innsending er to separate konsepter** ([Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur)) – registeret speiler virkeligheten med eksplisitt kvalitetstilstand, i stedet for å holde avvik utenfor.
4. **Semantikken endres fra typekodede rader til en rekursiv, ELM-orientert modell** – for leverandørene et representasjonsskifte mer enn en feltmapping ([Semantisk arkitektur](../semantisk-arkitektur-og-begrepskatalog/#semantisk-arkitektur)).
5. **Grunnskole og Utdanningsregisteret er dokumentert som egne spor** ([Grunnskole](../grunnskole/) og [Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur)).
6. **Realisering er innen rekkevidde** – ettårsperspektiv for KREG, utviklingsstart hos Novari innen ett år, og mulig bruk av SAMT-BU-midler inn i 2028, forutsatt finansierings- og prioriteringsbeslutninger hos partene.

### Veien videre etter 1. september

Rapporten leveres som pilotens milepæl «arkitektur og informasjonsmodell er ferdigstilt» og overleveres SAMT-BU.

Basert på arbeidsmøteserien anbefales følgende arkitektur for realiseringen:

1. **Direkte integrasjon mot KREG, ikke via FINT** – InSchool direkte mot KREGs API, VIGO via filoverføring med eventuell mellomvare inntil VIGO-modernisering ([Arkitekturskisser for piloten](../maalbilde-og-anbefalt-arkitektur/#arkitekturskisser-for-piloten)).
2. **Validering som egen komponent, adskilt fra innsending,** basert på Sikts ODM-tjeneste – om den skal tilbys som felles valideringskomponent for flere systemer, kost/nytte-vurderes ([Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur)).
3. **Filinnsendingen til NVB fases ut i sin helhet,** sammen med soltegnformatet, etter forutsetningene i [Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur) (sameksistens og utfasing).
4. **Utdanningsregisteret følges opp i eget spor** ([Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur)), slik at det verken blokkerer piloten eller bygges bort.
5. **Grunnskole tas videre som eget spor** etter forutsetningene under [Grunnskole](../grunnskole/).

Realiseringen kan følge tre faser. Faseinndelingen er et forslag og er ikke behandlet i arbeidsgruppen; innholdet i hver fase konkretiseres når systemeierne har tatt sin beslutning.

**Høst 2026 – avklaring.** Systemeierne beslutter integrasjonsmønsteret, og Sikt og Novari gjennomfører kartleggingene, starter API-dialogen og avklarer finansiering og prioritering.

**Frem mot sommer 2027 – forberedelse.** KREG gjøres klar til mottak, utvikling igangsettes hos Novari og leverandørene.

**Tidligst fra skoleåret 2027/2028 – realiseringspilot.** InSchool og VIGO leverer mot KREG for et avgrenset utvalg, med NVB i parallell på vilkårene i [Teknisk målarkitektur](../maalbilde-og-anbefalt-arkitektur/#teknisk-målarkitektur).

**Løpende.** De juridiske avklaringene i arbeidspakke 3 planlegges slik at de ikke blokkerer realiseringspiloten, grunnskolesporet tas videre etter forutsetningene under [Grunnskole](../grunnskole/), og forslagene om utdanningspersonregister og maskinlesbar validering meldes inn som kandidater til nye SAMT-BU-case.
