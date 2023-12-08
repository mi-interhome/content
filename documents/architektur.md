---
title: Architektur
index: 2
---

Schichten bzw. Ringarchitektur: Abhängigkeiten zwischen Paketen immer nach unten/innen gerichtet

### package `resources`

Dieses Paket enthält statische Ressourcen, die mit embed.FS eingebunden werden, z.B. die Definition der Suchfilter (_config-filter.json_).

### package `webcc`

Basistypen wie `domain`, `company`, `language`, `locale` sind hier definiert.

- Interne Abhängigkeiten: `resources`

### package `urls`

Zentrales Paket mit Funktionen zum parsen einer übergebenen URL (_parser.go_ hier URLParser.Parse()) und Erzeugung einer Datenstruktur zur Ausgabe (_generator.go_)

- Interne Abhängigkeiten: `webcc`

### package `seo`

Seo-Funktionalität wie Canonicals und Alternates

- Interne Abhängigkeiten: `webcc`, `urls`, `resources`

### package `usecase`

Usecase-Funktionalität wird direkt von den HTTP-Handlern aufgerufen.
G4-Pattern: Facade: Highlevel-Verknüpfung von Basis-Services

- URL-Parsen
- Aufruf von spezifischen Funktionen für die angeforderte Seite (page)
- aber auch nicht-Seiten-Usecases wie z.B. TR-Konfiguration, u.ä.

### package `page`

Kümmert sich ausschließlich um Seitenkonfigurationen. Auch leere Seiten, die nur Domain und Site-Daten enthalten.
Jeder Page-Type hat eigenen Handler.

- **Caching**: je nach Seitentyp unterschiedlich gelöst. Mehr Aufwand bei frequentierten Seiten wie Suchen, Details und Start. Diese sollten 'en block' gecached werden.
  Andere Seiten können auch live aus Inhaltsblöcken zusammengesetzt werden.
