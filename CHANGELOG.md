# Änderungsverlauf

Die Versionen sind aus den Markierungen im Quelltext rekonstruiert. Stände vor v9 sind nicht dokumentiert.

Datumsangaben fehlen, weil das Werkzeug ohne Versionsverwaltung entstanden ist. Ab jetzt bekommt jede Änderung einen Eintrag.

---

## v16 (aktueller Stand)

`schemaVersion: 16`

### Neu

* Anlage und Station sind getrennte Felder. Eine Anlage mit Stationen ist eine Linie, eine ohne ist ein Einzelplatz.
* Das Modul wird nur noch bei einer produktiven Schicht abgefragt, die Facharbeitertätigkeit bezieht sich auf die Anlage.
* Vorkommnisse führen Messwert, Einheit, Grenzwert, Status und den Vermerk, an wen gemeldet wurde.
* Die Maßnahme einer Wartung ist eine Liste statt eines einzelnen Textes.
* Bearbeiten-Funktion für Wartungen, Tätigkeiten und Vorkommnisse. Der Datensatz wird ins Formular zurückgeladen und an Ort und Stelle ersetzt, die ID bleibt erhalten.
* Migration von v15 auf v16 beim Laden, siehe README.
* Neuer Kopfbereich mit Statusleiste, Anleitung-Button und Umschalter für das dunkle Layout.
* Anlagen werden ausschließlich im Maschinen-Editor angelegt, die Erfassungsformulare erlauben nur noch Auswahl.

## v12

* Vorkommnisse als eigene Kategorie neben Wartung und Tätigkeit, mit eigener Tabelle, Suche und eigener Ebene im Kalender.

## v11

* Stabile IDs für alle Datensätze über `crypto.randomUUID()`.
* Zusammenführen zweier JSON-Stände mit Vergleichsplan und Konfliktauflösung je Datensatz.

## v10

* Tätigkeitserfassung mit Datum, Tätigkeit und optionaler Dauer, dazu die Kennzeichnung als produktive Schicht über den ganzen Tag.
* Tagesbackup in einen frei wählbaren Ordner, der Ordnerzugriff wird gemerkt.
* Der Ladebildschirm bietet die zuletzt benutzte Datei direkt zum Öffnen an.

## v9

* File System Access API: Speichern schreibt in dieselbe Datei zurück, ohne Dialog.
* Automatische Zwischensicherung in IndexedDB als Crash-Schutz, mit Wiederherstellungsangebot beim nächsten Start.
* Sichtbares Kennzeichen für ungespeicherte Änderungen.
