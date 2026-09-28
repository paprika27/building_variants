# Gebäudevarianten-Rechner

Ein Open-Source-Tool zur **parametrischen Planung, technischen Gegenüberstellung und Kostenschätzung von Gebäudevarianten**.

Das Tool verbindet Gebäudegeometrie, Bauteilaufbauten, Dämmstandards, Fenster und Türen, Heizlast, Wärmepumpe, Flächenheizung, PV und weitere technische Komponenten in einem gemeinsamen Mengengerüst.

Ziel ist nicht nur die Berechnung einzelner Kostenpositionen, sondern die **vergleichbare Modellierung kompletter Gebäudevarianten**.

---

## Motivation

Bei der frühen Gebäudeplanung werden technische Entscheidungen häufig isoliert betrachtet:

* Welche Wandkonstruktion ist günstiger?
* Wie viel Dämmung brauche ich?
* Wie groß muss die Wärmepumpe sein?
* Wie viel Fläche benötigt die Fußbodenheizung?
* Wie viele Fenster brauche ich?
* Wie verändert sich die Heizlast durch einen besseren U-Wert?
* Welche Variante benötigt wie viele Wandelemente?
* Welche Lösung lässt sich später sinnvoll automatisieren und steuern?

Diese Fragen hängen jedoch unmittelbar zusammen.

Eine bessere Außenwand verändert beispielsweise den Transmissionswärmeverlust. Dieser verändert die Heizlast. Die Heizlast beeinflusst die erforderliche Wärmepumpenleistung und die notwendige Heizfläche. Gleichzeitig verändern sich Materialmengen und Kosten.

Dieses Tool versucht deshalb, diese Zusammenhänge in einem **einheitlichen parametrischen Modell** abzubilden.

---

## Funktionen

### Variantenvergleich

Mehrere technische Varianten können parallel betrachtet werden.

Beispielsweise:

* Vorhangfassade Folie
* Gutex WDVS
* Gutex Vorhangfassade
* I-Träger gedämmt
* Vollholz-Ständer / KVH

Weitere Varianten können über das Datenmodell ergänzt werden.

---

### Gebäudegeometrie

Grundlegende geometrische Größen können als Parameter hinterlegt und für weitere Berechnungen verwendet werden:

* Grundfläche
* beheizte Geschossfläche
* Gebäudevolumen
* Wandlängen
* Wandhöhe
* Dachfläche
* Fensterflächen
* Türflächen

Aus diesen Größen werden weitere Mengen automatisch abgeleitet.

---

### Bauteile

Bauteile werden mit ihren technischen und wirtschaftlichen Eigenschaften beschrieben.

Beispielsweise:

* Dämmstärke
* U-Wert
* Bauteildicke
* Flächen
* Mengen
* Einheitspreise
* Montageaufwand

Dadurch können unterschiedliche Konstruktionsvarianten auf derselben geometrischen Grundlage verglichen werden.

---

### Raster und Wandelemente

Für Holz- und Elementbau können geometrische Raster berücksichtigt werden.

Beispielsweise:

* 62,5-cm-Raster
* 2,50-m-Elementhöhe
* Elementbreite
* Anzahl der Wandelemente
* Anzahl der Ständer
* Elementfläche

Damit lassen sich nicht nur Quadratmeterpreise vergleichen, sondern auch reale Mengen wie:

* Anzahl Wandelemente
* Anzahl Ständer
* Plattenflächen
* Montagevorgänge

ableiten.

---

### Fenster und Türen

Fenster und Türen können nach Typen modelliert werden.

Ein Typ kann beispielsweise enthalten:

* Anzahl
* Breite
* Höhe
* Fläche
* U-Wert
* g-Wert
* Einzelpreis

Die Gesamtfläche wird daraus automatisch berechnet.

Beispiel:

```text
6 × 0,90 × 1,20 m
+
4 × 1,20 × 1,50 m
=
13,68 m² Fensterfläche
```

Diese Fläche kann anschließend beispielsweise zur Berechnung der verbleibenden opaken Wandfläche verwendet werden.

---

## Heizlast

Das Tool kann eine **überschlägige Heizlast** aus den Gebäudeparametern ableiten.

Vereinfacht wird zwischen Transmissions- und Lüftungswärmeverlusten unterschieden:

