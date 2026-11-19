---
title: "Konklusjon og veien videre"
linkTitle: "Konklusjon og veien videre"
weight: 50
---

## 13. Konklusjon og veien videre

### 13.1 De viktigste momentene

1. **Dagens NVB-import er en risiko i seg selv.** Et posisjonsbasert, proprietært filformat, et register som bare inneholder det som validerer, batch-forsinkelser i en tidskritisk kjede, og regelverkskompetanse som sitter implisitt i én regelmotor ([kapittel 5](../dagens-nvb-import/)). Dette begrunner overgangen – ikke bare som modernisering, men som risikoreduksjon.
2. **Direkte integrasjon mot KREG er anbefalt flyt** ([kapittel 8](../maalbilde-og-anbefalt-arkitektur/)) – med formell beslutning hos systemeierne som første steg videre.
3. **Validering og innsending er to separate konsepter** ([kapittel 9](../maalbilde-og-anbefalt-arkitektur/)) – registeret speiler virkeligheten med eksplisitt kvalitetstilstand, i stedet for å holde avvik utenfor.
4. **Semantikken endres fra typekodede rader til en rekursiv, ELM-orientert modell** – for leverandørene et representasjonsskifte mer enn en feltmapping ([kapittel 11](../semantisk-arkitektur-og-begrepskatalog/)).
5. **Grunnskole og Utdanningsregisteret er dokumentert som egne spor** ([kapittel 10](../grunnskole/) og [9](../maalbilde-og-anbefalt-arkitektur/)).
6. **Realisering er innen rekkevidde** – ettårsperspektiv for KREG, utviklingsstart hos Novari innen ett år, og mulig bruk av SAMT-BU-midler inn i 2028, forutsatt finansierings- og prioriteringsbeslutninger hos partene.

### 13.2 Veien videre etter 1. september

Rapporten leveres som pilotens milepæl «arkitektur og informasjonsmodell er ferdigstilt» og overleveres SAMT-BU.

Basert på arbeidsmøteserien anbefales følgende arkitektur for realiseringen:

1. **Direkte integrasjon mot KREG, ikke via FINT** – InSchool direkte mot KREGs API, VIGO via filoverføring med eventuell mellomvare inntil VIGO-modernisering ([kapittel 8](../maalbilde-og-anbefalt-arkitektur/)).
2. **Validering som egen komponent, adskilt fra innsending,** basert på Sikts ODM-tjeneste – om den skal tilbys som felles valideringskomponent for flere systemer, kost/nytte-vurderes ([kapittel 9](../maalbilde-og-anbefalt-arkitektur/)).
3. **Filinnsendingen til NVB fases ut i sin helhet,** sammen med soltegnformatet, etter forutsetningene i [kapittel 9](../maalbilde-og-anbefalt-arkitektur/) (sameksistens og utfasing).
4. **Utdanningsregisteret følges opp i eget spor** ([kapittel 9](../maalbilde-og-anbefalt-arkitektur/)), slik at det verken blokkerer piloten eller bygges bort.
5. **Grunnskole tas videre som eget spor** etter forutsetningene i [kapittel 10](../grunnskole/).

Realiseringen kan følge tre faser. Faseinndelingen er et forslag og er ikke behandlet i arbeidsgruppen; innholdet i hver fase konkretiseres når systemeierne har tatt sin beslutning.

**Høst 2026 – avklaring.** Systemeierne beslutter integrasjonsmønsteret, og Sikt og Novari gjennomfører kartleggingene, starter API-dialogen og avklarer finansiering og prioritering.

**Frem mot sommer 2027 – forberedelse.** KREG gjøres klar til mottak, utvikling igangsettes hos Novari og leverandørene.

**Tidligst fra skoleåret 2027/2028 – realiseringspilot.** InSchool og VIGO leverer mot KREG for et avgrenset utvalg, med NVB i parallell på vilkårene i [kapittel 9](../maalbilde-og-anbefalt-arkitektur/).

**Løpende.** De juridiske avklaringene i arbeidspakke 3 planlegges slik at de ikke blokkerer realiseringspiloten, grunnskolesporet tas videre etter forutsetningene i [kapittel 10](../grunnskole/), og forslagene om utdanningspersonregister og maskinlesbar validering meldes inn som kandidater til nye SAMT-BU-case.