```text
Heizlast =
    Wandverlust
  + Dachverlust
  + Bodenverlust
  + Fensterverlust
  + Türverlust
  + Lüftungswärmeverlust
  + Wärmebrücken
```

Für die Transmission gilt vereinfacht:

```text
Q = U × A × ΔT
```

Für den Lüftungswärmeverlust:

```text
Q = 0,34 × n × V × ΔT
```

mit:

* `U` = U-Wert in W/m²K
* `A` = Fläche in m²
* `n` = Luftwechsel in 1/h
* `V` = beheiztes Volumen in m³
* `ΔT` = Temperaturdifferenz in K

Die spezifische Heizlast wird anschließend als

```text
Heizlast / beheizte Fläche
```

angegeben.

### Wichtig

Die Heizlastberechnung des Tools ist als **Planungs- und Vergleichswerkzeug** gedacht.

Sie ersetzt keine normgerechte Heizlastberechnung durch eine Fachplanung.

Insbesondere Wärmebrücken, detaillierte Lüftungssituationen, Raumweise Heizlasten, angrenzende unbeheizte Bereiche und weitere Randbedingungen können eine detailliertere Berechnung erfordern.

---

## Flächenheizung

Die Heizlast kann mit der erforderlichen Heizfläche verknüpft werden.

Vereinfacht:

```text
erforderliche Heizfläche =
Heizlast / spezifische Heizleistung
```

Dadurch können beispielsweise folgende Größen miteinander verbunden werden:

* Heizlast
* verfügbare Heizfläche
* Heizleistung pro m²
* Rohrabstand
* Vorlauftemperatur
* Rücklauftemperatur
* Anzahl Heizkreise
* Rohrlänge

Das ermöglicht insbesondere die Betrachtung von Niedertemperatursystemen in Verbindung mit Wärmepumpen.

---

## Wärmepumpe und Haustechnik

Das Modell kann technische Eigenschaften einer Wärmepumpe aufnehmen, beispielsweise:

* Wärmequelle
* Heizleistung
* Modulationsbereich
* Auslegungstemperatur
* Vorlauftemperatur
* Pufferspeicher
* Warmwasserspeicher
* Bivalenzpunkt
* Heizstab

Zusätzlich können Anforderungen an die Automatisierung erfasst werden.

---

## Home Assistant und offene Schnittstellen

Ein Schwerpunkt des Modells ist die **Offenheit der Haustechnik**.

Neben einer einfachen Kompatibilitätsangabe können beispielsweise folgende Eigenschaften erfasst werden:

* Home-Assistant-Anbindung
* lokale API
* REST
* MQTT
* Modbus
* KNX
* SG Ready
* lokale Steuerbarkeit
* Cloud-Abhängigkeit

Dadurch soll nicht nur die Frage

> „Ist das Gerät smart?“

beantwortet werden, sondern:

> „Kann das Gerät lokal, offen und langfristig in ein eigenes Automatisierungssystem integriert werden?“

---

## Weitere Bauteile

Das Datenmodell kann unter anderem folgende Bereiche abbilden:

* Dach
* Außenwand
* Kellerboden
* Perimeterdämmung
* Horizontalsperre
* Fenster
* Türen
* Wintergarten
* Flächenheizung
* Wärmepumpe
* Sole-Erdkollektor / Grabensystem
* PV
* Elektrik
* Bivalenz / Heizstab

Die Struktur ist bewusst erweiterbar.

---

# Datenmodell

Die Anwendung basiert auf einem JSON-Datenmodell.

Vereinfacht:

```json
{
  "variants": [],
  "clusters": [],
  "values": {},
  "collapsed": {}
}
```

## Varianten

Varianten definieren unterschiedliche technische Lösungen.

```json
{
  "id": "kvh",
  "name": "Vollholz-Ständer (KVH)"
}
```

Eine weitere Variante kann beispielsweise sein:

```json
{
  "id": "i_traeger",
  "name": "I-Träger gedämmt"
}
```

---

## Cluster

Ein Cluster fasst zusammengehörige Größen zusammen.

Beispielsweise:

```text
Wand
Dach
Fenster
Flächenheizung
Haustechnik
PV
```

Jede Zeile besitzt unter anderem:

```json
{
  "id": "uwert",
  "label": "U-Wert",
  "unit": "W/m²K",
  "type": "value"
}
```

Mögliche Typen sind unter anderem:

* `value`
* `auto`
* `select`
* `grandtotal`

---

# Formeln

Berechnete Werte beginnen mit `=`.

Zellen können über

```text
Cluster::Zeile::Variante
```

referenziert werden.

Beispiel:

```text
=wand::material_menge::folie*wand::material_einzelpreis::folie
```

Ein weiteres Beispiel:

```text
=wand::flaeche_netto::folie*wand::uwert::folie*heizlast::temperaturdifferenz::folie
```

Dadurch können technische Größen über mehrere Cluster hinweg miteinander verknüpft werden.

---

# Kostenmodell

Kosten werden über `costTerms` definiert.

Beispiel:

```json
"costTerms": [
  [
    "material_menge",
    "material_einzelpreis"
  ],
  [
    "aufwand_einbau"
  ]
]
```

Dies bedeutet:

```text
Materialmenge × Einzelpreis
+
Einbau-Aufwand
=
Zwischensumme
```

Die Kostenlogik ist damit nicht fest in der Benutzeroberfläche programmiert, sondern Bestandteil des Datenmodells.

---

# Vergleichbarkeit

Ein wesentliches Ziel des Projekts ist die **Vergleichbarkeit technischer Lösungen**.

Dazu werden drei Ebenen miteinander verbunden:

### 1. Technische Anforderungen

Beispiele:

* Ziel-U-Werte
* Heizlast
* Vorlauftemperatur
* Heizleistung
* Schnittstellen
* lokale Steuerbarkeit

### 2. Normierte Mengen

Beispiele:

* m² Wand
* m² Dach
* m² Fenster
* Wandelemente
* Rasterfelder
* Heizkreise
* Meter Rohr
* m³ Dämmstoff

### 3. Kosten

Beispiele:

```text
Menge × Einheitspreis
+
Montage
+
Nebenleistungen
```

Damit soll verhindert werden, dass Varianten nur anhand eines einzelnen Quadratmeterpreises miteinander verglichen werden.

---

# Status

Das Projekt befindet sich in Entwicklung.

Insbesondere folgende Bereiche können weiter verbessert werden:

* detailliertere Heizlastberechnung
* raumweise Heizlast
* Wärmebrücken
* detaillierte Heizflächenberechnung
* Fenster-/Türtypen
* Bauteilaufbauten
* Materialdatenbank
* Lebenszykluskosten
* CO₂-Bilanz
* Fördermittel
* technische Schnittstellen
* Import und Export von Projekten
* Szenarien und Sensitivitätsanalysen

---

# Hosting

Die Anwendung kann bei einer rein clientseitigen Architektur direkt über **GitHub Pages** veröffentlicht werden.

Typischer Aufbau:

```text
GitHub Repository
       │
       ├── source code
       ├── JSON-Datenmodell
       ├── README
       └── GitHub Actions
               │
               ▼
          GitHub Pages
               │
               ▼
        öffentliches Web-Tool
```

Bei Anwendungen ohne Backend ist kein eigener Server erforderlich.

Für Anwendungen mit:

* Datenbank
* Benutzerkonten
* serverseitiger Berechnung
* privaten Projekten
* API-Schlüsseln

ist zusätzlich ein Backend bzw. ein anderer Hosting-Dienst erforderlich.

---

# Open Source

```text
MIT License

Copyright (c) [YEAR] [NAME]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files, to deal in the Software
without restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, and/or sell copies of the
Software...
```
---

# Mitmachen

Beiträge sind willkommen.

Mögliche Beiträge:

* neue Bauteilmodelle
* weitere Konstruktionsvarianten
* bessere Berechnungsmodelle
* neue Schnittstellen
* UI/UX-Verbesserungen
* Dokumentation
* Tests
* Fehlerberichte

Für größere Änderungen empfiehlt sich zunächst ein Issue, damit das Datenmodell und die Berechnungslogik gemeinsam abgestimmt werden können.

---

# Haftungsausschluss

Das Tool dient der **Orientierung, Variantenbildung und überschlägigen Kosten- und Technikbetrachtung**.

Berechnete Werte stellen keine automatisch geprüfte Ausführungsplanung, Statik, Energieberatung, Heizlastberechnung oder Fachplanung dar.

Insbesondere bei sicherheits-, bauordnungs-, energie- oder haustechnisch relevanten Entscheidungen sind die jeweils erforderlichen Fachplanungen und Nachweise einzuholen.
